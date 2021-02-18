---
title: Dépanner des utilisateurs orphelins
description: Les utilisateurs orphelins se produisent lorsque la connexion d’un utilisateur de la base de données n’existe plus dans la base de données Master. Cette rubrique explique comment identifier et dépanner les utilisateurs orphelins.
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 2583e238295c87b555c43f25b7fd68b03f3647c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354036"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>Dépanner des utilisateurs orphelins (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  L’apparition d’utilisateurs orphelins dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se produit lorsqu'un utilisateur de base de données est basé sur un utilisateur dans la base de données **MASTER** , mais que l’utilisateur n’existe plus dans le **MASTER**. Cela peut se produire lorsque l’utilisateur est supprimé, ou lorsque la base de données est déplacée vers un autre serveur sur lequel l’utilisateur n'existe pas. Cette rubrique décrit comment rechercher des utilisateurs orphelins, puis comment les remapper à des utilisateurs.  
  
> [!NOTE]  
>  Réduisez la possibilité d’apparition d’utilisateurs orphelins en utilisant des utilisateurs de base de données autonome pour les bases de données pouvant être déplacées. Pour plus d’informations, voir [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="background"></a>Arrière-plan  
 Pour connecter une base de données à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec un principal de sécurité (identité de l’utilisateur de base de données) basé sur un utilisateur, le principal doit disposer d’un identifiant valide dans la base de données **master** . Cette connexion est utilisée dans le processus d'authentification chargé de vérifier l’identité du principal pour s’assurer que le principal est autorisé à se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’une instance de serveur sont visibles dans l’affichage catalogue **sys.server_principals** et l’affichage de compatibilité **sys.syslogins** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accèdent aux bases de données en tant qu’utilisateur de base de données mappé à l’utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il y a trois exceptions à cette règle :  
  
-   Utilisateurs de base de données autonome  
  
     Les utilisateurs de base de données autonome s’authentifient au niveau base de données d’utilisateurs et ne sont pas associés aux utilisateurs. Cela est recommandé, car les bases de données sont plus portables, et les utilisateurs de base de données autonome ne peuvent ainsi pas devenir orphelins. Cependant, ils doivent être recréés pour chaque base de données. Cela pourrait être peu pratique dans un environnement avec plusieurs bases de données.  
  
-   Le compte **invité** .  
  
     Lorsqu'il est activé dans la base de données, ce compte autorise les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non mappées à un utilisateur de base de données à accéder en tant qu' **invité** à la base de données. Le compte **invité** est désactivé par défaut.  
  
-   Appartenance aux groupes Microsoft Windows.  
  
     Une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'un utilisateur Windows peut accéder à la base de données si cet utilisateur est membre d'un groupe Windows lui-même utilisateur de la base de données.  
  
 Les informations relatives au mappage d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un utilisateur de base de données sont stockées dans la base de données. Elles incluent le nom de l'utilisateur de base de données et l'identificateur de sécurité de connexion (SID) de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondante. Les autorisations de cet utilisateur de base de données sont appliquées comme autorisation de la base de données.  
  
 Un utilisateur de base de données (basé sur un identifiant) pour lequel la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondante n’est pas définie sur une instance serveur, ou l’est de façon incorrecte, ne peut pas se connecter à cette instance. L'utilisateur devient donc un *utilisateur orphelin* de la base de données sur cette instance du serveur. Le fait de se retrouver orphelin peut se produire si l'utilisateur de base de données est mappé à un SID de connexion absent dans l’instance `master` . Un utilisateur peut devenir orphelin après qu'une base de données a été restaurée ou attachée à une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle l’identifiant n’a jamais été créé. Un utilisateur de base de données peut également se retrouver orphelin si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondante est supprimée. Même si l’utilisateur est recréé, il aura un SID différent. Ainsi, l'utilisateur de base de données reste orphelin.  
  
## <a name="detect-orphaned-users"></a>Détecter des utilisateurs orphelins  

**Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et PDW**

Pour détecter des utilisateurs orphelins dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction des connexions d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] manquantes, exécutez l’instruction suivante dans la base de données utilisateur :  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 Vous obtenez la liste des utilisateurs d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de leurs identificateurs de sécurité (SID) qui, dans la base de données active, ne sont liés à aucune connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

**Pour SQL Database et Azure Synapse Analytics**

La table `sys.server_principals` n’est pas disponible dans SQL Database ou Azure Synapse Analytics. Pour identifier les utilisateurs orphelins dans ces environnements, procédez comme suit :

1. Connectez-vous à la base de données `master` et sélectionnez les SID pour les connexions avec la requête suivante :
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. Connectez-vous à la base de données utilisateur et passez en revue les SID des utilisateurs dans la table `sys.database_principals` , à l’aide de la requête suivante :

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. Comparez les deux listes pour déterminer si des SID dans la table `sys.database_principals` de la base de données utilisateur n’ont aucun SID de connexion correspondant dans la table `sql_logins` de la base de données master. 
  
## <a name="resolve-an-orphaned-user"></a>Résoudre le cas d'un utilisateur orphelin  
Dans la base de données master, utilisez l’instruction [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) avec l’option SID pour recréer un identifiant manquant, en fournissant le `SID` de l’utilisateur de base de données obtenu dans la section précédente :  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 Pour mapper un utilisateur orphelin à un identifiant qui existe déjà dans **master**, exécutez l’instruction [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) dans la base de données utilisateur en spécifiant le nom d’identifiant.  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 Lorsque vous recréez un identifiant manquant, l’utilisateur peut accéder à la base de données avec le mot de passe fourni. L’utilisateur peut ensuite modifier le mot de passe du compte avec l’instruction ALTER LOGIN.  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  N’importe quel utilisateur peut modifier son propre mot de passe. Seuls les utilisateurs avec l’autorisation `ALTER ANY LOGIN` peuvent modifier le mot de passe d’un autre utilisateur. Toutefois, seuls les membres du rôle **sysadmin** peuvent modifier les mots de passe des membres du rôle **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  

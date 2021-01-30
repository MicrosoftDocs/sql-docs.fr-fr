---
description: sp_migrate_user_to_contained (Transact-SQL)
title: sp_migrate_user_to_contained (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_migrate_user_to_contained
- sp_migrate_user_to_contained_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_migrate_user_to_contained
ms.assetid: b3a49ff6-46ad-4ee7-b6fe-7e54213dc33e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81450361252fcdc2d02a9d9e118839db3c5af64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195486"
---
# <a name="sp_migrate_user_to_contained-transact-sql"></a>sp_migrate_user_to_contained (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Convertit un utilisateur de la base de données mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en utilisateur de base de données autonome avec mot de passe. Dans une base de données autonome, utilisez cette procédure pour supprimer les dépendances sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où la base de données est installée. **sp_migrate_user_to_contained** sépare l’utilisateur de la connexion d’origine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , afin que les paramètres tels que le mot de passe et la langue par défaut puissent être administrés séparément pour la base de données à relation contenant-contenu. **sp_migrate_user_to_contained** peut être utilisé avant de déplacer la base de données à relation contenant-contenu vers une autre instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pour éliminer les dépendances sur les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions d’instance actuelles.  
  
> [!NOTE]
> Soyez prudent lorsque vous utilisez **sp_migrate_user_to_contained**, car vous ne pourrez pas inverser l’effet. Cette procédure est utilisée uniquement dans une base de données à relation contenant-contenu. Pour plus d’informations, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_migrate_user_to_contained [ @username = ] N'user' ,   
    [ @rename = ] { N'copy_login_name' | N'keep_name' } ,   
    [ @disablelogin = ] { N'disable_login' | N'do_not_disable_login' }   
```  
  
## <a name="arguments"></a>Arguments  
 [**@username =** ] **N'**_utilisateur_*_'_*  
 Nom d'un utilisateur dans la base de données autonome actuelle mappée à un compte de connexion authentifié [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur est de **type sysname**, avec **null** comme valeur par défaut.  
  
 [**@rename =** ] **N'**_copy_login_name_*_'_*  |  **N'**_keep_name_*_'_*  
 Lorsqu’un utilisateur de base de données basé sur un compte de connexion a un nom d’utilisateur différent de celui de la connexion, utilisez *keep_name* pour conserver le nom d’utilisateur de la base de données pendant la migration. Utilisez *copy_login_name* pour créer le nouvel utilisateur de base de données à relation contenant-contenu avec le nom de la connexion, au lieu de l’utilisateur. Lorsqu'un utilisateur de la base de données basé sur un compte de connexion a le même nom d'utilisateur que le nom de connexion, les deux options créent l'utilisateur de base de données autonome sans modifier le nom.  
  
 [**@disablelogin =** ] **N'**_disable_login_*_'_*  |  **N'**_do_not_disable_login_*_'_*  
 *disable_login* désactive la connexion dans la base de données Master. Pour vous connecter lorsque la connexion est désactivée, la connexion doit fournir le nom de la base de données à relation contenant-contenu comme **catalogue initial** dans le cadre de la chaîne de connexion.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_migrate_user_to_contained** crée l’utilisateur de base de données à relation contenant-contenu avec le mot de passe, quelles que soient les propriétés ou les autorisations de la connexion. Par exemple, la procédure peut être effectuée si la connexion est désactivée ou si l’utilisateur se voit refuser l’autorisation de **connexion** à la base de données.  
  
 **sp_migrate_user_to_contained** a les restrictions suivantes.  
  
-   Le nom d'utilisateur ne doit pas déjà exister dans la base de données.  
  
-   Les utilisateurs intégrés, par exemple dbo et guest, ne peuvent pas être convertis.  
  
-   L’utilisateur ne peut pas être spécifié dans la clause **Execute As** d’une procédure stockée signée.  
  
-   L’utilisateur ne peut pas posséder une procédure stockée qui comprend la clause **Execute As owner** .  
  
-   **sp_migrate_user_to_contained** ne peut pas être utilisé dans une base de données système.  
  
## <a name="security"></a>Sécurité  
 Lorsque vous migrez des utilisateurs, veillez à ne pas désactiver ou supprimer tous les comptes de connexion de l'administrateur dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si toutes les connexions sont supprimées, consultez [se connecter à SQL Server lorsque les administrateurs système sont verrouillés](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md).  
  
 Si la connexion **BUILTIN\Administrateurs** est présente, les administrateurs peuvent se connecter en démarrant leur application à l’aide de l’option **exécuter en tant qu’administrateur** .  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’autorisation **CONTROL SERVER** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-migrating-a-single-user"></a>R. Migration d'un seul utilisateur  
 L'exemple suivant migre un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommé `Barry`, vers un utilisateur de base de données autonome avec mot de passe. L’exemple ne modifie pas le nom d’utilisateur et conserve la connexion activée.  
  
```sql  
sp_migrate_user_to_contained   
@username = N'Barry',  
@rename = N'keep_name',  
@disablelogin = N'do_not_disable_login' ;  
  
```  
  
### <a name="b-migrating-all-database-users-with-logins-to-contained-database-users-without-logins"></a>B. Migration de tous les utilisateurs de la base de données avec des comptes de connexion vers des utilisateurs de base de données autonome sans comptes de connexion  
 L'exemple suivant migre tous les utilisateurs basés sur des comptes de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers des utilisateurs de base de données autonome avec mots de passe. L'exemple exclut les comptes de connexion qui ne sont pas activés. L'exemple doit être exécuté dans la base de données autonome.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Migrer vers une base de données partiellement autonome](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)  
  
  

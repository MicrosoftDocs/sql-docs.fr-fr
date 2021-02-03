---
title: REVOKE - Autorisations sur un principal de serveur
description: Révoquez les autorisations d’une connexion SQL Server.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, impersonation
- permissions [SQL Server], impersonation
- permissions [SQL Server], logins
- impersonate [SQL Server], revoking
- logins [SQL Server], revoking
- REVOKE statement, logins
ms.assetid: 75409024-f150-4326-af16-9d60e900df18
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 062096e77c7e907594f7c2b9a372e3eb1899324a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185931"
---
# <a name="revoke-server-principal-permissions-transact-sql"></a>REVOKE – révocation d'autorisations de principal de serveur (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de révoquer des autorisations accordées et refusées sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    { FROM | TO } <server_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
    SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey     
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être révoquée sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 LOGIN **::** *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle l’autorisation doit être révoquée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 SERVER ROLE **::** *server_role*  
 Spécifie le rôle de serveur sur lequel l'autorisation est révoquée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 { FROM | TO } \<server_principal> Spécifie le rôle de serveur ou le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de connexion pour lequel l'autorisation est révoquée.  
  
 *SQL_Server_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'une connexion Windows.  
  
 *SQL_Server_login_from_certificate*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un certificat.  
  
 *SQL_Server_login_from_AsymKey*  
 Spécifie le nom d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une clé asymétrique.  
  
 *server_role*  
 Spécifie le nom d'un rôle de serveur défini par l'utilisateur.  
  
 GRANT OPTION  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *SQL_Server_login*  
 Spécifie la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de laquelle le principal qui exécute cette requête dérive son droit de révoquer l'autorisation.  
  
## <a name="remarks"></a>Notes  
 Les comptes de connexion et les rôles de serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des éléments sécurisables au niveau du serveur. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer pour un rôle de serveur ou un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Compte de connexion SQL Server ou autorisation de rôle de serveur|Déduite d'une autorisation de rôle de serveur ou de compte de connexion SQL Server|Déduite d'une autorisation de serveur|  
|------------------------------------------------|-----------------------------------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL SERVER|  
|IMPERSONATE|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|ALTER|CONTROL|ALTER ANY LOGIN<br /><br /> ALTER ANY SERVER ROLE|  
  
## <a name="permissions"></a>Autorisations  
 Pour les comptes de connexion, requiert l'autorisation CONTROL sur le compte de connexion ou l'autorisation ALTER ANY LOGIN sur le serveur.  
  
 Pour les rôles de serveur, requiert l'autorisation CONTROL sur le rôle de serveur ou l'autorisation ALTER ANY SERVER ROLE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-revoking-impersonate-permission-on-a-login"></a>R. Révocation d'une autorisation IMPERSONATE sur une connexion  
 Dans l’exemple ci-dessous, l’autorisation `IMPERSONATE` sur la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof` est révoquée sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir du compte d’utilisateur Windows `AdvWorks\YoonM`.  
  
```sql  
USE master;  
REVOKE IMPERSONATE ON LOGIN::WanidaBenshoof FROM [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-revoking-view-definition-permission-with-cascade"></a>B. Révocation d’une autorisation VIEW DEFINITION avec l’option CASCADE  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`EricKurjan` est révoquée pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`RMeyyappan`. L'option `CASCADE` indique que l'autorisation `VIEW DEFINITION` sur `EricKurjan` sera également révoquée pour les principaux auxquels `RMeyyappan` a accordé cette autorisation.  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON LOGIN::EricKurjan FROM RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-revoking-view-definition-permission-on-a-server-role"></a>C. Révocation d'une autorisation VIEW DEFINITION sur un rôle de serveur  
 L'exemple suivant révoque `VIEW DEFINITION` sur le rôle de serveur `Sales` pour le rôle de serveur `Auditors`.  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [Autorisations du principal du serveur GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [Autorisations du principal du serveur DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  


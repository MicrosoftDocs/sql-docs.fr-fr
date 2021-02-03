---
description: REVOKE – révocation d'autorisations de serveur (Transact-SQL)
title: REVOKE - Révoquer des autorisations sur un serveur (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- REVOKE statement, server permissions
- servers [SQL Server], permissions
ms.assetid: 7b9a56b3-face-452e-a655-147dac306ba1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d0302007b004d1208104928aa5ce3b87243e4c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210937"
---
# <a name="revoke-server-permissions-transact-sql"></a>REVOKE – révocation d'autorisations de serveur (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de supprimer des autorisations GRANT et DENY au niveau serveur.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission  [ ,...n ]   
    { TO | FROM } <grantee_principal> [ ,...n ]  
        [ CASCADE ]  
    [ AS <grantor_principal> ]   
  
<grantee_principal> ::= SQL_Server_login   
        | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être accordée sur un serveur. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 { TO | FROM } \<grantee_principal> spécifie le principal pour lequel l’autorisation est révoquée.  
  
 AS \<grantor_principal> spécifie le principal à partir duquel le principal qui exécute cette requête dérive son droit de révoquer l’autorisation.  
  
 GRANT OPTION FOR  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 *SQL_Server_login*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_mapped_to_Windows_login*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur une connexion Windows.  
  
 *SQL_Server_login_mapped_to_Windows_group*  
 Spécifie une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappée sur un groupe Windows.  
  
 *SQL_Server_login_mapped_to_certificate*  
 Spécifie un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappé à un certificat.  
  
 *SQL_Server_login_mapped_to_asymmetric_key*  
 Spécifie un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mappé à une clé asymétrique.  
  
 *server_role*  
 Spécifie un rôle de serveur défini par l'utilisateur.  
  
## <a name="remarks"></a>Notes  
 Les autorisations dans l'étendue du serveur peuvent être révoquées seulement lorsque la base de données en cours est master.  
  
 REVOKE permet de supprimer les deux autorisations GRANT et DENY.  
  
 Utilisez REVOKE GRANT OPTION FOR pour révoquer le droit d'accorder à son tour l'autorisation spécifiée. Si le principal possède l'autorisation avec le droit de l'accorder, le droit d'accorder l'autorisation est révoqué, mais l'autorisation elle-même n'est pas révoquée. En revanche, si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même est révoquée.  
  
 Les informations sur les autorisations de serveur peuvent être consultées dans la vue de catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), tandis que les informations sur les principaux de serveur peuvent être consultées dans la vue de catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Des informations sur l’appartenance des rôles de serveur peuvent être consultées dans la vue de catalogue [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un serveur représente le plus haut niveau de la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un serveur sont répertoriées dans le tableau ci-dessous.  
  
|Autorisation de serveur|Déduite d'une autorisation de serveur|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|Créer un groupe de disponibilité<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER ou l'appartenance au rôle serveur fixe sysadmin.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-revoking-a-permission-from-a-login"></a>R. Révocation d'une autorisation pour une connexion  
 Dans l'exemple ci-dessous, l'autorisation `VIEW SERVER STATE` est révoquée pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof`.  
  
```sql  
USE master;  
REVOKE VIEW SERVER STATE FROM WanidaBenshoof;  
GO  
```  
  
### <a name="b-revoking-the-with-grant-option"></a>B. Révocation de l'option WITH GRANT  
 Dans l'exemple ci-dessous, le droit d'accorder l'autorisation `CONNECT SQL` est révoqué pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`JanethEsteves`.  
  
```sql  
USE master;  
REVOKE GRANT OPTION FOR CONNECT SQL FROM JanethEsteves;  
GO  
```  
  
 La connexion possède encore l'autorisation CONNECT SQL, mais ne peut plus accorder cette autorisation à d'autres principaux.  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY - Refuser des autorisations sur un serveur &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un serveur (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  

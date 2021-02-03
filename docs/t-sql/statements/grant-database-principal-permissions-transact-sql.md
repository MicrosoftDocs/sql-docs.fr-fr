---
title: GRANT - Autorisations sur un principal de base de données
description: Octroyez les autorisations sur un utilisateur de base de données, un rôle de base de données ou un rôle d’application.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 03/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- permissions [SQL Server], database roles
- granting permissions [SQL Server], database users
- granting permissions [SQL Server], application roles
- granting permissions [SQL Server], database roles
- database user permissions [SQL Server]
- permissions [SQL Server], application roles
- permissions [SQL Server], database users
- GRANT statement, users
- GRANT statement, roles
- application roles [SQL Server], permissions
ms.assetid: 012588a2-cbe1-48f0-a731-b4a2b83203d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b77abab3cfa86caa7c9b79a52150a2ff976958e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205826"
---
# <a name="grant-database-principal-permissions-transact-sql"></a>GRANT – octroi d'autorisations de principal de base de données (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Permet d'accorder des autorisations sur un utilisateur de base de données, un rôle de base de données ou un rôle d'application dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
     | [ ROLE :: database_role ]  
     | [ APPLICATION ROLE :: application_role ]  
    }  
    TO <database_principal> [ ,...n ]  
       [ WITH GRANT OPTION ]  
       [ AS <database_principal> ]  
  
<database_principal> ::=  
    Database_user   
  | Database_role   
  | Application_role   
  | Database_user_mapped_to_Windows_User   
  | Database_user_mapped_to_Windows_Group   
  | Database_user_mapped_to_certificate   
  | Database_user_mapped_to_asymmetric_key   
  | Database_user_with_no_login   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être accordée sur le principal de base de données. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 USER ::*database_user*  
 Spécifie la classe et le nom de l'utilisateur sur lequel l'autorisation est accordée. L'identificateur d'étendue (::) est requis.  
  
 ROLE ::*database_role*  
 Spécifie la classe et le nom du rôle sur lequel l'autorisation est accordée. L'identificateur d'étendue (::) est requis.  
  
 APPLICATION ROLE ::*application_role*  
   
 Spécifie la classe et le nom du rôle d'application sur lequel l'autorisation est accordée. L'identificateur d'étendue (::) est requis.  
  
 WITH GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 AS \<database_principal>  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit d'accorder l'autorisation.  
  
 *Database_user*  
 Spécifie un utilisateur de base de données.  
  
 *Database_role*  
 Spécifie un rôle de base de données.  
  
 *Application_role*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie un rôle d'application.  
  
 *Database_user_mapped_to_Windows_User*  
 Spécifie un utilisateur de base de données mappé sur un utilisateur Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
  
 Spécifie un utilisateur de base de données mappé à un groupe Windows.  
  
 *Database_user_mapped_to_certificate*  
  
 Spécifie un utilisateur de base de données mappé sur un certificat.  
  
 *Database_user_mapped_to_asymmetric_key*  
  
 Spécifie un utilisateur de base de données mappé à une clé asymétrique.  
  
 *Database_user_with_no_login*  
 Spécifie un utilisateur de base de données sans principal au niveau serveur correspondant.  
  
## <a name="remarks"></a>Notes  
 Des informations sur les principaux de base de données sont consultables dans la vue de catalogue [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md). Des informations sur les autorisations au niveau base de données sont consultables dans la vue de catalogue [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md).  
  
## <a name="database-user-permissions"></a>Autorisations d'un utilisateur de base de données  
 Un utilisateur de base de données est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un utilisateur de base de données sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation d'utilisateur de base de données|Déduite d'une autorisation d'utilisateur de base de données|Impliquée par une autorisation de base de données|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>Autorisations de rôle de base de données  
 Un rôle de base de données est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un rôle de base de données sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de rôle de base de données|Déduite d'une autorisation de rôle de base de données|Impliquée par une autorisation de base de données|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>Autorisations de rôle d'application  
 Un rôle d'application est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un rôle d'application sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de rôle d'application|Déduite d'une autorisation de rôle d'application|Impliquée par une autorisation de base de données|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée.  
  
 Si vous utilisez l'option AS, les conditions supplémentaires ci-dessous s'appliquent.  
  
|AS *granting_principal*|Autres autorisations nécessaires|  
|------------------------------|------------------------------------|  
|Utilisateur de base de données|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à un utilisateur Windows|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à un groupe Windows|Appartenance au groupe Windows, appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à un certificat|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à une clé asymétrique|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner, ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données qui n'est mappé sur aucun principal d'un serveur|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Rôle de base de données|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Rôle d'application|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
  
 Les principaux qui possèdent l'autorisation CONTROL sur un élément sécurisable peuvent accorder une autorisation sur cet élément sécurisable.  
  
 Les bénéficiaires de l'autorisation CONTROL sur une base de données, tels que les membres du rôle de base de données fixe db_owner, peuvent accorder une autorisation quelconque sur tout élément sécurisable inclus dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-granting-control-permission-on-a-user-to-another-user"></a>R. Octroi de l'autorisation CONTROL sur un utilisateur à un autre utilisateur  
 Dans l'exemple ci-dessous, l'autorisation `CONTROL` sur l'utilisateur `AdventureWorks2012``Wanida` est accordée à l'utilisateur `RolandX`.  
  
```sql  
GRANT CONTROL ON USER::Wanida TO RolandX;  
GO  
```  
  
### <a name="b-granting-view-definition-permission-on-a-role-to-a-user-with-grant-option"></a>B. Octroi de l'autorisation VIEW DEFINITION sur un rôle à un utilisateur avec l'option GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur le rôle `AdventureWorks2012``SammamishParking` est accordée avec l'option `GRANT OPTION` à l'utilisateur de base de données `JinghaoLiu`.  
  
```sql  
GRANT VIEW DEFINITION ON ROLE::SammamishParking   
    TO JinghaoLiu WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-impersonate-permission-on-a-user-to-an-application-role"></a>C. Octroi de l'autorisation IMPERSONATE sur un utilisateur à un rôle d'application  
 Dans l'exemple ci-dessous, l'autorisation `IMPERSONATE` sur l'utilisateur `HamithaL` est accordée au rôle d'application `AdventureWorks2012``AccountsPayable17`.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
```sql  
GRANT IMPERSONATE ON USER::HamithaL TO AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>Voir aussi  
 [DENY - Refuser des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

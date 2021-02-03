---
title: REVOKE - Autorisations sur un principal de base de données
description: Révoquez des autorisations sur un utilisateur de base de données, un rôle de base de données ou un rôle d’application.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- REVOKE statement, roles
- database user permissions [SQL Server]
- REVOKE statement, users
- application roles [SQL Server], permissions
ms.assetid: c45e1086-c25b-48bb-a764-4a893e983db2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 312c90b77326bf153cc73f805ebed0e8a1b020f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209899"
---
# <a name="revoke-database-principal-permissions-transact-sql"></a>REVOKE – révocation d'autorisations de principal de base de données (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de révoquer des autorisations accordées ou refusées sur un utilisateur de base de données, un rôle de base de données ou un rôle d'application.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
       | [ ROLE :: database_role ]  
       | [ APPLICATION ROLE :: application_role ]  
    }  
    { FROM | TO } <database_principal> [ ,...n ]  
        [ CASCADE ]  
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
 Spécifie une autorisation qui peut être révoquée sur le principal de base de données. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 USER ::*database_user*  
 Spécifie la classe et le nom de l'utilisateur sur lequel l'autorisation doit être révoquée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 ROLE ::*database_role*  
 Spécifie la classe et le nom du rôle sur lequel l'autorisation doit être révoquée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 APPLICATION ROLE ::*application_role*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Spécifie la classe et le nom du rôle d'application sur lequel l'autorisation doit être révoquée. Le qualificateur d’étendue ( **::** ) est obligatoire.  
  
 GRANT OPTION  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS \<database_principal> Spécifie un principal dont le principal qui exécute cette requête dérive son droit de révoquer l’autorisation.  
  
 *Database_user*  
 Spécifie un utilisateur de base de données.  
  
 *Database_role*  
 Spécifie un rôle de base de données.  
  
 *Application_role*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Spécifie un rôle d'application.  
  
 *Database_user_mapped_to_Windows_User*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé sur un utilisateur Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé à un groupe Windows.  
  
 *Database_user_mapped_to_certificate*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé sur un certificat.  
  
 *Database_user_mapped_to_asymmetric_key*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé à une clé asymétrique.  
  
 *Database_user_with_no_login*  
 Spécifie un utilisateur de base de données sans principal au niveau serveur correspondant.  
  
## <a name="remarks"></a>Notes  
  
## <a name="database-user-permissions"></a>Autorisations d'un utilisateur de base de données  
 Un utilisateur de base de données est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un utilisateur de base de données sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation d'utilisateur de base de données|Déduite d'une autorisation d'utilisateur de base de données|Impliquée par une autorisation de base de données|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>Autorisations de rôle de base de données  
 Un rôle de base de données est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un rôle de base de données sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de rôle de base de données|Déduite d'une autorisation de rôle de base de données|Impliquée par une autorisation de base de données|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>Autorisations de rôle d'application  
 Un rôle d'application est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un rôle d'application sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de rôle d'application|Déduite d'une autorisation de rôle d'application|Impliquée par une autorisation de base de données|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le principal spécifié ou une autorisation plus élevée qui implique l'autorisation CONTROL.  
  
 Les bénéficiaires de l’autorisation CONTROL sur une base de données, comme les membres du rôle de base de données fixe **db_owner**, peuvent accorder n’importe quelle autorisation sur n’importe quel sécurisable de la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-revoking-control-permission-on-a-user-from-another-user"></a>R. Révocation d'une autorisation CONTROL sur un utilisateur à partir d'un autre utilisateur  
 Dans l'exemple ci-dessous, l'autorisation `CONTROL` sur l'utilisateur [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]`Wanida` est révoquée pour l'utilisateur `RolandX`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE CONTROL ON USER::Wanida FROM RolandX;  
GO  
```  
  
### <a name="b-revoking-view-definition-permission-on-a-role-from-a-user-to-which-it-was-granted-with-grant-option"></a>B. Révocation d'une autorisation VIEW DEFINITION sur un rôle à partir d'un utilisateur auquel l'autorisation avait été accordée avec l'option WITH GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `VIEW DEFINITION` sur le rôle [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]`SammamishParking` est révoquée pour l'utilisateur de base de données `JinghaoLiu`. L'option `CASCADE` est spécifiée car l'utilisateur `JinghaoLiu` avait obtenu l'autorisation `VIEW DEFINITION` avec l'option `WITH GRANT OPTION`.  
  
```sql 
USE AdventureWorks2012;  
REVOKE VIEW DEFINITION ON ROLE::SammamishParking   
    FROM JinghaoLiu CASCADE;  
GO  
```  
  
### <a name="c-revoking-impersonate-permission-on-a-user-from-an-application-role"></a>C. Révocation d'une autorisation IMPERSONATE sur un utilisateur à partir d'un rôle d'application  
 Dans l'exemple ci-dessous, l'autorisation `IMPERSONATE` sur l'utilisateur `HamithaL` est révoquée pour le rôle d'application [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]`AccountsPayable17`.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
```sql  
USE AdventureWorks2012;  
REVOKE IMPERSONATE ON USER::HamithaL FROM AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT - Octroyer des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)   
 [DENY - Refuser des autorisations sur un principal de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  


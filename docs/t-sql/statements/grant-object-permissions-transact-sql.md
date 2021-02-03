---
description: GRANT – octroi d'autorisations d'objet (Transact-SQL)
title: GRANT - Octroyer des autorisations sur un objet (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], objects
- GRANT statement, objects
ms.assetid: c001c2e7-d092-43d4-8fa6-693b3ec4c3ea
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 681cb64aa6ef4874d331d61e2d8c07df2ed06ac1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161240"
---
# <a name="grant-object-permissions-transact-sql"></a>GRANT – octroi d'autorisations d'objet (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Permet d'accorder des autorisations sur une table, une vue, une fonction table, une procédure stockée, une procédure stockée étendue, une fonction scalaire, une fonction d'agrégation, une file d'attente de service ou un synonyme.  
  

  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
    TO <database_principal> [ ,...n ]   
    [ WITH GRANT OPTION ]  
    [ AS <database_principal> ]  
  
<permission> ::=  
    ALL [ PRIVILEGES ] | permission [ ( column [ ,...n ] ) ]  
  
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
 Spécifie une autorisation qui peut être accordée sur un objet contenu dans un schéma. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ALL  
 L'utilisation de l'option ALL n'entraîne pas l'octroi de toutes les autorisations possibles. L'utilisation de l'option ALL équivaut à accorder toutes les autorisations [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 applicables à l'objet spécifié. La signification de l'option ALL varie comme suit :  
  
- Autorisations de fonction scalaire : EXECUTE, REFERENCES.  
- Autorisations de fonction table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Autorisations de procédure stockée : EXECUTE.  
- Autorisations de table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Autorisations de vue : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Inclus pour la compatibilité [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92. Ne change pas le comportement de l'option ALL.  
  
*column*  
 Spécifie le nom d'une colonne dans une table, une vue ou une fonction table, pour laquelle l'autorisation doit être accordée. Les parenthèses ( ) sont obligatoires. Seules les autorisations SELECT, REFERENCES et UPDATE peuvent être accordées sur une colonne. *column* peut être spécifié dans la clause des autorisations ou après le nom de l’élément sécurisable.  
  
> [!CAUTION]  
>  Une instruction DENY de niveau table n'a pas la priorité sur une instruction GRANT de niveau colonne. Cette incohérence dans la hiérarchie des autorisations a été conservée pour des raisons de compatibilité descendante.  
  
 ON [ OBJECT :: ] [ *schema_name* ] . *object_name*  
 Spécifie l'objet sur lequel l'autorisation doit être accordée. L’expression OBJECT est facultative si *schema_name* est spécifié. Si l'expression OBJECT est utilisée, l'identificateur d'étendue (::) est requis. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma (.) est obligatoire.  
  
 TO \<database_principal>  
 Spécifie le principal auquel l'autorisation est accordée.  
  
 WITH GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 AS \<database_principal> Spécifie un principal dont le principal qui exécute cette requête dérive son droit d’accorder l’autorisation.  
  
 *Database_user*  
 Spécifie un utilisateur de base de données.  
  
 *Database_role*  
 Spécifie un rôle de base de données.  
  
 *Application_role*  
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
  
> [!IMPORTANT]  
>  Dans certains cas, une combinaison d'autorisations ALTER et REFERENCE pourrait autoriser le bénéficiaire des autorisations à afficher des données ou à exécuter des fonctions non autorisées. Par exemple : un utilisateur avec une autorisation ALTER sur une table et une autorisation REFERENCE sur une fonction peut créer une colonne calculée sur une fonction et l’exécuter. Dans ce cas, l'utilisateur aurait également besoin de l'autorisation SELECT sur la colonne calculée.  
  
 Des informations sur les objets sont consultables dans différents affichages catalogue. Pour plus d’informations, consultez [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md).  
  
 Un objet est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un objet sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation d'objet|Déduite d'une autorisation d'objet|Déduite d'une autorisation de schéma|  
|-----------------------|----------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|Suppression|CONTROL|Suppression|  
|Exécutez|CONTROL|Exécutez|  
|INSERT|CONTROL|INSERT|  
|RECEIVE|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|RECEIVE|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|VIEW CHANGE TRACKING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée.  
  
 Si vous utilisez l'option AS, les conditions supplémentaires ci-dessous s'appliquent.  
  
|AS|Autres autorisations nécessaires|  
|--------|------------------------------------|  
|Utilisateur de base de données|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à une connexion Windows|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à un groupe Windows|Appartenance au groupe Windows, appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à un certificat|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données mappé à une clé asymétrique|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Utilisateur de base de données qui n'est mappé sur aucun principal d'un serveur|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Rôle de base de données|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
|Rôle d'application|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-granting-select-permission-on-a-table"></a>R. Octroi d'une autorisation SELECT sur une table  
 Dans l'exemple ci-dessous, l'autorisation `SELECT` est accordée à l'utilisateur `RosaQdM` sur la table `Person.Address` dans la base de données `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-a-stored-procedure"></a>B. Octroi d'une autorisation EXECUTE sur une procédure stockée  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur la procédure stockée `HumanResources.uspUpdateEmployeeHireInfo` est accordée à un rôle d'application nommé `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-granting-references-permission-on-a-view-with-grant-option"></a>C. Octroi d'une autorisation REFERENCES sur une vue avec l'option GRANT OPTION  
 Dans l'exemple ci-dessous, l'autorisation `REFERENCES` sur la colonne `BusinessEntityID` dans la vue `HumanResources.vEmployee` est accordée à l'utilisateur `Wanida` avec l'option `GRANT OPTION`.  
  
```sql  
GRANT REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida WITH GRANT OPTION;  
GO  
```  
  
### <a name="d-granting-select-permission-on-a-table-without-using-the-object-phrase"></a>D. Octroi d'une autorisation SELECT sur une table sans utilisation de l'expression OBJECT  
 Dans l'exemple ci-dessous, l'autorisation `SELECT` est accordée à l'utilisateur `RosaQdM` sur la table `Person.Address` dans la base de données `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="e-granting-select-permission-on-a-table-to-a-domain-account"></a>E. Octroi d'une autorisation SELECT sur une table à un compte de domaine  
 Dans l'exemple ci-dessous, l'autorisation `SELECT` est accordée à l'utilisateur `AdventureWorks2012\RosaQdM` sur la table `Person.Address` dans la base de données `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO [AdventureWorks2012\RosaQdM];  
GO  
```  
  
### <a name="f-granting-execute-permission-on-a-procedure-to-a-role"></a>F. Octroi d'une autorisation EXECUTE sur une procédure à un rôle  
 L'exemple suivant crée un rôle, puis accorde une autorisation `EXECUTE` au rôle sur la procédure `uspGetBillOfMaterials` dans la base de données `AdventureWorks2012`.  
  
```sql  
CREATE ROLE newrole ;  
GRANT EXECUTE ON dbo.uspGetBillOfMaterials TO newrole ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DENY – refus d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  


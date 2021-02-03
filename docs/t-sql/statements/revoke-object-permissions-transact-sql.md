---
description: REVOKE – révocation d'autorisations d'objet (Transact-SQL)
title: REVOKE - Révoquer des autorisations sur un objet (Transact-SQL) | Microsoft Docs
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
- table permissions [SQL Server], revoking
- REVOKE statement, objects
- revoking permissions to access tables
- object permissions [SQL Server], revoking
ms.assetid: 99c7146e-d2e7-4f1a-80ff-21a05bc5e8bb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4f0f8cc03788db8db74564089dd6db9bffd4ca12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209873"
---
# <a name="revoke-object-permissions-transact-sql"></a>REVOKE – révocation d'autorisations d'objet (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de révoquer des autorisations sur une table, une vue, une fonction table, une procédure stockée, une procédure stockée étendue, une fonction scalaire, une fonction d'agrégation, une file d'attente de service ou un synonyme. 
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
        { FROM | TO } <database_principal> [ ,...n ]   
    [ CASCADE ]  
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
 Spécifie une autorisation qui peut être révoquée sur un objet contenu dans un schéma. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ALL  
 L'utilisation de l'option ALL n'entraîne pas la révocation de toutes les autorisations possibles. L'utilisation de l'option ALL équivaut à révoquer toutes les autorisations [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 applicables à l'objet spécifié. La signification de l'option ALL varie comme suit :  
  
 Autorisations de fonction scalaire : EXECUTE, REFERENCES.  
  
 Autorisations de fonction table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
 Autorisations de procédure stockée : EXECUTE.  
  
 Autorisations de table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
 Autorisations de vue : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
 PRIVILEGES  
 Inclus pour la compatibilité [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92. Ne change pas le comportement de l'option ALL.  
  
 *column*  
 Spécifie le nom d'une colonne dans une table, une vue ou une fonction table, pour laquelle l'autorisation doit être révoquée. Les parenthèses ( ) sont obligatoires. Seules les autorisations SELECT, REFERENCES et UPDATE peuvent être refusées sur une colonne. *column* peut être spécifié dans la clause des autorisations ou après le nom de l’élément sécurisable.  
  
 ON [ OBJECT :: ] [ *schema_name* ] . *object_name*  
 Spécifie l’objet sur lequel l’autorisation doit être révoquée. L’expression OBJECT est facultative si *schema_name* est spécifié. Si l'expression OBJECT est utilisée, l'identificateur d'étendue (::) est requis. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma (.) est obligatoire.  
  
 { FROM | TO } \<database_principal> Spécifie le principal pour lequel l’autorisation est révoquée.  
  
 GRANT OPTION  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS \<database_principal> spécifie un principal dont le principal qui exécute cette requête dérive son droit de révoquer l’autorisation.  
  
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
 Des informations sur les objets sont consultables dans différents affichages catalogue. Pour plus d’informations, consultez [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md).  
  
 Un objet est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer sur un objet sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
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
 Requiert l'autorisation CONTROL sur l'objet.  
  
 Si vous utilisez la clause AS, le principal spécifié doit posséder l'objet sur lequel les autorisations doivent être révoquées.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-revoking-select-permission-on-a-table"></a>R. Révocation d'une autorisation SELECT sur une table  
 Dans l'exemple ci-dessous, l'autorisation `SELECT` est révoquée pour l'utilisateur `RosaQdM` sur la table `Person.Address` dans la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE SELECT ON OBJECT::Person.Address FROM RosaQdM;  
GO  
```  
  
### <a name="b-revoking-execute-permission-on-a-stored-procedure"></a>B. Révocation d'une autorisation EXECUTE sur une procédure stockée  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur la procédure stockée `HumanResources.uspUpdateEmployeeHireInfo` est révoquée pour un rôle d'application nommé `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    FROM Recruiting11;  
GO   
```  
  
### <a name="c-revoking-references-permission-on-a-view-with-cascade"></a>C. Révocation d'une autorisation REFERENCES sur une vue avec l'option CASCADE  
 Dans l'exemple ci-dessous, l'autorisation `REFERENCES` sur la colonne `BusinessEntityID` dans la vue `HumanResources.vEmployee` est révoquée pour l'utilisateur `Wanida` avec l'option `CASCADE`.  
  
```sql  
USE AdventureWorks2012;  
REVOKE REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    FROM Wanida CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT – octroi d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [DENY – refus d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  


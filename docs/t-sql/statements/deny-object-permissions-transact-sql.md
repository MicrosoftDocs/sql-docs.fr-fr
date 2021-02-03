---
description: DENY – refus d'autorisations d'objet (Transact-SQL)
title: DENY - Refuser des autorisations sur un objet (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, objects
- table permissions [SQL Server]
ms.assetid: 0b8d3ddc-38c0-4241-b7bb-ee654a5081aa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 238940e3e6519be1b4758e794cf391869f057986
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177732"
---
# <a name="deny-object-permissions-transact-sql"></a>DENY – refus d'autorisations d'objet (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Refuse des autorisations à un membre de la classe OBJECT d'éléments sécurisables. Il s'agit des membres de la classe OBJECT : tables, vues, fonctions table, procédures stockées, procédures stockées étendues, fonctions scalaires, fonctions d'agrégation, files d'attente de service et synonymes.  

  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DENY <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
        TO <database_principal> [ ,...n ]   
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
 Spécifie une autorisation qui peut être refusée sur un objet contenu dans un schéma. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ALL  
 L'utilisation de l'option ALL n'entraîne pas le refus de toutes les autorisations possibles. L'utilisation de l'option ALL équivaut à refuser toutes les autorisations ANSI-92 applicables à l'objet spécifié. La signification de l'option ALL varie comme suit :  
  
 - Autorisations de fonction scalaire : EXECUTE, REFERENCES.  
 - Autorisations de fonction table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
 - Autorisations de procédure stockée : EXECUTE.  
 - Autorisations de table : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
 - Autorisations de vue : DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Inclus pour la conformité à ANSI-92. Ne change pas le comportement de l'option ALL.  
  
*column*  
 Spécifie le nom d'une colonne dans une table, une vue ou une fonction table, pour laquelle l'autorisation doit être refusée. Les parenthèses **( )** sont obligatoires. Seules les autorisations SELECT, REFERENCES et UPDATE peuvent être refusées sur une colonne. *column* peut être spécifié dans la clause des autorisations ou après le nom de l’élément sécurisable.  
  
> [!CAUTION]  
>  Une instruction DENY de niveau table n'a pas la priorité sur une instruction GRANT de niveau colonne. Cette incohérence dans la hiérarchie des autorisations a été conservée pour des raisons de compatibilité descendante.  
  
 ON [ OBJECT **::** ] [ *schema_name* ] **.** *object_name*  
 Spécifie l’objet sur lequel l’autorisation doit être refusée. L’expression OBJECT est facultative si *schema_name* est spécifié. Si l’expression OBJECT est utilisée, le qualificateur d’étendue ( **::** ) est obligatoire. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma ( **.** ) est obligatoire.  
  
 TO \<database_principal>  
 Spécifie le principal auquel l'autorisation est refusée.  
  
 CASCADE  
 Indique que l'autorisation à refuser est également refusée pour les autres principaux auxquels elle a été accordée par ce principal.  
  
 AS \<database_principal>  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de refuser l'autorisation.  
  
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
  
 Un objet est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de refuser sur un objet sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
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
  
 Si vous utilisez la clause AS, le principal spécifié doit posséder l'objet sur lequel les autorisations doivent être refusées.  
  
## <a name="examples"></a>Exemples  
Les exemples suivants utilisent la base de données AdventureWorks.
  
### <a name="a-denying-select-permission-on-a-table"></a>R. Refus d'une autorisation SELECT sur une table  
 Dans l’exemple ci-dessous, l’autorisation `SELECT` est refusée à l’utilisateur `RosaQdM` sur la table `Person.Address`.  
  
```sql  
DENY SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-denying-execute-permission-on-a-stored-procedure"></a>B. Refus d'une autorisation EXECUTE sur une procédure stockée  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur la procédure stockée `HumanResources.uspUpdateEmployeeHireInfo` est refusée à un rôle d'application nommé `Recruiting11`.  
  
```sql  
DENY EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-denying-references-permission-on-a-view-with-cascade"></a>C. Refus d'une autorisation REFERENCES sur une vue avec l'option CASCADE  
 Dans l'exemple ci-dessous, l'autorisation `REFERENCES` sur la colonne `BusinessEntityID` dans la vue `HumanResources.vEmployee` est refusée à l'utilisateur `Wanida` avec l'option `CASCADE`.  
  
```sql  
DENY REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT – octroi d’autorisations d’objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  

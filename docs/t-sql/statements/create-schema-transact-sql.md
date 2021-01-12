---
description: CREATE SCHEMA (Transact-SQL)
title: CREATE SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SCHEMA_TSQL
- SCHEMA
- CREATE SCHEMA
- SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], schemas
- table creation [SQL Server], CREATE SCHEMA statement
- permissions [SQL Server], schemas
- schemas [SQL Server], creating
- CREATE SCHEMA statement
ms.assetid: df74fc36-20da-4efa-b412-c4e191786695
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 83699c45a36f353d20b69ba6b9a4101608bd2be8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092009"
---
# <a name="create-schema-transact-sql"></a>CREATE SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crée un schéma dans la base de données active. La transaction CREATE SCHEMA peut également créer des tables et des vues dans le nouveau schéma et définir des autorisations GRANT, DENY ou REVOKE sur ces objets.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE SCHEMA schema_name_clause [ <schema_element> [ ...n ] ]  
  
<schema_name_clause> ::=  
    {  
    schema_name  
    | AUTHORIZATION owner_name  
    | schema_name AUTHORIZATION owner_name  
    }  
  
<schema_element> ::=   
    {   
        table_definition | view_definition | grant_statement |   
        revoke_statement | deny_statement   
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE SCHEMA schema_name [ AUTHORIZATION owner_name ] [;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom qui identifie le schéma dans cette base de données.  
  
 AUTHORIZATION *owner_name*  
 Spécifie le nom du principal au niveau base de données qui est propriétaire du schéma. Ce principal peut posséder d'autres schémas et peut ne pas utiliser le schéma actif comme schéma par défaut.  
  
 *table_definition*  
 Spécifie une instruction CREATE TABLE qui crée une table dans le schéma. Le principal qui exécute cette instruction doit avoir l'autorisation CREATE TABLE sur la base de données active.  
  
 *view_definition*  
 Spécifie une instruction CREATE VIEW qui crée une vue dans le schéma. Le principal qui exécute cette instruction doit avoir l'autorisation CREATE VIEW sur la base de données active.  
  
 *grant_statement*  
 Spécifie une instruction GRANT qui accorde des autorisations sur tout élément sécurisable à l'exception du nouveau schéma.  
  
 *revoke_statement*  
 Spécifie une instruction REVOKE qui révoque des autorisations sur tout élément sécurisable à l'exception du nouveau schéma.  
  
 *deny_statement*  
 Spécifie une instruction DENY qui refuse des autorisations sur tout élément sécurisable à l'exception du nouveau schéma.  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  Les instructions qui contiennent CREATE SCHEMA AUTHORIZATION sans spécifier un nom sont acceptées uniquement pour des raisons de compatibilité descendante. L'instruction ne génère pas d'erreur, mais un schéma n'est pas créé.  
  
 CREATE SCHEMA peut créer un schéma, les tables et les vues qu'il contient, et accorder (GRANT), révoquer (REVOKE) ou refuser (DENY) des autorisations sur tout élément sécurisable dans une instruction. Cette instruction doit être exécutée en tant que lot séparé. Les objets créés par l'instruction CREATE SCHEMA sont créés à l'intérieur du schéma en cours de création.  
  
 Les transactions CREATE SCHEMA sont atomiques. Si une erreur se produit pendant l'exécution d'une instruction CREATE SCHEMA, aucun des éléments sécurisables spécifiés n'est créé et aucune autorisation n'est accordée.  
  
 Il est possible de répertorier les éléments sécurisables à créer par l'instruction CREATE SCHEMA dans n'importe quel ordre, excepté pour les vues qui référencent d'autres vues. Dans ce cas, la vue référencée doit être créée avant la vue qui la référence.  
  
 Par conséquent, une instruction GRANT peut accorder une autorisation sur un objet avant la création de cet objet, ou une instruction CREATE VIEW peut apparaître avant les instructions CREATE TABLE qui créent les tables référencées par la vue. De même, les instructions CREATE TABLE peuvent déclarer des clés étrangères sur des tables qui sont définies ultérieurement dans l'instruction CREATE SCHEMA.  
  
> [!NOTE]  
>  DENY et REVOKE sont pris en charge dans des instructions CREATE SCHEMA. Les clauses DENY et REVOKE sont exécutées dans leur ordre d'apparition dans l'instruction CREATE SCHEMA.  
  
 Le principal qui exécute CREATE SCHEMA peut désigner un autre principal de base de données en tant que propriétaire du schéma créé. Pour cela, il est nécessaire de disposer d'autorisations supplémentaires, comme l'explique la section « Autorisations » plus loin dans cette rubrique.  
  
 Le nouveau schéma appartient à l'un des principaux de base de données suivants : utilisateur de base de données, rôle de base de données ou rôle d'application. Les objets créés dans un schéma appartiennent au propriétaire du schéma. La valeur **principal_id** de ces objets est NULL dans **sys.objects**. Il est possible de transférer la propriété des objets contenus dans le schéma à n'importe quel principal de base de données, mais le propriétaire du schéma conserve toujours l'autorisation CONTROL sur les objets dans le schéma.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 **Création implicite d’utilisateur et de schéma**  
  
 Dans certains cas, un utilisateur peut utiliser une base de données sans posséder de compte d'utilisateur de base de données (principal de base de données dans la base de données). Cela peut se produire dans les situations suivantes :  
  
-   Une connexion a des privilèges **CONTROL SERVER**.  
  
-   Un utilisateur de Windows ne possède pas de compte d'utilisateur de base de données individuel (principal de base de données dans la base de données), mais il accède à une base de données en tant que membre d'un groupe Windows possédant un compte d'utilisateur de base de données (principal de base de données pour le groupe Windows).  
  
 Lorsqu'un utilisateur sans compte d'utilisateur de base de données crée un objet sans spécifier de schéma existant, un principal de base de données et un schéma par défaut sont créés automatiquement dans la base de données pour cet utilisateur. Le schéma et le principal de base de données créés auront le même nom que celui utilisé par l'utilisateur lors de la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (le nom de connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le nom d'utilisateur Windows).  
  
 Ce comportement est nécessaire pour permettre aux utilisateurs basés sur des groupes Windows de créer et de posséder des objets. Il peut cependant entraîner la création involontaire de schémas et d'utilisateurs. Pour éviter la création implicite d'utilisateurs et de schémas, dans la mesure du possible vous devez créer de manière explicite des principaux de base de données et assigner un schéma par défaut. Vous pouvez également déclarer de manière explicite un schéma existant lors de la création d'objets dans une base de données, à l'aide de noms d'objets en deux ou trois parties.  

> [!NOTE]
>  La création implicite d’un utilisateur Azure Active Directory n’est pas possible sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Étant donné que la création d’un utilisateur Azure AD à partir du fournisseur externe doit vérifier l’état des utilisateurs dans l’annuaire AAD, la création de l’utilisateur échoue avec l’erreur 2760 : **Le nom de schéma spécifié « \<user_name@domain> » n’existe pas ou vous n’êtes pas autorisé à l’utiliser.** Puis avec l’erreur 2759 : **Échec de CREATE SCHEMA en raison d’erreurs antérieures.** Pour contourner ces erreurs, créez d’abord l’utilisateur Azure AD à partir du fournisseur externe, puis réexécutez l’instruction de création de l’objet.
 
  
## <a name="deprecation-notice"></a>Note relative à la suppression de fonctionnalités  
 Les instructions CREATE SCHEMA qui ne spécifient pas de nom de schéma continuent à être prises en charge pour des raisons de compatibilité descendante. De telles instructions ne créent pas réellement un schéma dans la base de données, mais créent plutôt des tables et des vues, et accordent des autorisations. Les principaux n'ont pas besoin de l'autorisation CREATE SCHEMA pour exécuter cette version antérieure de CREATE SCHEMA, car aucun schéma n'est créé. Cette fonctionnalité sera retirée dans une version future de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CREATE SCHEMA sur la base de données.  
  
 Pour créer un objet spécifié dans l'instruction CREATE SCHEMA, l'utilisateur doit disposer de l'autorisation CREATE correspondante.  
  
 Pour spécifier un autre utilisateur comme propriétaire du schéma à créer, l'appelant doit avoir l'autorisation IMPERSONATE sur cet utilisateur. Si un rôle de base de données est spécifié en tant que propriétaire, l'appelant doit disposer de l'un des éléments suivants : appartenance au rôle ou autorisation ALTER pour le rôle.  
  
> [!NOTE]  
>  Pour la compatibilité descendante de la syntaxe, aucune vérification n'est effectuée sur l'autorisation CREATE SCHEMA car aucun schéma n'est créé.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-schema-and-granting-permissions"></a>R. Création d’un schéma et octroi d’autorisations  
 L'exemple suivant crée le schéma `Sprockets` détenu par `Annik` qui contient la table `NineProngs`. L'instruction accorde `SELECT` à `Mandar` et refuse `SELECT` à `Prasanna`. Remarquez que `Sprockets` et `NineProngs` sont créés dans une même instruction.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets AUTHORIZATION Annik  
    CREATE TABLE NineProngs (source int, cost int, partnumber int)  
    GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
    DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
GO   
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-creating-a-schema-and-a-table-in-the-schema"></a>B. Création d’un schéma et d’une table dans le schéma  
 L’exemple suivant crée un schéma `Sales`, puis une table `Sales.Region` dans ce schéma.  
  
```sql  
CREATE SCHEMA Sales;  
GO
  
CREATE TABLE Sales.Region   
(Region_id INT NOT NULL,  
Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
```  
  
### <a name="c-setting-the-owner-of-a-schema"></a>C. Définition du propriétaire d’un schéma  
 L’exemple suivant crée un schéma `Production` appartenant à `Mary`.  
  
```sql  
CREATE SCHEMA Production AUTHORIZATION [Contoso\Mary];  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [Créer un schéma de base de données](../../relational-databases/security/authentication-access/create-a-database-schema.md)  
  
  



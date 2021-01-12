---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 73cd35c7f05d39d7ec6930be6273dc22c16f9c56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092054"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Transfère un élément sécurisable d'un schéma à un autre.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom d’un schéma de la base de données active où l’élément sécurisable doit être déplacé. Ne peut pas être SYS ou INFORMATION_SCHEMA.  
  
 \<entity_type>  
 Classe de l'entité pour laquelle il y a un changement de propriétaire. Object est la valeur par défaut.  
  
 *securable_name*  
 Nom en une ou deux parties d’un élément sécurisable délimité aux schémas à transférer dans le schéma.  
  
## <a name="remarks"></a>Notes  
 Les utilisateurs et les schémas sont complètement distincts.  
  
 L'instruction ALTER SCHEMA ne peut être utilisée que pour transférer des éléments sécurisables entre des schémas de la même base de données. Pour modifier ou supprimer un élément sécurisable au sein du même schéma, utilisez l'instruction ALTER ou DROP propre à cet élément sécurisable.  
  
 Si un nom en une seule partie est utilisé pour *securable_name*, les règles de résolution de noms en vigueur s’appliquent pour identifier l’emplacement de l’élément sécurisable.  
  
 Toutes les autorisations associées à cet élément sécurisable sont supprimées après le transfert de l'élément sécurisable vers le nouveau schéma. Si le propriétaire de l'élément sécurisable a été défini explicitement, il reste inchangé. Si la valeur SCHEMA OWNER a été attribuée au propriétaire de l'élément sécurisable, celui-ci reste SCHEMA OWNER ; une fois le transfert effectué, la valeur de SCHEMA OWNER est le propriétaire du nouveau schéma. L'identification principal_id du nouveau propriétaire sera NULL.  
  
 Le fait de déplacer une procédure stockée, une fonction, une vue ou un déclencheur ne change pas le nom de schéma, le cas échéant, de l’objet correspondant présent dans la colonne de définition de la vue de catalogue[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) ou obtenu à l’aide de la fonction intégrée [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md). Par conséquent, il est déconseillé d’utiliser ALTER SCHEMA pour renommer ces types d’objets. À la place, supprimez puis recréez l’objet dans son nouveau schéma.  
  
 Le fait de déplacer un objet tel qu’une table ou un synonyme ne met pas automatiquement à jour les références à cet objet. Vous devez modifier manuellement tout objet qui référence l’objet transféré. Par exemple, si vous déplacez une table et que cette table est référencée dans un déclencheur, vous devez modifier le déclencheur pour refléter le nom du nouveau schéma. Utilisez [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) pour répertorier les dépendances de l’objet avant de le déplacer.  

 Pour changer le schéma d’une table à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table, puis sélectionnez **Conception**. Appuyez sur **F4** pour ouvrir la fenêtre Propriétés. Dans la zone **Schéma**, sélectionnez un nouveau schéma.  
 
 ALTER SCHEMA utilise un verrou de niveau de schéma.
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Autorisations  
 Pour transférer un élément sécurisable provenant d'un autre schéma, l'utilisateur actuel doit bénéficier de l'autorisation CONTROL sur l'élément sécurisable (et non sur le schéma) et de l'autorisation ALTER sur le schéma cible.  
  
 Si l’élément sécurisable est soumis à une spécification EXECUTE AS OWNER et que le propriétaire est défini sur SCHEMA OWNER, l’utilisateur doit également bénéficier de l’autorisation IMPERSONATE sur le propriétaire du schéma cible.  
  
 Toutes les autorisations associées à l'élément sécurisable soumis à un transfert sont supprimées lors du déplacement de cet élément.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-transferring-ownership-of-a-table"></a>R. Transfert de la propriété d'une table  
 L’exemple suivant modifie le schéma `HumanResources` en transférant la table `Address` du schéma `Person` au schéma « HumanResources ».  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>B. Transfert de propriété d'un type  
 L'exemple suivant crée un type dans le schéma `Production`, puis transfère le type vers le schéma `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>C. Transfert de la propriété d'une table  
 L’exemple suivant crée une table `Region` dans le schéma `dbo`, crée un schéma `Sales`, puis déplace la table `Region` du schéma `dbo` vers le schéma `Sales`.  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  


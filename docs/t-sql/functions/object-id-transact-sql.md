---
description: OBJECT_ID (Transact-SQL)
title: OBJECT_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_ID
- OBJECT_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], IDs
- identification numbers [SQL Server], database objects
- checking object exists
- IDs [SQL Server], database objects
- OBJECT_ID function
- database objects [SQL Server], IDs
- displaying object IDs
- viewing object IDs
- verifying object exists
ms.assetid: f89286db-440f-4218-a828-30881ce3077a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d3e0cac16d1a3d7f180117dc29b20fea8e53276
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189914"
---
# <a name="object_id-transact-sql"></a>OBJECT_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie le numéro d'identification d'un objet de la base de données pour un objet défini avec l'étendue d'un schéma.  
  
> [!IMPORTANT]  
>  Il n'est pas possible d'exécuter des requêtes sur des objets qui ne sont pas définis avec l'étendue d'un schéma, tels que des déclencheurs DDL, en utilisant OBJECT_ID. Pour les objets qui ne figurent pas dans la vue de catalogue [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md), vous pouvez obtenir leurs numéros d’identification en interrogeant la vue de catalogue appropriée. Par exemple, pour renvoyer le numéro d’identification d’un déclencheur DDL, utilisez `SELECT OBJECT_ID FROM sys.triggers WHERE name = 'DatabaseTriggerLog``'`.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
OBJECT_ID ( '[ database_name . [ schema_name ] . | schema_name . ]   
  object_name' [ ,'object_type' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **'** *object_name* **'**  
 Objet à utiliser. *object_name* est de type **varchar** ou **nvarchar**. Si *object_name* est de type **varchar**, il est converti implicitement en **nvarchar**. La spécification des noms de la base de données et du schéma est facultative.  
  
 **'** *object_type* **'**  
 Type de l'objet défini avec l'étendue du schéma. *object_type* est de type **varchar** ou **nvarchar**. Si *object_type* est de type **varchar**, il est converti implicitement en **nvarchar**. Pour obtenir la liste des types d’objets, consultez la colonne **type** de [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="exceptions"></a>Exceptions  
 Pour un index spatial, OBJECT_ID retourne la valeur NULL.  
  
 Retourne NULL en cas d'erreur.  
  
 Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, telles que OBJECT_ID, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Remarques  
 Quand le paramètre d'une fonction système est facultatif, la base de données active, l'ordinateur hôte, l'utilisateur du serveur ou l'utilisateur de la base de données sont pris implicitement en considération. Les fonctions intégrées doivent toujours être suivies de parenthèses.  
  
 Lorsqu’un nom de table temporaire est spécifié, le nom de la base de données doit figurer avant le nom de la table temporaire, à moins que la base de données active soit **tempdb**. Par exemple : `SELECT OBJECT_ID('tempdb..#mytemptable')`.  
  
 Les fonctions système peuvent être utilisées dans la liste de sélection, dans une clause WHERE, et partout où une expression est autorisée. Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md) et [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-object-id-for-a-specified-object"></a>R. Renvoi de l'identificateur d'un objet spécifié  
 L'exemple suivant renvoie l'ID d'objet de la table `Production.WorkOrder` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE master;  
GO  
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';  
GO  
```  
  
### <a name="b-verifying-that-an-object-exists"></a>B. Vérification de l'existence d'un objet  
 L'exemple suivant vérifie l'existence d'une table spécifiée en vérifiant que la table a un identificateur d'objet. Si la table existe, elle est supprimée. Si elle n'existe pas, l'instruction `DROP TABLE` n'est pas exécutée.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL  
DROP TABLE dbo.AWBuildVersion;  
GO  
```  
  
### <a name="c-using-object_id-to-specify-the-value-of-a-system-function-parameter"></a>C. Utilisation de OBJECT_ID pour spécifier la valeur d'un paramètre d'une fonction système  
 L’exemple suivant retourne des informations pour tous les index et partitions de la table `Person.Address` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] à l’aide de la fonction [sys.dm_db_index_operational_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md).  
 
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
> [!IMPORTANT]  
>  Lorsque vous utilisez les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] DB_ID et OBJECT_ID pour obtenir la valeur d'un paramètre, assurez-vous toujours que l'ID retourné est valide. Si le nom de la base de données ou de l'objet est introuvable, par exemple s'il n'existe pas ou n'est pas correctement orthographié, les deux fonctions retournent la valeur NULL. La fonction **sys.dm_db_index_operational_stats** interprète la valeur NULL comme une valeur générique qui désigne toutes les bases de données ou tous les objets. Comme il peut s'agir d'une opération non intentionnelle, les exemples fournis dans cette section présentent une méthode sûre pour déterminer les ID de base de données et d'objet.
  
```sql  
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-returning-the-object-id-for-a-specified-object"></a>D. Renvoi de l’identificateur d’un objet spécifié  
 L'exemple suivant renvoie l'ID d'objet de la table `FactFinance` de la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```sql  
SELECT OBJECT_ID('AdventureWorksPDW2012.dbo.FactFinance') AS 'Object ID';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)  
  
  


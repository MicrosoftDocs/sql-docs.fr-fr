---
description: sys.sql_expression_dependencies (Transact-SQL)
title: sys.sql_expression_dependencies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_expression_dependencies
- sql_expression_dependencies_TSQL
- sql_expression_dependencies
- sys.sql_expression_dependencies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_expression_dependencies catalog view
ms.assetid: 78a218e4-bf99-4a6a-acbf-ff82425a5946
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34c88c909239e7f9d9fcb12e5b30677708d9cfbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210323"
---
# <a name="syssql_expression_dependencies-transact-sql"></a>sys.sql_expression_dependencies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque dépendance par nom sur une entité définie par l'utilisateur dans la base de données actuelle. Cela comprend les dépendances entre les fonctions scalaires définies par l’utilisateur compilées en mode natif et d’autres [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] modules. Une dépendance entre deux entités est créée lorsqu’une entité, appelée *entité référencée*, apparaît par nom dans une expression SQL rendue persistante d’une autre entité, appelée *entité de référence*. Par exemple, lorsqu'une table est référencée dans la définition d'une vue, la vue, comme entité de référence, dépend de la table, l'entité référencée. Si la table est supprimée, la vue est inutilisable.  
  
 Pour plus d’informations, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 Vous pouvez utiliser cet affichage catalogue pour signaler des informations de dépendance pour les entités suivantes :  
  
-   Entités liées au schéma.  
  
-   Entités non liées au schéma.  
  
-   Entités des bases de données croisées et entre serveurs. Les noms d'entités sont signalés ; toutefois, les ID d'entité ne sont pas résolus.  
  
-   Dépendances au niveau des colonnes sur les entités liées au schéma. Les dépendances au niveau des colonnes pour les objets non liés au schéma peuvent être retournées à l’aide de [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md).  
  
-   Déclencheurs DDL au niveau du serveur dans le contexte de la base de données master.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|referencing_id|**int**|ID de l'entité de référence. N'accepte pas la valeur NULL.|  
|referencing_minor_id|**int**|ID de colonne lorsque l'entité de référence est une colonne ; sinon 0. N'accepte pas la valeur NULL.|  
|referencing_class|**tinyint**|Classe de l'entité de référence.<br /><br /> 1 = Objet ou colonne<br /><br /> 12 = Déclencheur DDL de base de données<br /><br /> 13 = Déclencheur DDL de serveur<br /><br /> N'accepte pas la valeur NULL.|  
|referencing_class_desc|**nvarchar(60)**|Description de la classe de l'entité de référence.<br /><br /> OBJECT_OR_COLUMN<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER<br /><br /> N'accepte pas la valeur NULL.|  
|is_schema_bound_reference|**bit**|1 = l'entité référencée est liée au schéma.<br /><br /> 0 = l'entité référencée n'est pas liée au schéma.<br /><br /> N'accepte pas la valeur NULL.|  
|referenced_class|**tinyint**|Classe de l'entité référencée.<br /><br /> 1 = Objet ou colonne<br /><br /> 6 = Type<br /><br /> 10 = Collection du schéma XML<br /><br /> 21 = Fonction de partition<br /><br /> N'accepte pas la valeur NULL.|  
|referenced_class_desc|**nvarchar(60)**|Description de la classe de l'entité référencée.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION<br /><br /> N'accepte pas la valeur NULL.|  
|referenced_server_name|**sysname**|Nom du serveur de l'entité référencée.<br /><br /> Cette colonne est remplie pour les dépendances entre serveurs qui sont établies en spécifiant un nom en quatre parties valide. Pour plus d’informations sur les noms en plusieurs parties, consultez conventions de la [syntaxe Transact-sql &#40;&#41;Transact-SQL ](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> NULL pour les entités non liées au schéma pour lesquelles l'entité a été référencée sans spécifier un nom en quatre parties.<br /><br /> NULL pour les entités liées au schéma, car elles doivent se trouver dans la même base de données et, par conséquent, ne peuvent être définies qu’à l’aide d’un nom en deux parties (*Schema. Object*).|  
|referenced_database_name|**sysname**|Nom de la base de données de l'entité référencée.<br /><br /> Cette colonne est remplie pour les références des bases de données croisées et entre serveurs qui sont établies en spécifiant un nom en trois ou quatre parties valide.<br /><br /> NULL pour les références non liées au schéma en cas de spécification à l'aide d'un nom en une ou deux parties.<br /><br /> NULL pour les entités liées au schéma, car elles doivent se trouver dans la même base de données et, par conséquent, ne peuvent être définies qu’à l’aide d’un nom en deux parties (*Schema. Object*).|  
|referenced_schema_name|**sysname**|Schéma auquel l'entité référencée appartient.<br /><br /> NULL pour les références non liées au schéma dans lesquelles l'entité a été référencée sans spécifier le nom de schéma.<br /><br /> Jamais NULL pour les références liées au schéma, car les entités liées au schéma doivent être définies et référencées en utilisant un nom en deux parties.|  
|referenced_entity_name|**sysname**|Nom de l'entité référencée. N'accepte pas la valeur NULL.|  
|referenced_id|**int**|ID de l'entité référencée. La valeur de cette colonne n’est jamais NULL pour les références liées au schéma. La valeur de cette colonne est toujours NULL pour les références entre serveurs et les bases de données croisées.<br /><br /> NULL pour les références dans la base de données si l'ID ne peut pas être déterminé. Pour les références non liées au schéma, l'ID ne peut pas être résolu dans les cas suivants :<br /><br /> L'entité référencée n'existe pas dans la base de données.<br /><br /> Le schéma de l'entité référencée dépend du schéma de l'appelant et est résolu au moment de l'exécution. Dans ce cas, is_caller_dependent a la valeur 1.|  
|referenced_minor_id|**int**|ID de la colonne référencée lorsque l'entité de référence est une colonne ; sinon 0. N'accepte pas la valeur NULL.<br /><br /> Une entité référencée est une colonne lorsqu'une colonne est identifiée par son nom dans l'entité de référence, ou lorsque l'entité parente est utilisée dans une instruction SELECT *.|  
|is_caller_dependent|**bit**|Indique que la liaison de schéma pour l'entité référencée se produit au moment de l'exécution ; par conséquent, la résolution de l'ID d'entité dépend du schéma de l'appelant. Cela se produit lorsque l'entité référencée est une procédure stockée, procédure stockée étendue ou fonction définie par l'utilisateur non liée au schéma appelée dans une instruction EXECUTE.<br /><br /> 1 = l'entité référencée dépend de l'appelant et est résolue au moment de l'exécution. Dans ce cas, referenced_id a la valeur NULL.<br /><br /> 0 = l'ID de l'entité référencée ne dépend pas de l'appelant.<br /><br /> Toujours 0 pour les références liées au schéma et pour les références des bases de données croisées et entre serveurs qui spécifient explicitement un nom de schéma. Par exemple, une référence à une entité au format `EXEC MyDatabase.MySchema.MyProc` ne dépend pas de l'appelant. Toutefois, une référence au format `EXEC MyDatabase..MyProc` dépend de l'appelant.|  
|is_ambiguous|**bit**|Indique que la référence est ambiguë et peut être résolue au moment de l’exécution en une fonction définie par l’utilisateur, un type défini par l’utilisateur (UDT) ou une référence XQuery à une colonne de type **XML**.<br /><br /> Par exemple, supposez que l'instruction `SELECT Sales.GetOrder() FROM Sales.MySales` est définie dans une procédure stockée. Jusqu'à ce que la procédure stockée soit exécutée, il n'est pas possible de savoir si `Sales.GetOrder()` est une fonction définie par l'utilisateur dans le schéma `Sales` ou une colonne nommée `Sales` de type défini par l'utilisateur avec une méthode nommée `GetOrder()`.<br /><br /> 1 = la référence est ambiguë.<br /><br /> 0 = la référence n'est pas équivoque ou l'entité peut être liée avec succès lorsque la vue est appelée.<br /><br /> Toujours 0 pour les références liées au schéma.|  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant répertorie les types des entités pour lesquelles les informations de dépendance sont créées et gérées. Les informations de dépendance ne sont pas créées ni gérées pour les règles, les valeurs par défaut, les tables temporaires, les procédures stockées temporaires ou les objets système.  

> [!NOTE]
> Azure Synapse Analytics et Parallel Data Warehouse prennent en charge les tables, les vues, les statistiques filtrées et les types d’entité de procédures stockées Transact-SQL dans cette liste.  Les informations de dépendance sont créées et gérées uniquement pour les tables, les vues et les statistiques filtrées.  
  
|Type d'entité|Entité de référence|Entité référencée|  
|-----------------|------------------------|-----------------------|  
|Table de charge de travail|Oui*|Oui|  
|Affichage|Oui|Oui|  
|Index filtré|Oui**|Non|  
|Statistiques filtrées|Oui**|Non|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée * * _|Oui|Oui|  
|Procédure stockée CLR|Non|Oui|  
|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l'utilisateur|Oui|Oui|  
|Fonction CLR définie par l'utilisateur|Non|Oui|  
|Déclencheur CLR (DML et DDL)|Non|Non|  
|Déclencheur DML [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|Non|  
|Déclencheur DDL au niveau de la base de données [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|Non|  
|Déclencheur DDL au niveau du serveur [!INCLUDE[tsql](../../includes/tsql-md.md)]|Oui|Non|  
|Procédures stockées étendues|Non|Oui|  
|File d’attente|Non|Oui|  
|Synonyme|Non|Oui|  
|Type (alias et type CLR défini par l'utilisateur)|Non|Oui|  
|Collection de schémas XML|Non|Oui|  
|Fonction de partition|Non|Oui|  
  
 \_ Une table est suivie en tant qu’entité de référence uniquement lorsqu’elle fait référence à un [!INCLUDE[tsql](../../includes/tsql-md.md)] module, un type défini par l’utilisateur ou une collection de schémas XML dans la définition d’une colonne calculée, d’une contrainte CHECK ou d’une contrainte default.  
  
 ** Chaque colonne utilisée dans le prédicat de filtre est suivie en tant qu'entité de référence.  
  
 * * Les procédures stockées numérotées avec une valeur entière supérieure à 1 ne sont pas suivies en tant qu’entité de référence ou référencée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DEFINITION sur la base de données et l'autorisation SELECT sur sys.sql_expression_dependencies pour la base de données. Par défaut, l'autorisation SELECT est accordée uniquement aux membres du rôle de base de données fixe db_owner. Lorsque les autorisations SELECT et VIEW DEFINITION sont accordées à un autre utilisateur, le bénéficiaire peut consulter toutes les dépendances dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-entities-that-are-referenced-by-another-entity"></a>R. Retour d'entités qui sont référencées par une autre entité  
 L'exemple suivant retourne les tables et colonnes référencées dans la vue `Production.vProductAndDescription`. La vue dépend des entités (tables et colonnes) retournées dans les colonnes `referenced_entity_name` et `referenced_column_name`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
GO  
  
```  
  
### <a name="b-returning-entities-that-reference-another-entity"></a>B. Retour d'entités qui référencent une autre entité  
 L'exemple suivant retourne les entités qui référencent la table `Production.Product`. Les entités retournées dans la colonne `referencing_entity_name` dépendent de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
    OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referenced_id = OBJECT_ID(N'Production.Product');  
GO  
  
```  
  
### <a name="c-returning-cross-database-dependencies"></a>C. Retour de dépendances de bases de données croisées  
 L'exemple suivant retourne toutes les dépendances de bases de données croisées. L'exemple crée d'abord la base de données `db1` et deux procédures stockées qui référencent les tables dans les bases de données `db2` et `db3`. La table `sys.sql_expression_dependencies` est alors interrogée pour signaler les dépendances de bases de données croisées entre les procédures et les tables. Remarquez que la valeur NULL est retournée dans la colonne `referenced_schema_name` pour l'entité référencée `t3`, car aucun nom de schéma n'a été spécifié pour cette entité dans la définition de la procédure.  
  
```  
CREATE DATABASE db1;  
GO  
USE db1;  
GO  
CREATE PROCEDURE p1 AS SELECT _ FROM db2.s1.t1;  
GO  
CREATE PROCEDURE p2 AS  
    UPDATE db3..t3  
    SET c1 = c1 + 1;  
GO  
SELECT OBJECT_NAME (referencing_id),referenced_database_name,   
    referenced_schema_name, referenced_entity_name  
FROM sys.sql_expression_dependencies  
WHERE referenced_database_name IS NOT NULL;  
GO  
USE master;  
GO  
DROP DATABASE db1;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
  

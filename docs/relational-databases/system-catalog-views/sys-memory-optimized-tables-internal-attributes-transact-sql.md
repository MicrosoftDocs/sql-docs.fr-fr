---
description: sys.memory_optimized_tables_internal_attributes (Transact-SQL)
title: sys.memory_optimized_tables_internal_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.memory_optimized_tables_internal_attributes
- sys.memory_optimized_tables_internal_attributes_TSQL
- memory_optimized_tables_internal_attributes
- memory_optimized_tables_internal_attributes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.memory_optimized_tables_internal_attributes catalog view
ms.assetid: 78ef5807-0504-4de8-9a01-ede6c03c7ff1
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29d087c2501bd4296c3c60b9cefb4ca28be93fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191331"
---
# <a name="sysmemory_optimized_tables_internal_attributes-transact-sql"></a>sys.memory_optimized_tables_internal_attributes (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Contient une ligne pour chaque table optimisée en mémoire interne utilisé pour stocker les tables utilisateur optimisées en mémoire. Chaque table utilisateur correspond à une ou plusieurs tables internes. Une seule table est utilisée pour le stockage de données principal. Les tables internes supplémentaires servent à la prise en charge des fonctionnalités telles que les index columnstore temporels et le stockage (LOB) hors ligne des tables optimisées en mémoire.
 
| Nom de la colonne  | Type de données  | Description |
| :------ |:----------| :-----|
|object_id  |**int**|       Identifiant de la table utilisateur. Les tables optimisées en mémoire internes destinées à la prise en charge d’une table utilisateur (par exemple, le stockage hors ligne ou les lignes supprimées dans le cas de combinaisons Hk/Columnstore) ont le même object_id que leurs parents. |
|xtp_object_id  |**bigint**|    ID de l’objet In-Memory OLTP correspondant à la table optimisée en mémoire interne utilisée pour prendre en charge la table utilisateur. Il est unique dans la base de données, et peut évoluer au fil de la durée de vie de l’objet. 
|type|  **int** |   Type de la table interne.<br/><br/> 0 => DELETED_ROWS_TABLE <br/> 1 => USER_TABLE <br/> 2 => DICTIONARIES_TABLE<br/>3 => SEGMENTS_TABLE<br/>4 => ROW_GROUPS_INFO_TABLE<br/>5 => INTERNAL OFF-ROW DATA TABLE<br/>252 => INTERNAL_TEMPORAL_HISTORY_TABLE | 
|type_desc| **nvarchar(60)**|   Description du type<br/><br/>DELETED_ROWS_TABLE -> Lignes supprimées de suivi de table interne pour un index columnstore<br/>USER_TABLE -> Table contenant les données de ligne utilisateur<br/>DICTIONARIES_TABLE -> Dictionnaires pour un index columnstore<br/>SEGMENTS_TABLE -> Segments compressés pour un index columnstore<br/>ROW_GROUPS_INFO_TABLE -> Métadonnées à propos des groupes de lignes compressés d’un index columnstore<br/>INTERNAL OFF-ROW DATA TABLE -> Table interne utilisée pour le stockage d’une colonne hors ligne. Dans ce cas, minor_id reflète column_id.<br/>INTERNAL_TEMPORAL_HISTORY_TABLE -> Fin à chaud de la table d’historique basée sur le disque. Tout d’abord, les lignes insérées dans l’historique sont insérées dans cette table optimisée en mémoire interne. Il existe une tâche en arrière-plan qui déplace de manière asynchrone les lignes de cette table interne vers la table d’historique sur disque. |
|minor_id|  **int**|    0 indique une table utilisateur ou interne<br/><br/>Non-0 indique l’ID d’une colonne stockée hors ligne. Se joint à column_id dans sys.columns.<br/><br/>Chaque colonne stockée hors ligne a une ligne correspondante dans cette vue système.|

## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-columns-that-are-stored-off-row"></a>R. Renvoi de toutes les colonnes qui sont stockées hors ligne

Le script T-SQL suivant illustre une table avec plusieurs grandes colonnes non-LOB et une seule colonne LOB :

```Transact-SQL
CREATE TABLE dbo.LargeTableSample
(
      Id   int IDENTITY PRIMARY KEY NONCLUSTERED,
      C1   nvarchar(4000),
      C2   nvarchar(4000),
      C3   nvarchar(4000),
      C4   nvarchar(4000),
      Misc nvarchar(max)
) WITH (MEMORY_OPTIMIZED = ON);
GO
```

La requête suivante affiche toutes les colonnes qui sont stockées hors ligne, ainsi que leur taille. Une taille de -1 indique une colonne LOB. Toutes les colonnes LOB sont stockées hors ligne.

```Transact-SQL
SELECT 
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table', 
  c.name AS 'column', 
  c.max_length
FROM sys.memory_optimized_tables_internal_attributes moa
     JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
     JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="b-returning-memory-consumption-of-all-columns-that-are-stored-off-row"></a>B. Retour de la consommation de mémoire de toutes les colonnes qui sont stockées hors ligne

Pour obtenir plus d’informations sur la consommation de mémoire des colonnes hors ligne, vous pouvez utiliser la requête suivante, qui montre la consommation de mémoire de toutes les tables internes et de leurs index utilisés pour stocker les colonnes hors ligne :

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  c.name AS 'column',
  c.max_length,
  mc.memory_consumer_desc,
  mc.index_id,
  mc.allocated_bytes,
  mc.used_bytes
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="c-returning-memory-consumption-of-columnstore-indexes-on-memory-optimized-tables"></a>C. Retour de la consommation de mémoire des index columnstore sur les tables optimisées en mémoire

Utilisez la requête suivante pour afficher la consommation de mémoire des index ColumnStore sur les tables optimisées en mémoire :

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  SUM(mc.allocated_bytes) / 1024 as [allocated_kb],
  SUM(mc.used_bytes) / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
GROUP BY o.schema_id, moa.object_id, i.name;
```

Utilisez la requête suivante pour réduire la consommation de mémoire dans les structures internes utilisées pour les index ColumnStore sur les tables optimisées en mémoire :

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  moa.type_desc AS 'internal table',
  mc.index_id AS 'index',
  mc.memory_consumer_desc,
  mc.allocated_bytes / 1024 as [allocated_kb],
  mc.used_bytes / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
```



---
description: sys.dm_db_xtp_hash_index_stats (Transact-SQL)
title: sys.dm_db_xtp_hash_index_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_hash_index_stats
- sys.dm_db_xtp_hash_index_stats_TSQL
- dm_db_xtp_hash_index_stats
- dm_db_xtp_hash_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_hash_index_stats (dynamic management view)
ms.assetid: 45969884-cd61-48e8-aee5-c725c78e3e4c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 239ca2e5a60f8ffeeafd807316e5a903dc840754
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092872"
---
# <a name="sysdm_db_xtp_hash_index_stats-transact-sql"></a>sys.dm_db_xtp_hash_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Ces statistiques sont utiles pour comprendre et ajuster le nombre de compartiments. Elles peuvent également être utilisées pour détecter les cas où la clé d'index possède un grand nombre de doublons.  
  
 Une longueur de chaîne moyenne élevée indique que de nombreuses lignes sont hachées dans le même compartiments. Cela peut se produire si :  
  
-   Le nombre de compartiments vides est faible ou les longueurs de chaîne moyenne et maximale sont similaires. Il est probable que le nombre de compartiments est trop bas. Cela entraîne le hachage de plusieurs clés d'index dans le même compartiment.  
  
-   Le nombre de compartiments vides est élevé ou la longueur de chaîne maximale est élevée par rapport à la longueur de chaîne moyenne. Il est probable qu'il existe plusieurs lignes avec des valeurs de clés d'index dupliquées ou des valeurs de clé sont biaisées. Toutes les lignes avec la même valeur de clé d'index sont hachées dans le même compartiment, par conséquent, il existe une chaîne de type Long dans ce compartiment.  
  
Les chaînes de type Long peuvent affecter les performances des opérations DML sur des lignes, notamment SELECT et INSERT. Les chaînes de type Short avec un nombre de compartiments vides élevé sont une indication de bucket_count trop élevé. Cela altère les performances des analyses d'index.  
  
> [!WARNING]
> **sys.dm_db_xtp_hash_index_stats** analyse la totalité de la table. Ainsi, s’il existe de grandes tables dans votre base de données, l’exécution de **sys.dm_db_xtp_hash_index_stats** peut prendre beaucoup de temps.  
  
Pour plus d’informations, consultez [index de hachage pour les Tables Memory-Optimized](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|object_id|**int**|ID d'objet d'une table parent.|  
|xtp_object_id|**bigint**|ID de la table optimisée en mémoire.|  
|index_id|**int**|ID d'index.|  
|total_bucket_count|**bigint**|Nombre total de compartiments de hachage dans l'index.|  
|empty_bucket_count|**bigint**|Nombre total de compartiments de hachage vides dans l'index.|  
|avg_chain_length|**bigint**|Longueur moyenne des chaînes de ligne sur tous les compartiments de hachage dans l'index.|  
|max_chain_length|**bigint**|Longueur maximale des chaînes de ligne dans les compartiments de hachage.|  
|xtp_object_id|**bigint**|ID d’objet OLTP en mémoire qui correspond à la table optimisée en mémoire.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  

## <a name="examples"></a>Exemples  
  
### <a name="a-troubleshooting-hash-index-bucket-count"></a>R. Résolution de problèmes liés au nombre de compartiments d’index de hachage

La requête suivante peut être utilisée pour résoudre les problèmes liés au nombre de compartiments d’index de hachage d’une table existante. La requête retourne des statistiques sur le pourcentage de compartiments vides et la longueur de chaîne pour tous les index de hachage sur les tables utilisateur.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    FLOOR((  
      CAST(h.empty_bucket_count as float) /  
        h.total_bucket_count) * 100)  
                             as [empty_bucket_percent],  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type=1
  ORDER BY [table], [index];  
``` 

Pour plus d’informations sur la façon d’interpréter les résultats de cette requête, consultez [résolution des problèmes liés aux index de hachage pour les Tables Memory-Optimized](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md) .  

### <a name="b-hash-index-statistics-for-internal-tables"></a>B. Statistiques d’index de hachage pour les tables internes

Certaines fonctionnalités utilisent des tables internes qui tirent parti des index de hachage, par exemple les index ColumnStore sur les tables optimisées en mémoire. La requête suivante retourne des statistiques pour les index de hachage sur les tables internes qui sont liées aux tables utilisateur.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [user_table],
    ia.type_desc as [internal_table_type],
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type!=1
  ORDER BY [user_table], [internal_table_type], [index]; 
```

Notez que la BUCKET_COUNT de l’index sur les tables internes ne peut pas être modifiée. par conséquent, la sortie de cette requête doit être considérée comme informatif uniquement. Aucune action n'est requise.  

Cette requête ne doit retourner aucune ligne, sauf si vous utilisez une fonctionnalité qui exploite les index de hachage sur les tables internes. La table mémoire optimisée suivante contient un index ColumnStore. Après avoir créé cette table, vous verrez des index de hachage sur les tables internes.

```sql
  CREATE TABLE dbo.table_columnstore
  (
    c1 INT NOT NULL PRIMARY KEY NONCLUSTERED,
    INDEX ix_columnstore CLUSTERED COLUMNSTORE
  ) WITH (MEMORY_OPTIMIZED=ON)
```

## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  

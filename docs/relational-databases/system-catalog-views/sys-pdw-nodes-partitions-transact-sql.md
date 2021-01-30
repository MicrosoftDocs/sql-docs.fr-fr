---
description: sys.pdw_nodes_partitions (Transact-SQL)
title: sys.pdw_nodes_partitions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: b4216752-4813-4b2c-b259-7d8ffc6cc190
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c4c6e3ce34f1fbd73bf046b9fdeb178d82878f6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207833"
---
# <a name="syspdw_nodes_partitions-transact-sql"></a>sys.pdw_nodes_partitions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient une ligne pour chaque partition de toutes les tables, et la plupart des types d’index dans une [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] base de données. Toutes les tables et tous les index contiennent au moins une partition, qu’elles soient ou non explicitement partitionnées.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|ID de la partition. Unique dans une base de données.|  
|object_id|**int**|ID de l'objet auquel cette partition appartient. Chaque table ou vue comporte au moins une partition.|  
|index_id|**int**|ID de l'index de l'objet auquel cette partition appartient.|  
|partition_number|**int**|Numéro de partition de base 1 dans l’index ou le segment de mémoire propriétaire. Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , la valeur de cette colonne est 1.|  
|hobt_id|**bigint**|ID de la segment de mémoire ou arbre B (B-tree) de données (HoBT) qui contient les lignes de cette partition.|  
|rows|**bigint**|Nombre approximatif de lignes dans cette partition. |  
|data_compression|**int**|Indique l'état de compression pour chaque partition :<br /><br /> 0 = AUCUN<br /><br /> 1 = LIGNE<br /><br /> 2 = PAGE<br /><br /> 3 = COLUMNSTORE|  
|data_compression_desc|**nvarchar(60)**|Indique l'état de compression pour chaque partition. Les valeurs possibles sont NONE, ROW et PAGE.|  
|pdw_node_id|**int**|Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `CONTROL SERVER`.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

### <a name="example-a-display-rows-in-each-partition-within-each-distribution"></a>Exemple A : afficher des lignes dans chaque partition au sein de chaque distribution 

**S’applique à :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 
Pour afficher le nombre de lignes dans chaque partition au sein de chaque distribution, utilisez [DBCC PDW_SHOWPARTITIONSTATS (SQL Server PDW)](../../t-sql/database-console-commands/dbcc-pdw-showpartitionstats-transact-sql.md) .

### <a name="example-b-uses-system-views-to-view-rows-in-each-partition-of-each-distribution-of-a-table"></a>Exemple B : utilise des vues système pour afficher des lignes dans chaque partition de chaque distribution d’une table

**S’applique à :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
 
Cette requête retourne le nombre de lignes dans chaque partition de chaque distribution de la table `myTable` .  
 
```sql  
SELECT o.name, pnp.index_id, pnp.partition_id, pnp.rows,   
    pnp.data_compression_desc, pnp.pdw_node_id  
FROM sys.pdw_nodes_partitions AS pnp  
JOIN sys.pdw_nodes_tables AS NTables  
    ON pnp.object_id = NTables.object_id  
AND pnp.pdw_node_id = NTables.pdw_node_id  
JOIN sys.pdw_table_mappings AS TMap  
    ON NTables.name = TMap.physical_name 
    AND substring(TMap.physical_name,40, 10) = pnp.distribution_id 
JOIN sys.objects AS o  
    ON TMap.object_id = o.object_id  
WHERE o.name = 'myTable'  
ORDER BY o.name, pnp.index_id, pnp.partition_id;  
```    
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  


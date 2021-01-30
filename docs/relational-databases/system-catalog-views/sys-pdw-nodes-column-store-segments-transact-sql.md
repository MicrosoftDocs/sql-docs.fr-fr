---
description: sys.pdw_nodes_column_store_segments (Transact-SQL)
title: sys.pdw_nodes_column_store_segments (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/28/2018
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: e2fdf8e9-1b74-4682-b2d4-c62aca053d7f
author: julieMSFT
ms.author: jrasnick
manager: jrj
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08e99e71c4086f60b869b9d25c6b0d0f4deec35d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186559"
---
# <a name="syspdw_nodes_column_store_segments-transact-sql"></a>sys.pdw_nodes_column_store_segments (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Contient une ligne pour chaque colonne dans un index columnstore.

| Nom de la colonne                 | Type de données  | Description                                                  |
| :-------------------------- | :--------- | :----------------------------------------------------------- |
| **partition_id**            | **bigint** | Indique l'ID de partition. Unique dans une base de données.     |
| **hobt_id**                 | **bigint** | ID du segment ou de l'index d'arbre B (B-tree) pour la table ayant cet index columnstore. |
| **column_id**               | **int**    | ID de la colonne columnstore.                                |
| **segment_id**              | **int**    | Retourne le segment de la colonne. Pour la compatibilité descendante, le nom de colonne continue d’être appelé segment_id même s’il s’agit de l’ID rowgroup. Vous pouvez identifier un segment de manière unique à l’aide de <hobt_id, partition_id, column_id>, <segment_id>. |
| **version**                 | **int**    | Version du format de segment de colonne.                        |
| **encoding_type**           | **int**    | Type d’encodage utilisé pour ce segment :<br /><br /> 1 = VALUE_BASED non chaîne/binaire sans dictionnaire (semblable à 4 avec certaines variations internes)<br /><br /> 2 = VALUE_HASH_BASED-colonne non chaîne/binaire avec des valeurs communes dans le dictionnaire<br /><br /> 3 = STRING_HASH_BASED-chaîne/colonne binaire avec des valeurs communes dans le dictionnaire<br /><br /> 4 = STORE_BY_VALUE_BASED-non-chaîne/binaire sans dictionnaire<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED chaîne/binaire sans dictionnaire<br /><br /> Tous les encodages tirent parti de l’encodage de bits et de la longueur d’exécution lorsque cela est possible. |
| **row_count**               | **int**    | Nombre de lignes dans le groupe de lignes.                             |
| **has_nulls**               | **int**    | 1 si le segment de colonne a des valeurs NULL.                     |
| **base_id**                 | **bigint** | ID de la valeur de base si le type d’encodage 1 est utilisé.  Si le type d’encodage 1 n’est pas utilisé, base_id a la valeur 1. |
| **magnitude**               | **float**  | Magnitude si le type d’encodage 1 est utilisé.  Si le type d’encodage 1 n’est pas utilisé, l’argument magnitude a la valeur 1. |
| **primary__dictionary_id**  | **int**    | ID du dictionnaire principal. Une valeur différente de zéro pointe vers le dictionnaire local de cette colonne dans le segment actuel (c’est-à-dire, rowgroup). La valeur-1 indique qu’il n’existe aucun dictionnaire local pour ce segment. |
| **secondary_dictionary_id** | **int**    | ID du dictionnaire secondaire. Une valeur différente de zéro pointe vers le dictionnaire local de cette colonne dans le segment actuel (c’est-à-dire, rowgroup). La valeur-1 indique qu’il n’existe aucun dictionnaire local pour ce segment. |
| **min_data_id**             | **bigint** | ID de données minimal dans le segment de colonne.                       |
| **max_data_id**             | **bigint** | ID de données maximal dans le segment de colonne.                       |
| **null_value**              | **bigint** | Valeur utilisée pour représenter les valeurs NULL.                               |
| **on_disk_size**            | **bigint** | Taille de segment en octets.                                    |
| **pdw_node_id**             | **int**    | Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Joignez sys.pdw_nodes_column_store_segments avec d’autres tables système pour déterminer le nombre de segments ColumnStore par table logique.

```sql
SELECT  sm.name           as schema_nm
,       tb.name           as table_nm
,       nc.name           as col_nm
,       nc.column_id
,       COUNT(*)          as segment_count
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb                   ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp       ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt         ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[pdw_nodes_partitions] np     ON  np.[object_id]          = nt.[object_id]
                                          AND np.[pdw_node_id]        = nt.[pdw_node_id]
                                          AND np.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_nodes_columns] nc        ON  np.[object_id]          = nc.[object_id]
                                          AND np.[pdw_node_id]        = nc.[pdw_node_id]
                                          AND np.[distribution_id]    = nc.[distribution_id]
JOIN    sys.[pdw_nodes_column_store_segments] rg  ON  rg.[partition_id]         = np.[partition_id]
                                                      AND rg.[pdw_node_id]      = np.[pdw_node_id]
                                                      AND rg.[distribution_id]  = np.[distribution_id]
                                                      AND rg.[column_id]        = nc.[column_id]
GROUP BY    sm.name
,           tb.name
,           nc.name
,           nc.column_id  
ORDER BY    table_nm
,           nc.column_id
,           sm.name ;
```

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation **VIEW SERVER STATE**.

## <a name="see-also"></a>Voir aussi

[Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)  
[sys.pdw_nodes_column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
[sys.pdw_nodes_column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)

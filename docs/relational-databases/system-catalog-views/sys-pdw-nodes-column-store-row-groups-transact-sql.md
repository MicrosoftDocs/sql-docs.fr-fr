---
description: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
title: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 08/05/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 17a4c925-d4b5-46ee-9cd6-044f714e6f0e
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d4b9ea267b0dbf23ff304dc7632a2fdc742d37dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186573"
---
# <a name="syspdw_nodes_column_store_row_groups-transact-sql"></a>sys.pdw_nodes_column_store_row_groups (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Fournit des informations d’index ColumnStore en cluster sur une base par segment pour aider l’administrateur à prendre des décisions de gestion du système dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . **sys.pdw_nodes_column_store_row_groups** contient une colonne pour le nombre total de lignes stockées physiquement (y compris celles marquées comme supprimées) et une colonne pour le nombre de lignes marquées comme supprimées. Utilisez **sys.pdw_nodes_column_store_row_groups** pour déterminer les groupes de lignes qui ont un pourcentage élevé de lignes supprimées et doivent être reconstruites.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table sous-jacente. Il s’agit de la table physique sur le nœud de calcul, et non de la object_id de la table logique sur le nœud de contrôle. Par exemple, object_id ne correspond pas à la object_id dans sys. tables.<br /><br /> Pour effectuer une jointure avec sys. tables, utilisez sys.pdw_index_mappings.|  
|**index_id**|**int**|ID de l’index ColumnStore cluster sur *object_id* table.|  
|**partition_number**|**int**|ID de la partition de table qui contient les *row_group_id* de groupe de lignes. Vous pouvez utiliser *partition_number* pour joindre cette DMV à sys. partitions.|  
|**row_group_id**|**int**|ID de ce groupe de lignes. Cet ID est unique dans la partition.|  
|**dellta_store_hobt_id**|**bigint**|hobt_id des groupes de lignes delta, ou NULL si le type de groupe de lignes n'est pas delta. Un groupe de lignes delta est un groupe de lignes en lecture/écriture qui reçoit des enregistrements. Un groupe de lignes Delta a l’état **ouvert** . Un groupe de lignes delta est toujours au format rowstore et n'a pas été compressé au format columnstore.|  
|**state**|**tinyint**|Numéro d'ID associé à state_description.<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED|  
|**state_desccription**|**nvarchar(60)**|Description de l'état permanent du groupe de lignes :<br /><br /> OUVRIR : groupe de lignes en lecture/écriture qui accepte les nouveaux enregistrements. Un groupe de lignes ouvert est toujours au format rowstore et n'a pas été compressé au format columnstore.<br /><br /> CLOSEd : groupe de lignes qui a été rempli, mais pas encore compressé par le processus du moteur de Tuple.<br /><br /> Compressed : groupe de lignes qui a été rempli et compressé.|  
|**total_rows**|**bigint**|Nombre total de lignes stockées physiquement dans le groupe de lignes. Certaines peuvent avoir été supprimées, mais elles sont toujours stockées. Le nombre maximal de lignes d'un groupe de lignes est 1 048 576 (hexadécimal FFFFF).|  
|**deleted_rows**|**bigint**|Nombre de lignes physiquement stockées dans le groupe de lignes qui sont marquées pour suppression.<br /><br /> Toujours 0 pour les groupes de lignes DELTA.|  
|**size_in_bytes**|**int**|Taille combinée, en octets, de toutes les pages de ce groupe de lignes. Cette taille n’inclut pas la taille requise pour stocker les métadonnées ou les dictionnaires partagés.|  
|**pdw_node_id**|**int**|ID unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|  
|**distribution_id**|**int**|ID unique de la distribution.|
  
## <a name="remarks"></a>Notes  
 Retourne une ligne pour chaque groupe de lignes columnstore pour chaque table ayant un index columnstore cluster ou non cluster.  
  
 Utilisez **sys.pdw_nodes_column_store_row_groups** pour déterminer le nombre de lignes incluses dans le groupe de lignes et la taille du groupe de lignes.  
  
 Lorsque le nombre de lignes supprimées dans un groupe de lignes atteint un fort pourcentage du nombre total de lignes, la table est moins efficace. Reconstruisez l'index columnstore pour réduire la taille de la table, ce qui réduit les E/S disque nécessaires pour lire la table. Pour reconstruire l’index ColumnStore, utilisez l’option **Rebuild** de l’instruction **ALTER index** .  
  
 Le ColumnStore pouvant être mis à jour insère d’abord de nouvelles données dans un rowgroup **ouvert** , qui est au format rowstore, et est également parfois appelé table Delta.  Une fois qu’un rowgroup ouvert est plein, son état passe à **fermé**. Un rowgroup fermé est compressé au format ColumnStore par le moteur de tuple et l’état passe à **compressé**.  Le moteur de tuple est un processus en arrière-plan qui se réveille régulièrement et vérifie s'il existe des rowgroups fermés prêts à être compressés dans un rowgroup columnstore.  Le moteur de tuple libère également les rowgroups dans lesquels chaque ligne a été supprimée. Les RowGroups désalloués sont marqués comme étant mis **hors** service. Pour exécuter immédiatement le moteur de tuple, utilisez l’option **REorganize** de l’instruction **ALTER index** .  
  
 Lorsqu'un groupe de lignes columnstore est rempli, il est compressé, et cesse de recevoir de nouvelles lignes. Lorsque vous supprimez des lignes d'un groupe compressé, elles sont conservées mais sont marquées comme étant supprimées. Les mises à jour dans un groupe compressé sont implémentées comme une suppression du groupe compressé, et une insertion dans un groupe ouvert.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **VIEW SERVER STATE**.  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant joint la table **sys.pdw_nodes_column_store_row_groups** à d’autres tables système pour retourner des informations sur des tables spécifiques. La colonne calculée `PercentFull` est une estimation de l'efficacité du groupe de lignes. Pour rechercher des informations sur une seule table, supprimez les traits d’Union de commentaire devant la clause WHERE et fournissez un nom de table.  
  
```sql
SELECT IndexMap.object_id,   
  object_name(IndexMap.object_id) AS LogicalTableName,   
  i.name AS LogicalIndexName, IndexMap.index_id, NI.type_desc,   
  IndexMap.physical_name AS PhyIndexNameFromIMap,   
  CSRowGroups.*,  
  100*(ISNULL(deleted_rows,0))/total_rows AS PercentDeletedRows   
FROM sys.tables AS t  
JOIN sys.indexes AS i  
    ON t.object_id = i.object_id  
JOIN sys.pdw_index_mappings AS IndexMap  
    ON i.object_id = IndexMap.object_id  
    AND i.index_id = IndexMap.index_id  
JOIN sys.pdw_nodes_indexes AS NI  
    ON IndexMap.physical_name = NI.name  
    AND IndexMap.index_id = NI.index_id  
JOIN sys.pdw_nodes_column_store_row_groups AS CSRowGroups  
    ON CSRowGroups.object_id = NI.object_id   
    AND CSRowGroups.pdw_node_id = NI.pdw_node_id  
    AND CSRowGroups.distribution_id = NI.distribution_id
    AND CSRowGroups.index_id = NI.index_id      
WHERE total_rows > 0
--WHERE t.name = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, IndexMap.physical_name, pdw_node_id;  
```  

L' [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] exemple suivant compte les lignes par partition pour les magasins de colonnes en cluster, ainsi que le nombre de lignes dans les groupes de lignes ouverts, fermés ou compressés :  

```sql
SELECT
    s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,rg.partition_number AS [Partition Number]
    ,SUM(rg.total_rows) AS [Total Rows]
    ,SUM(CASE WHEN rg.State = 1 THEN rg.Total_rows Else 0 END) AS [Rows in OPEN Row Groups]
    ,SUM(CASE WHEN rg.State = 2 THEN rg.Total_Rows ELSE 0 END) AS [Rows in Closed Row Groups]
    ,SUM(CASE WHEN rg.State = 3 THEN rg.Total_Rows ELSE 0 END) AS [Rows in COMPRESSED Row Groups]
FROM sys.pdw_nodes_column_store_row_groups rg
  JOIN sys.pdw_nodes_tables pt
    ON rg.object_id = pt.object_id
    AND rg.pdw_node_id = pt.pdw_node_id
    AND pt.distribution_id = rg.distribution_id
  JOIN sys.pdw_table_mappings tm
    ON pt.name = tm.physical_name
  INNER JOIN sys.tables t
    ON tm.object_id = t.object_id
  INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
GROUP BY s.name, t.name, rg.partition_number
ORDER BY 1, 2
```
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CRÉER un INDEX COLUMNSTORE &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  
  
  

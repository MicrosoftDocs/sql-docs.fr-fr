---
description: sys.dm_db_partition_stats (Transact-SQL)
title: sys.dm_db_partition_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_partition_stats
- dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_partition_stats dynamic management view
ms.assetid: 9db9d184-b3a2-421e-a804-b18ebcb099b7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe7e6639e8f0b04e8a3482ce56baf732a9113c6d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095149"
---
# <a name="sysdm_db_partition_stats-transact-sql"></a>sys.dm_db_partition_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie le nombre de pages et de lignes de chaque partition de la base de données active.  
  
> [!NOTE]  
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_db_partition_stats**. La partition_id dans sys.dm_pdw_nodes_db_partition_stats diffère de la partition_id de l’affichage catalogue sys. partitions pour Azure Synapse Analytics.
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|ID de la partition. Unique dans la base de données. Il s’agit de la même valeur que la **partition_id** dans l’affichage catalogue **sys. partitions** , à l’exception d’Azure Synapse Analytics.|  
|**object_id**|**int**|ID d'objet de la table ou de la vue indexée à laquelle appartient la partition.|  
|**index_id**|**int**|ID du segment de mémoire ou de l'index auquel appartient la partition.<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = Index cluster<br /><br /> > 1 = Index non-cluster|  
|**partition_number**|**int**|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire.|  
|**in_row_data_page_count**|**bigint**|Nombre de pages utilisées pour stocker les données des lignes de cette partition. Si la partition fait partie d'un segment de mémoire, la valeur de cette colonne est le nombre de pages de données dans ce segment. Si la partition fait partie d'un index, la valeur de cette colonne est le nombre de pages au niveau feuille. (Les pages non-feuille dans l’arborescence binaire (B-Tree) ne sont pas incluses dans le nombre.) Les pages IAM (page IAM (Index Allocation Map)) ne sont pas incluses dans les deux cas. Toujours 0 pour un index columnstore optimisé en mémoire xVelocity.|  
|**in_row_used_page_count**|**bigint**|Nombre total de pages utilisées pour stocker et gérer les données des lignes de cette partition. Ce nombre comprend les pages non-feuille dans l'arbre B (B-tree), les pages IAM et toutes les pages incluses dans la colonne **in_row_data_page_count**. Toujours 0 pour un index columnstore.|  
|**in_row_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer les données des lignes de cette partition, que les pages soient utilisées ou non. Toujours 0 pour un index columnstore.|  
|**lob_used_page_count**|**bigint**|Nombre de pages utilisées pour stocker et gérer les colonnes hors ligne **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** et **xml** dans la partition. Les pages IAM sont incluses.<br /><br /> Nombre total d'objets LOB utilisés pour stocker et gérer l'index columnstore dans la partition.|  
|**lob_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer les colonnes hors ligne **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** et **xml** dans la partition, que les pages soient utilisées ou non. Les pages IAM sont incluses.<br /><br /> Nombre total d'objets LOB réservés pour le stockage et la gestion d'un index columnstore dans la partition.|  
|**row_overflow_used_page_count**|**bigint**|Nombre de pages utilisées pour stocker et gérer les colonnes en dépassement de capacité des lignes **varchar**, **nvarchar**, **varbinary** et **sql_variant** de la partition. Les pages IAM sont incluses.<br /><br /> Toujours 0 pour un index columnstore.|  
|**row_overflow_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer les colonnes en dépassement de capacité des lignes **varchar**, **nvarchar**, **varbinary** et **sql_variant** de la partition, que les pages soient utilisées ou non. Les pages IAM sont incluses.<br /><br /> Toujours 0 pour un index columnstore.|  
|**used_page_count**|**bigint**|Nombre total de pages utilisées pour la partition. Calculé en tant que **in_row_used_page_count**  +  **lob_used_page_count**  +  **row_overflow_used_page_count**.|  
|**reserved_page_count**|**bigint**|Nombre total de pages réservées pour la partition. Calculé en tant que **in_row_reserved_page_count**  +  **lob_reserved_page_count**  +  **row_overflow_reserved_page_count**.|  
|**row_count**|**bigint**|Nombre approximatif de lignes dans la partition.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
|**distribution_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> ID numérique unique associé à la distribution.|  
  
## <a name="remarks"></a>Notes  
 **sys.dm_db_partition_stats** affiche des informations sur l'espace utilisé pour stocker et gérer les données LOB des données dans la ligne et les données en dépassement de capacité des lignes de toutes les partitions d'une base de données. Une seule ligne est affichée par partition.  
  
 Les nombres sur lesquels le résultat est basé sont placés en mémoire cache ou stockés sur disque dans diverses tables système.  
  
 Les données dans la ligne, les données LOB et les données en dépassement de capacité des lignes représentent les trois unités d'allocation qui composent une partition. Il est possible d'effectuer des requêtes dans l'affichage catalogue [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) sur les métadonnées de chaque unité d'allocation de la base de données.  
  
 Si le segment de mémoire ou l'index n'est pas partitionné, il se compose d'une partition (dont le numéro est égal à 1) ; par conséquent, une seule ligne est renvoyée pour ce segment ou cet index. Il est possible d'effectuer des requêtes dans l'affichage catalogue [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) sur les métadonnées de chaque partition des tables et des index d'une base de données.  
  
 Le nombre total pour une table ou un index s'obtient en ajoutant les nombres obtenus pour l'ensemble des partitions concernées.  
  
## <a name="permissions"></a>Autorisations  
 Requiert `VIEW DATABASE STATE` `VIEW DEFINITION` les autorisations et pour interroger la vue de gestion dynamique **sys.dm_db_partition_stats** . Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-counts-for-all-partitions-of-all-indexes-and-heaps-in-a-database"></a>R. Renvoi de tous les nombres pour toutes les partitions de tous les index et segments de mémoire d'une base de données  
 Le code exemple suivant affiche tous les nombres pour toutes les partitions de tous les index et segments de mémoire de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats;  
GO  
```  
  
### <a name="b-returning-all-counts-for-all-partitions-of-a-table-and-its-indexes"></a>B. Renvoi de tous les nombres pour toutes les partitions d'une table et de ses index  
 Le code exemple suivant affiche tous les nombres pour toutes les partitions de la table `HumanResources.Employee` et de ses index.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats   
WHERE object_id = OBJECT_ID('HumanResources.Employee');  
GO  
```  
  
### <a name="c-returning-total-used-pages-and-total-number-of-rows-for-a-heap-or-clustered-index"></a>C. Renvoi du nombre total de pages utilisées et du nombre total de lignes d'un segment de mémoire ou d'un index cluster  
 Le code exemple suivant renvoie le nombre total de pages utilisées et le nombre total de lignes du segment de mémoire ou de l'index cluster de la table `HumanResources.Employee`. Du fait que la table `Employee` n'est pas partitionnée par défaut, la somme n'inclut qu'une seule partition.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SUM(used_page_count) AS total_number_of_used_pages,   
    SUM (row_count) AS total_number_of_rows   
FROM sys.dm_db_partition_stats  
WHERE object_id=OBJECT_ID('HumanResources.Employee')    AND (index_id=0 or index_id=1);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  



---
description: sys. dm_column_store_object_pool (Transact-SQL)
title: sys. dm_column_store_object_pool (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a8a58ca7-0a7d-4786-bfd9-e8894bd345dd
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2c53ddf41cd1d1ac1b71b28779e19383d4266834
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89537646"
---
# <a name="sysdm_column_store_object_pool-transact-sql"></a>sys. dm_column_store_object_pool (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

 Retourne le nombre de différents types d’utilisation du pool de mémoire d’objets pour les objets d’index ColumnStore.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|`database_id`|`int`|ID de la base de données. Il est unique au sein d’une instance d’une base de données SQL Server ou d’un serveur de base de données SQL Azure. |  
|`object_id`|`int`|ID de l'objet. L’objet est l’un des object_types. | 
|`index_id`|`int`|ID de l'index columnstore.|  
|`partition_number`|`bigint`|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire. Chaque table ou vue a au moins une partition.| 
|`column_id`|`int`|ID de la colonne columnstore. La valeur est NULL pour DELETE_BITMAP.| 
|`row_group_id`|`int`|ID du rowgroup.|
|`object_type`|`smallint`|1 = COLUMN_SEGMENT<br /><br /> 2 = COLUMN_SEGMENT_PRIMARY_DICTIONARY<br /><br /> 3 = COLUMN_SEGMENT_SECONDARY_DICTIONARY<br /><br /> 4 = COLUMN_SEGMENT_BULKINSERT_DICTIONARY<br /><br /> 5 = COLUMN_SEGMENT_DELETE_BITMAP|  
|`object_type_desc`|`nvarchar(60)`|COLUMN_SEGMENT : un segment de colonne. `object_id` est l’ID de segment. Un segment stocke toutes les valeurs d’une colonne dans un rowgroup. Par exemple, si une table comporte 10 colonnes, il y a 10 segments de colonne par rowgroup. <br /><br /> COLUMN_SEGMENT_PRIMARY_DICTIONARY : dictionnaire global qui contient des informations de recherche pour tous les segments de colonne de la table.<br /><br /> COLUMN_SEGMENT_SECONDARY_DICTIONARY-un dictionnaire local associé à une colonne.<br /><br /> COLUMN_SEGMENT_BULKINSERT_DICTIONARY-une autre représentation du dictionnaire global. Cela fournit une recherche inverse de la valeur à dictionary_id. Utilisé pour créer des segments compressés dans le cadre du moteur de tuple ou du chargement en masse.<br /><br /> COLUMN_SEGMENT_DELETE_BITMAP : bitmap qui effectue le suivi des suppressions de segments. Il y a une image bitmap de suppression par partition.|  
|`access_count`|`int`|Nombre d’accès en lecture ou en écriture à cet objet.|  
|`memory_used_in_bytes`|`bigint`|Mémoire utilisée par cet objet dans le pool d’objets.|  
|`object_load_time`|`datetime`|Heure à laquelle la object_id a été introduite dans le pool d’objets.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] les niveaux Premium, requiert l' `VIEW DATABASE STATE` autorisation dans la base de données. Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] les niveaux standard et de base, nécessite l'  **administrateur du serveur** ou un compte d' **administrateur Azure Active Directory** .   
 
## <a name="see-also"></a>Voir aussi  
  
 [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Surveillance et réglage des performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)  
  
  

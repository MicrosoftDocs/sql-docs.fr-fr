---
description: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4adcc52df1d3039f18ca63cbe93bd355f8df4544
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180664"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Retourne l’activité actuelle des e/s au niveau des lignes, du verrouillage et de la méthode d’accès pour les RowGroups compressés dans un index ColumnStore. Utilisez **sys.dm_db_column_store_row_group_operational_stats** pour suivre la durée pendant laquelle une requête de l’utilisateur doit attendre la lecture ou l’écriture dans un rowgroup ou une partition d’un index ColumnStore compressé, et identifier les RowGroups qui rencontrent une activité d’e/s importante ou des zones réactives.  
  
 Les index ColumnStore en mémoire n’apparaissent pas dans cette vue DMV.  
 
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table avec l’index ColumnStore.|  
|**index_id**|**int**|ID de l'index columnstore.|  
|**partition_number**|**int**|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire.|  
|**row_group_id**|**int**|ID de rowgroup dans l’index ColumnStore. Cela est unique au sein d’une partition.|  
|**scan_count**|**int**|Nombre d’analyses par le biais de rowgroup depuis le dernier redémarrage SQL.|  
|**delete_buffer_scan_count**|**int**|Nombre de fois où le tampon de suppression a été utilisé pour déterminer les lignes supprimées dans ce rowgroup. Cela comprend l’accès à la Hashtable en mémoire et au BTREE sous-jacent.|  
|**index_scan_count**|**int**|Nombre de fois où la partition d’index ColumnStore a été analysée. Cela est identique pour tous les RowGroups de la partition.|  
|**rowgroup_lock_count**|**bigint**|Nombre cumulatif de demandes de verrous pour ce rowgroup depuis le dernier redémarrage SQL.|  
|**rowgroup_lock_wait_count**|**bigint**|Nombre cumulatif de fois où le moteur de base de données a attendu ce verrou rowgroup depuis le dernier redémarrage SQL.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Nombre cumulé de millisecondes pendant lesquelles le moteur de base de données a attendu ce verrou rowgroup depuis le dernier redémarrage SQL.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations suivantes sont nécessaires :  
  
-   Autorisation CONTROL sur la table spécifiée par object_id.  
  
-   Autorisation VIEW DATABASE STATE pour renvoyer des informations sur tous les objets dans la base de données, en utilisant l’objet générique @*object_id* = null  
  
 L'octroi de l'autorisation VIEW DATABASE STATE autorise le renvoi de tous les objets de la base de données, quelles que soient les autorisations CONTROL refusées sur des objets spécifiques.  
  
 Le refus de l'autorisation VIEW DATABASE STATE interdit le retour de tous les objets de la base de données, quelles que soient les autorisations CONTROL accordées sur des objets spécifiques. En outre, lorsque la base de données générique @*database_id*= null est spécifiée, la base de données est omise.  
  
 Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  


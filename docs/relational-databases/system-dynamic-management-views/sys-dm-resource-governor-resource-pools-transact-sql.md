---
description: sys.dm_resource_governor_resource_pools (Transact-SQL)
title: sys.dm_resource_governor_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_resource_pools_TSQL
- dm_resource_governor_resource_pools_TSQL
- sys.dm_resource_governor_resource_pools
- dm_resource_governor_resource_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_resource_pools dynamic management view
ms.assetid: 9bfc926e-d8bc-40f8-9229-ab1f8a1e69c5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 81b2333e94a00c00c6743a51029d2b0e70cd9840
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99135218"
---
# <a name="sysdm_resource_governor_resource_pools-transact-sql"></a>sys.dm_resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations sur l'état et la configuration actuels des pools de ressources, ainsi que sur leurs statistiques.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_resource_governor_resource_pools**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|ID du pool de ressources. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom du pool de ressources. N'accepte pas la valeur NULL.|  
|statistics_start_time|**datetime**|Heure à laquelle les statistiques ont été réinitialisées pour ce pool. N'accepte pas la valeur NULL.|  
|total_cpu_usage_ms|**bigint**|L'utilisation cumulative de l'UC, en millisecondes, depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL.|  
|cache_memory_kb|**bigint**|Utilisation de la mémoire cache totale actuelle en kilo-octets. N'accepte pas la valeur NULL.|  
|compile_memory_kb|**bigint**|Utilisation de la mémoire occultée totale actuelle en kilo-octets (Ko). Cette utilisation est essentiellement destinée à la compilation et l'optimisation, mais d'autres utilisations de la mémoire peuvent exister. N'accepte pas la valeur NULL.|  
|used_memgrant_kb|**bigint**|Quantité totale de la mémoire utilisée (occultée) actuelle provenant des allocations de mémoire. N'accepte pas la valeur NULL.|  
|total_memgrant_count|**bigint**|Nombre cumulatif d'allocations de mémoire dans ce pool de ressources. N'accepte pas la valeur NULL.|  
|total_memgrant_timeout_count|**bigint**|Nombre cumulatif de dépassements du délai d'allocation de mémoire dans ce pool de ressources. N'accepte pas la valeur NULL.|  
|active_memgrant_count|**int**|Nombre actuel d'allocations de mémoire. N'accepte pas la valeur NULL.|  
|active_memgrant_kb|**bigint**|Somme, en kilo-octets (Ko), des allocations de mémoire actuelles. N'accepte pas la valeur NULL.|  
|memgrant_waiter_count|**int**|Nombre de requêtes actuellement en attente d'allocations de mémoire. N'accepte pas la valeur NULL.|  
|max_memory_kb|**bigint**|Quantité maximale de mémoire, en kilo-octets, dont peut disposer le pool de ressources. Cette valeur est basée sur les paramètres actuels et l'état du serveur. N'accepte pas la valeur NULL.|  
|used_memory_kb|**bigint**|Quantité de mémoire utilisée, en kilo-octets, pour le pool de ressources. N'accepte pas la valeur NULL.|  
|target_memory_kb|**bigint**|Quantité de mémoire cible, en kilo-octets, que le pool de ressources tente d'atteindre. Cette valeur est basée sur les paramètres actuels et l'état du serveur. N'accepte pas la valeur NULL.|  
|out_of_memory_count|**bigint**|Nombre d'échecs d'allocations de mémoire dans le pool depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL.|  
|min_cpu_percent|**int**|Configuration actuelle de la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|max_cpu_percent|**int**|Configuration actuelle de la bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|min_memory_percent|**int**|Configuration actuelle de la quantité de mémoire garantie pour toutes les demandes dans le pool de ressources en cas de contention de mémoire. Cette valeur n'est pas partagée avec d'autres pools de ressources. N'accepte pas la valeur NULL.|  
|max_memory_percent|**int**|Configuration actuelle du pourcentage de la mémoire totale du serveur qui peut être utilisé par les demandes dans ce pool de ressources. N'accepte pas la valeur NULL.|  
|cap_cpu_percent|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront. Limite le niveau de bande passante processeur maximal au niveau spécifié. La plage autorisée pour la valeur est comprise entre 1 et 100. N'accepte pas la valeur NULL.|  
|min_iops_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Paramètre des E/S minimales par seconde (IOPS) par volume disque de ce pool. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|max_iops_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Paramètre des E/S maximales par seconde (IOPS) par volume disque de ce pool. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres de pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME ont la valeur 0.|  
|read_io_queued_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total d'entrées/sorties de lecture empilées depuis que le gouverneur de ressources a été réinitialisé. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|read_io_issued_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total d'entrées/sorties de lecture émises depuis que les statistiques du gouverneur de ressources ont été réinitialisées. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|read_io_completed_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total d'entrées/sorties de lecture terminées depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL.|  
|read_io_throttled_total|**int**|Total d'entrées/sorties de lecture limitées depuis que les statistiques du gouverneur de ressources ont été réinitialisées. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|read_bytes_total|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Nombre total d'octets lus depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL.|  
|read_io_stall_total_ms|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Durée totale (en millisecondes) entre l'arrivée des E/S de lecture et la fin de l'opération. N'accepte pas la valeur NULL. |  
|read_io_stall_queued_ms|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Durée totale (en millisecondes) entre l'arrivée des E/S de lecture et leur émission. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.<br /><br /> Pour déterminer si le paramètre d’e/s du pool est à l’origine de la latence, soustrayez **read_io_stall_queued_ms** de **read_io_stall_total_ms**.|  
|write_io_queued_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total des entrées/sorties d'écriture empilées depuis que les statistiques du gouverneur de ressources ont été réinitialisées. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|write_io_issued_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total des entrées/sorties d'écriture émises depuis que les statistiques du gouverneur de ressources ont été réinitialisées. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|write_io_completed_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total des entrées/sorties d'écriture terminées depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL|  
|write_io_throttled_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total des entrées/sorties d'écriture limitées depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL|  
|write_bytes_total|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Nombre total d'octets écrits depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL.|  
|write_io_stall_total_ms|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Durée totale (en millisecondes) entre l'arrivée des E/S d'écriture et la fin de l'opération. N'accepte pas la valeur NULL. |  
|write_io_stall_queued_ms|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Durée totale (en millisecondes) entre l'arrivée des E/S d'écriture et leur émission. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.<br /><br /> Latence introduite par la gouvernance des ressources d'E/S.|  
|io_issue_violations_total|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Total des violations d'émission d'E/S. Autrement dit, le nombre de fois où la fréquence d'émission d'E/S était inférieure à la fréquence réservée. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|io_issue_delay_total_ms|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Durée totale (en millisecondes) entre l'émission planifiée et l'émission réelle des E/S. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Autrement dit, les paramètres du pool de ressources MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME sont 0.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="remarks"></a>Notes  
 Les groupes de charges de travail et les pools de ressources du gouverneur de ressources respectent un mappage de type plusieurs-à-un. De nombreuses statistiques de pool de ressources sont donc dérivées des statistiques de groupe de charges de travail.  
  
 Cette vue de gestion dynamique montre la configuration en mémoire. Pour afficher les métadonnées de configuration stockées, utilisez l’affichage catalogue sys.resource_governor_resource_pools.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  




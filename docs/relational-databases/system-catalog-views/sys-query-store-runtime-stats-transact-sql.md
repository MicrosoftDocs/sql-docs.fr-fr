---
description: sys.query_store_runtime_stats (Transact-SQL)
title: sys.query_store_runtime_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.QUERY_STORE_RUNTIME_STATS_TSQL
- QUERY_STORE_RUNTIME_STATS_TSQL
- SYS.QUERY_STORE_RUNTIME_STATS
- QUERY_STORE_RUNTIME_STATS
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_runtime_stats catalog view
- sys.query_store_runtime_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 54bb057d2e53c38d7a37dbc0d9200513748eb8af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161409"
---
# <a name="sysquery_store_runtime_stats-transact-sql"></a>sys.query_store_runtime_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contient des informations sur les statistiques d’exécution du runtime pour la requête.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**runtime_stats_id**|**bigint**|Identificateur de la ligne représentant les statistiques d’exécution du runtime pour le **plan_id**, **execution_type** et **runtime_stats_interval_id**. Elle est unique uniquement pour les intervalles de statistiques d’exécution précédents. Pour l’intervalle actuellement actif, il peut y avoir plusieurs lignes représentant des statistiques d’exécution pour le plan référencé par **plan_id**, avec le type d’exécution représenté par **execution_type**. En règle générale, une ligne représente les statistiques d’exécution qui sont vidées sur le disque, tandis que d’autres représentent l’État en mémoire. Par conséquent, pour obtenir l’état réel de chaque intervalle, vous devez regrouper les métriques, les regrouper par **plan_id**, les **execution_type** et les **runtime_stats_interval_id**.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**plan_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**execution_type**|**tinyint**|Détermine le type d’exécution de la requête :<br /><br /> 0-exécution normale (terminée avec succès)<br /><br /> 3-exécution abandonnée par le client<br /><br /> 4-exécution abandonnée par l’exception|  
|**execution_type_desc**|**nvarchar(128)**|Description textuelle du champ de type d’exécution :<br /><br /> 0-normal<br /><br /> 3-abandonné<br /><br /> 4-exception|  
|**first_execution_time**|**datetimeoffset**|Heure de la première exécution du plan de requête dans l’intervalle d’agrégation. Cela fait référence à l’heure de fin de l’exécution de la requête.|  
|**last_execution_time**|**datetimeoffset**|Heure de la dernière exécution du plan de requête dans l’intervalle d’agrégation. Cela fait référence à l’heure de fin de l’exécution de la requête.|  
|**count_executions**|**bigint**|Nombre total d’exécutions pour le plan de requête dans l’intervalle d’agrégation.|  
|**avg_duration**|**float**|Durée moyenne du plan de requête dans l’intervalle d’agrégation (signalée en microsecondes).|  
|**last_duration**|**bigint**|Dernière durée du plan de requête dans l’intervalle d’agrégation (signalée en microsecondes).|  
|**min_duration**|**bigint**|Durée minimale du plan de requête dans l’intervalle d’agrégation (signalée en microsecondes).|  
|**max_duration**|**bigint**|Durée maximale du plan de requête dans l’intervalle d’agrégation (signalée en microsecondes).|  
|**stdev_duration**|**float**|Écart type de durée pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).|  
|**avg_cpu_time**|**float**|Temps processeur moyen pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_cpu_time**|**bigint**|Dernier temps processeur pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_cpu_time**|**bigint**|Temps processeur minimal pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_cpu_time**|**bigint**|Temps processeur maximal pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_cpu_time**|**float**|Écart type du temps processeur pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_logical_io_reads**|**float**|Nombre moyen de lectures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation. (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_logical_io_reads**|**bigint**|Nombre de lectures d’e/s logiques pour le plan de requête au cours de l’intervalle d’agrégation. (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_logical_io_reads**|**bigint**|Nombre minimal de lectures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation. (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_logical_io_reads**|**bigint**|Nombre maximal de lectures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation. (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_logical_io_reads**|**float**|Nombre de lectures d’e/s logiques d’écart type pour le plan de requête dans l’intervalle d’agrégation. (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_logical_io_writes**|**float**|Nombre moyen d’écritures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_logical_io_writes**|**bigint**|Nombre d’écritures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_logical_io_writes**|**bigint**|Nombre minimal d’écritures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_logical_io_writes**|**bigint**|Nombre maximal d’écritures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_logical_io_writes**|**float**|Nombre d’écritures d’e/s logiques pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_physical_io_reads**|**float**|Nombre moyen de lectures d’e/s physiques pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_physical_io_reads**|**bigint**|Nombre de lectures d’e/s physiques pour le plan de requête au cours de l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_physical_io_reads**|**bigint**|Nombre minimal de lectures d’e/s physiques pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_physical_io_reads**|**bigint**|Nombre maximal de lectures d’e/s physiques pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_physical_io_reads**|**float**|Le nombre d’e/s physiques lit les écarts types pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_clr_time**|**float**|Temps CLR moyen pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_clr_time**|**bigint**|Heure du dernier CLR du plan de requête dans l’intervalle d’agrégation (signalée en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_clr_time**|**bigint**|Temps CLR minimal pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_clr_time**|**bigint**|Temps CLR maximal pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_clr_time**|**float**|Écart type de temps du CLR pour le plan de requête dans l’intervalle d’agrégation (indiqué en microsecondes).<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_dop**|**float**|DOP moyen (degré de parallélisme) pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_dop**|**bigint**|Dernier DOP (degré de parallélisme) pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_dop**|**bigint**|DOP minimal (degré de parallélisme) pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_dop**|**bigint**|PARALLÉLISME maximal (degré de parallélisme) pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_dop**|**float**|L’écart type de parallélisme (degré de parallélisme) pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_query_max_used_memory**|**float**|Allocation de mémoire moyenne (signalée en nombre de pages de 8 Ko) pour le plan de requête dans l’intervalle d’agrégation. Toujours 0 pour les requêtes utilisant des procédures optimisées en mémoire compilées en mode natif.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_query_max_used_memory**|**bigint**|Dernière allocation de mémoire (signalée comme nombre de pages de 8 Ko) pour le plan de requête dans l’intervalle d’agrégation. Toujours 0 pour les requêtes utilisant des procédures optimisées en mémoire compilées en mode natif.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_query_max_used_memory**|**bigint**|Allocation de mémoire minimale (signalée comme nombre de pages de 8 Ko) pour le plan de requête dans l’intervalle d’agrégation. Toujours 0 pour les requêtes utilisant des procédures optimisées en mémoire compilées en mode natif.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_query_max_used_memory**|**bigint**|Allocation de mémoire maximale (signalée comme nombre de pages de 8 Ko) pour le plan de requête dans l’intervalle d’agrégation. Toujours 0 pour les requêtes utilisant des procédures optimisées en mémoire compilées en mode natif.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_query_max_used_memory**|**float**|Écart type d’allocation de mémoire (signalé comme nombre de pages de 8 Ko) pour le plan de requête dans l’intervalle d’agrégation. Toujours 0 pour les requêtes utilisant des procédures optimisées en mémoire compilées en mode natif.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**avg_rowcount**|**float**|Nombre moyen de lignes retournées pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_rowcount**|**bigint**|Nombre de lignes retournées par la dernière exécution du plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_rowcount**|**bigint**|Nombre minimal de lignes retournées pour le plan de requête dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_rowcount**|**bigint**|Nombre maximal de lignes retournées pour le plan de requête dans l’intervalle d’agrégation.|  
|**stdev_rowcount**|**float**|Nombre d’écarts types de lignes retournées pour le plan de requête dans l’intervalle d’agrégation.|
|**avg_log_bytes_used**|**float**|Nombre moyen d’octets dans le journal de base de données utilisé par le plan de requête, dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**last_log_bytes_used**|**bigint**|Nombre d’octets dans le journal de base de données utilisé par la dernière exécution du plan de requête, dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**min_log_bytes_used**|**bigint**|Nombre minimal d’octets dans le journal de base de données utilisé par le plan de requête, dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**max_log_bytes_used**|**bigint**|Nombre maximal d’octets dans le journal de base de données utilisé par le plan de requête, dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|
|**stdev_log_bytes_used**|**float**|Écart type du nombre d’octets dans le journal de base de données utilisé par un plan de requête, dans l’intervalle d’agrégation.<br/>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne toujours la valeur zéro (0).|  
|**avg_tempdb_space_used**|**float**|Nombre moyen de pages utilisées dans tempdb pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko).<br><br/>**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**last_tempdb_space_used**|**bigint**|Nombre de pages utilisées dans tempdb pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko).<br><br/>**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**min_tempdb_space_used**|**bigint**|Nombre minimal de pages utilisées dans tempdb pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko).<br><br/>**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**max_tempdb_space_used**|**bigint**|Nombre maximal de pages utilisées dans tempdb pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko).<br><br/>**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**stdev_tempdb_space_used**|**float**|Nombre de pages utilisées dans l’écart type tempdb pour le plan de requête dans l’intervalle d’agrégation (exprimé en nombre de pages de 8 Ko).<br><br/>**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**avg_page_server_io_reads**|**float**|Nombre moyen de lectures d’e/s de serveur de pages pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br><br/>**S’applique à :** Azure SQL Database hyperscale</br>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non hyperscale) retourne toujours la valeur zéro (0).|
|**last_page_server_io_reads**|**bigint**|Nombre de lectures d’e/s de serveur de pages pour le plan de requête au cours de l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br><br/>**S’applique à :** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hyperscale</br>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non hyperscale) retourne toujours la valeur zéro (0).|
|**min_page_server_io_reads**|**bigint**|Nombre minimal de lectures d’e/s de serveur de pages pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br><br/>**S’applique à :** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hyperscale</br>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non hyperscale) retourne toujours la valeur zéro (0).|
|**max_page_server_io_reads**|**bigint**|Nombre maximal de lectures d’e/s de serveur de pages pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br><br/>**S’applique à :** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hyperscale</br>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non hyperscale) retourne toujours la valeur zéro (0).|
|**stdev_page_server_io_reads**|**float**|Nombre de lectures d’e/s de serveur de pages pour le plan de requête dans l’intervalle d’agrégation (exprimé sous la forme d’un nombre de pages de 8 Ko lues).<br><br/>**S’applique à :** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hyperscale</br>**Remarque :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (non hyperscale) retourne toujours la valeur zéro (0).|
  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)    
 [Bonnes pratiques relatives au Magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)   
  

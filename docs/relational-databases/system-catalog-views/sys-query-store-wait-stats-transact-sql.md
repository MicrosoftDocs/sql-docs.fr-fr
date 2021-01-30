---
description: sys.query_store_wait_stats (Transact-SQL)
title: sys.query_store_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2517cd093bd840c8cf4cea34073acb8e03a98c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171814"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Contient des informations sur les informations d’attente de la requête.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Identificateur de la ligne représentant les statistiques d’attente pour le plan_id, runtime_stats_interval_id, execution_type et wait_category. Elle est unique uniquement pour les intervalles de statistiques d’exécution précédents. Pour l’intervalle actuellement actif, il peut y avoir plusieurs lignes représentant des statistiques d’attente pour le plan référencé par plan_id, avec le type d’exécution représenté par execution_type et la catégorie d’attente représentée par wait_category. En règle générale, une ligne représente les statistiques d’attente qui sont vidées sur le disque, tandis que d’autres représentent l’État en mémoire. Par conséquent, pour obtenir l’état réel de chaque intervalle, vous devez agréger les métriques, les regrouper par plan_id, les runtime_stats_interval_id, les execution_type et les wait_category. |  
|**plan_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_plan &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_runtime_stats_interval &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|Les types d’attente sont catégorisés à l’aide du tableau ci-dessous, puis le temps d’attente est agrégé sur ces catégories d’attente. Les différentes catégories d’attente nécessitent une analyse de suivi différente pour résoudre le problème, mais les types d’attente de la même catégorie mènent à des expériences de dépannage similaires et le fait de fournir la requête affectée en plus des attentes est la pièce manquante permettant d’effectuer la majorité de ces investigations.|
|**wait_category_desc**|**nvarchar(128)**|Pour une description textuelle du champ de catégorie d’attente, consultez le tableau ci-dessous.|
|**execution_type**|**tinyint**|Détermine le type d’exécution de la requête :<br /><br /> 0-exécution normale (terminée avec succès)<br /><br /> 3-exécution abandonnée par le client<br /><br /> 4-exécution abandonnée par l’exception|  
|**execution_type_desc**|**nvarchar(128)**|Description textuelle du champ de type d’exécution :<br /><br /> 0-normal<br /><br /> 3-abandonné<br /><br /> 4-exception|  
|**total_query_wait_time_ms**|**bigint**|`CPU wait`Durée totale du plan de requête dans l’intervalle d’agrégation et la catégorie d’attente (indiqué en millisecondes).|
|**avg_query_wait_time_ms**|**float**|Durée d’attente moyenne pour le plan de requête par exécution au sein de l’intervalle d’agrégation et de la catégorie d’attente (indiqué en millisecondes).|
|**last_query_wait_time_ms**|**bigint**|Durée de la dernière attente pour le plan de requête dans l’intervalle d’agrégation et la catégorie d’attente (signalée en millisecondes).|
|**min_query_wait_time_ms**|**bigint**|`CPU wait`Durée minimale pour le plan de requête dans l’intervalle d’agrégation et la catégorie d’attente (signalée en millisecondes).|
|**max_query_wait_time_ms**|**bigint**|`CPU wait`Durée maximale pour le plan de requête dans l’intervalle d’agrégation et la catégorie d’attente (signalée en millisecondes).|
|**stdev_query_wait_time_ms**|**float**|`Query wait` écart type de durée pour le plan de requête dans l’intervalle d’agrégation et la catégorie d’attente (indiqué en millisecondes).|

## <a name="wait-categories-mapping-table"></a>Table de mappage des catégories d’attente

"%" est utilisé comme caractère générique
  
|Valeur entière|Catégorie d’attente|Les types d’attente incluent dans la catégorie|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|Unknown |  
|**1**|**UC**|SOS_SCHEDULER_YIELD|
|**2**|**Thread de travail**|THREADPOOL|
|**3**|**Verrou**|LCK_M_%|
|**4**|**Latch**|LATCH_%|
|**5**|**Verrou de mémoire tampon**|PAGELATCH_%|
|**6**|**E/s de mémoire tampon**|PAGEIOLATCH_%|
|**7**|**Compilation** _|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|_ *8**|**SQL CLR**|CLR%, SQLCLR%|
|**9**|**Mise en miroir**|DBMIRROR%|
|**10**|**Transaction**|XACT%, DTC%, TRAN_MARKLATCH_%, MSQL_XACT_%, TRANSACTION_MUTEX|
|**11**|**Périodes**|SLEEP_%, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE, CHECKPOINT_QUEUE, XE_TIMER_EVENT|
|**12**|**Emption**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(mais pas BROKER_RECEIVE_WAITFOR)**|
|**14**|**E/s du journal TRAN**|GESTIONNAIRE, LOGBUFFER, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, CHKPT, WRITELOG|
|**15**|**E/s réseau**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET, PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Parallélisme**|CXPACKET, EXCHANGE, HT%, BMP%, BP%|
|**17**|**Mémoire**|RESOURCE_SEMAPHORE, CMEMTHREAD, CMEMPARTITIONED, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT, MEMORY_GRANT_UPDATE|
|**19**|**Attente de l’utilisateur**|WAITFOR, WAIT_FOR_RESULTS, BROKER_RECEIVE_WAITFOR|
|**19**|**Suivi**|TRACEWRITE, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT, TRACE_EVTNOTIFF|
|**20**|**Recherche en texte intégral**|FT_RESTART_CRAWL, RASSEMBLEUR DE TEXTE INTÉGRAL, MSSEARCH, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR, PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**21**|**Autres e/s disque**|ASYNC_IO_COMPLETION, IO_COMPLETION, BACKUPIO, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Réplication**|SE_REPL_%, REPL_%, HADR_% **(mais pas HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_%, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ, PWAIT_HADRSIM|
|**23**|**Gouverneur du taux de journal**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR, HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

La catégorie d’attente de **compilation** n’est pas prise en charge actuellement.

## <a name="permissions"></a>Autorisations

 Nécessite l’autorisation `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Voir aussi

- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [Analyse des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  

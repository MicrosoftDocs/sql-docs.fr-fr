---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d76ff951e165c7ea2787588891d739b112fefc25
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095136"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations sur toutes les requêtes qui ont demandé et qui attendent une allocation de mémoire ou qui ont reçu une allocation de mémoire. Les requêtes qui ne nécessitent pas d’allocation de mémoire n’apparaissent pas dans cette vue. Par exemple, les opérations de jointure de tri et de hachage ont des allocations de mémoire pour l’exécution des requêtes, tandis que les requêtes sans clause **order by** n’ont pas d’allocation de mémoire.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée. En outre, les valeurs des colonnes **scheduler_id**, **wait_order** **pool_id**, **group_id** sont filtrées ; la valeur de la colonne est définie sur NULL.  
  
> [!NOTE]  
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_exec_query_memory_grants**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|ID (SPID) de la session dans laquelle cette requête est en cours d'exécution.|  
|**request_id**|**int**|ID de la demande. Unique dans le contexte de la session.|  
|**scheduler_id**|**int**|ID du planificateur qui planifie cette requête.|  
|**dop**|**smallint**|Degré de parallélisme de cette requête.|  
|**request_time**|**datetime**|Date et heure auxquelles cette requête a demandé l'allocation de mémoire.|  
|**grant_time**|**datetime**|Date et heure auxquelles la mémoire a été allouée pour cette requête. NULL si la mémoire n'a pas encore été allouée.|  
|**requested_memory_kb**|**bigint**|Quantité totale de mémoire demandée, en kilo-octets.|  
|**granted_memory_kb**|**bigint**|Quantité totale de mémoire actuellement allouée, en kilo-octets. Peut être NULL si la mémoire n'a pas encore été allouée. Dans une situation type, cette valeur doit être la même que **requested_memory_kb**. Pour la création d'index, le serveur peut autoriser de la mémoire à la demande supplémentaire au-delà de la mémoire allouée initialement.|  
|**required_memory_kb**|**bigint**|Mémoire minimale requise pour exécuter cette requête, en kilo-octets. **requested_memory_kb** est supérieure ou égale à cette valeur.|  
|**used_memory_kb**|**bigint**|Mémoire physique utilisée à ce moment, en kilo-octets.|  
|**max_used_memory_kb**|**bigint**|Mémoire physique maximale utilisée jusqu'à ce moment, en kilo-octets.|  
|**query_cost**|**float**|Coût estimé de la requête.|  
|**timeout_sec**|**int**|Délai d'expiration, en secondes, avant que cette requête abandonne la demande d'allocation de la mémoire.|  
|**resource_semaphore_id**|**smallint**|ID non unique du sémaphore de ressource sur lequel attend cette requête.<br /><br /> **Remarque :** Cet ID est unique dans les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Cette modification peut affecter l'exécution de la requête de résolution des problèmes. Pour plus d'informations, consultez la section « Notes », plus loin dans cette rubrique.|  
|**queue_id**|**smallint**|ID de la file d'attente dans laquelle cette requête attend l'allocation de mémoire. NULL si la mémoire est déjà allouée.|  
|**wait_order**|**int**|Ordre séquentiel des requêtes en attente dans le **queue_id** spécifié. Cette valeur peut changer pour une requête donnée si d’autres requêtes obtiennent des allocations de mémoire ou expirent. NULL si la mémoire est déjà allouée.|  
|**is_next_candidate**|**bit**|Candidat pour l'allocation mémoire suivante.<br /><br /> 1 = Oui<br /><br /> 0 = Non<br /><br /> NULL = La mémoire est déjà allouée.|  
|**wait_time_ms**|**bigint**|Temps d'attente en millisecondes. NULL si la mémoire est déjà allouée.|  
|**plan_handle**|**varbinary(64)**|Identificateur de ce plan de requête. Utilisez **sys.dm_exec_query_plan** pour extraire le plan XML réel.|  
|**sql_handle**|**varbinary(64)**|Identificateur de texte [!INCLUDE[tsql](../../includes/tsql-md.md)] pour cette requête. Utilisez **sys.dm_exec_sql_text** pour obtenir le texte [!INCLUDE[tsql](../../includes/tsql-md.md)] réel.|  
|**group_id**|**int**|ID du groupe de charge de travail dans lequel cette requête est exécutée.|  
|**pool_id**|**int**|ID du pool de ressources auquel appartient ce groupe de charge de travail.|  
|**is_small**|**tinyint**|Si la valeur est définie sur 1, cette allocation utilise le sémaphore de ressource le plus petit. Si la valeur est définie sur 0, c'est que le sémaphore de ressource ordinaire est utilisé.|  
|**ideal_memory_kb**|**bigint**|Taille de l'allocation mémoire, en kilo-octets (Ko) pour l'ajuster à la mémoire physique. Elle est basée sur l'estimation de la cardinalité.|  
|**pdw_node_id**|**int**|Identificateur du nœud sur lequel cette distribution se trouve.<br /><br /> **S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|Nombre de [threads de travail](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling)réservés.<br /><br />**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. |  
|**used_worker_count**|**bigint**|Nombre de [threads de travail](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) utilisés pour le moment.<br /><br />**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**max_used_worker_count**|**bigint**|Nombre maximal de [threads de travail](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) utilisés jusqu’à présent.<br /><br />**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**reserved_node_bitmap**|**bigint**|Bitmap des nœuds NUMA où les [threads de travail](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) sont réservés.<br /><br />**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], requiert l’autorisation `VIEW DATABASE STATE` dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Un scénario de débogage type pour le délai d'attente de la requête peut ressembler à ce qui suit :  
  
-   Recherchez l'état général de la mémoire système à l'aide de [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md), [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) et de différents compteurs de performance.  
  
-   Recherchez les réservations de mémoire d'exécution de la requête dans **sys.dm_os_memory_clerks** où `type = 'MEMORYCLERK_SQLQERESERVATIONS'`.  
  
-   Recherchez les requêtes en attente <sup>1</sup> pour les autorisations à l’aide de **sys.dm_exec_query_memory_grants**.  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup> Dans ce scénario, le type d’attente est généralement RESOURCE_SEMAPHORE. Pour plus d’informations, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). 
  
-   Cache de recherche pour les requêtes avec des allocations de mémoire à l’aide de [sys.dm_exec_cached_plans &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) et [sys.dm_exec_query_plan &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   Examinez attentivement les requêtes nécessitant beaucoup de mémoire à l'aide de [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   Si vous suspectez une perte de contrôle de requête, examinez le plan d'exécution de requêtes à partir de [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) et le texte du lot à partir de [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 Les requêtes qui utilisent des vues de gestion dynamique incluant `ORDER BY` ou agrégées peuvent augmenter la consommation de mémoire et contribuer ainsi au problème qu’elles résolvent.  
  
 La fonctionnalité Gouverneur de ressources permet à un administrateur de base de données de répartir des ressources serveur entre plusieurs pools de ressources (64 pools au maximum). À partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , chaque pool se comporte comme une petite instance de serveur indépendante et requiert 2 sémaphores. Le nombre de lignes retournées à partir de **sys.dm_exec_query_resource_semaphores** peut être jusqu’à 20 fois supérieur à celui des lignes retournées dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_exec_query_resource_semaphores &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [Guide d’architecture de thread et de tâche](../../relational-databases/thread-and-task-architecture-guide.md)   
  

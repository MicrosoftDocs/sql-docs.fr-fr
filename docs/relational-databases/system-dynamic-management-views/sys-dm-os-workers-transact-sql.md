---
description: sys.dm_os_workers (Transact-SQL)
title: sys.dm_os_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_workers_TSQL
- sys.dm_os_workers_TSQL
- dm_os_workers
- sys.dm_os_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_workers dynamic management view
ms.assetid: 4d5d1e52-a574-4bdd-87ae-b932527235e8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3d13344aef1c0a85b9893a9a36f0222253468b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144777"
---
# <a name="sysdm_os_workers-transact-sql"></a>sys.dm_os_workers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque processus de travail du système. Pour plus d’informations sur les Workers, consultez le [Guide d’architecture des threads et des tâches](../../relational-databases/thread-and-task-architecture-guide.md). 
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_workers**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|worker_address|**varbinary (8)**|Adresse mémoire du processus de travail.|  
|status|**int**|À usage interne uniquement|  
|is_preemptive|**bit**|1 = le processus de travail s'exécute avec une planification préemptive. Tout processus de travail qui exécute du code externe est exécuté en mode de planification préemptive.|  
|is_fiber|**bit**|1 = le processus de travail s'exécute en mode Regroupement léger. Pour plus d'informations, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).|  
|is_sick|**bit**|1 = le processus de travail est bloqué car il essaie toujours d'obtenir un verrouillage total de l'UC. Si ce bit est défini, cela peut signaler un problème de contention sur un objet auquel les accès sont fréquents.|  
|is_in_cc_exception|**bit**|1 = le processus de travail gère actuellement une exception non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_fatal_exception|**bit**|Indique si le processus de travail a reçu une exception fatale.|  
|is_inside_catch|**bit**|1 = le processus de travail gère actuellement une exception.|  
|is_in_polling_io_completion_routine|**bit**|1 = le processus de travail exécute actuellement une routine d'exécution d'E/S pour une E/S en attente. Pour plus d’informations, consultez [sys.dm_io_pending_io_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql.md).|  
|context_switch_count|**int**|Nombre de changements de contexte du planificateur qui sont exécutés par ce processus de travail.|  
|pending_io_count|**int**|Nombre d'E/S physiques qui sont effectuées par ce processus de travail.|  
|pending_io_byte_count|**bigint**|Nombre total d'octets correspondant à toutes les E/S physiques en attente pour ce processus de travail.|  
|pending_io_byte_average|**int**|Nombre moyen d'octets des E/S physiques pour ce processus de travail.|  
|wait_started_ms_ticks|**bigint**|Point dans le temps, dans [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quand ce travail est passé à l’état suspendu. La soustraction de cette valeur de ms_ticks dans [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) retourne le nombre de millisecondes d’attente du thread de travail.|  
|wait_resumed_ms_ticks|**bigint**|Point dans le temps, dans [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quand ce travail est passé à l’État exécutable. La soustraction de cette valeur de ms_ticks dans [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) retourne le nombre de millisecondes que le processus de travail a dans la file d’attente exécutable.|  
|task_bound_ms_ticks|**bigint**|Point dans le temps, dans [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), quand une tâche est liée à ce Worker.|  
|worker_created_ms_ticks|**bigint**|Point dans le temps, dans [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), lors de la création d’un Worker.|  
|exception_num|**int**|Numéro d'erreur de la dernière exception rencontrée par ce processus de travail.|  
|exception_severity|**int**|Gravité de la dernière exception rencontrée par ce processus de travail.|  
|exception_address|**varbinary (8)**|Adresse du code qui a levé l'exception.|  
|affinité|**bigint**|Affinité de thread du processus de travail. Correspond à l’affinité du thread dans [sys.dm_os_threads &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|state|**nvarchar(60)**|État du processus de travail. Peut avoir l’une des valeurs suivantes :<br /><br /> INIT = le processus de travail est en cours d'initialisation.<br /><br /> RUNNING = le processus de travail est en cours d'exécution, en mode non préemptif ou préemptif.<br /><br /> RUNNABLE = le processus de travail est prêt à s'exécuter sur le planificateur.<br /><br /> SUSPENDED = le processus de travail est actuellement interrompu car il attend qu'un événement lui envoie un signal.|  
|start_quantum|**bigint**|Temps, en millisecondes, au début de l'exécution actuelle de ce processus de travail.|  
|end_quantum|**bigint**|Temps, en millisecondes, à la fin de l'exécution actuelle de ce processus de travail.|  
|last_wait_type|**nvarchar(60)**|Type de la dernière attente. Pour obtenir la liste des types d’attente, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|return_code|**int**|Valeur retournée par la dernière attente. Peut avoir l’une des valeurs suivantes :<br /><br /> 0 =SUCCESS<br /><br /> 3 = DEADLOCK<br /><br /> 4 = PREMATURE_WAKEUP<br /><br /> 258 = TIMEOUT|  
|quantum_used|**bigint**|À usage interne uniquement|  
|max_quantum|**bigint**|À usage interne uniquement|  
|boost_count|**int**|À usage interne uniquement|  
|tasks_processed_count|**int**|Nombre de tâches traitées par ce processus de travail.|  
|fiber_address|**varbinary (8)**|Adresse mémoire de la fibre à laquelle ce processus de travail est associé.<br /><br /> NULL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas configuré pour le regroupement léger.|  
|task_address|**varbinary (8)**|Adresse mémoire de la tâche active. Pour plus d’informations, consultez [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|memory_object_address|**varbinary (8)**|Adresse mémoire de l'objet mémoire du processus de travail. Pour plus d’informations, consultez [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|thread_address|**varbinary (8)**|Adresse mémoire du thread associé au processus de travail. Pour plus d’informations, consultez [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|signal_worker_address|**varbinary (8)**|Adresse mémoire du processus de travail qui a signalé cet objet en dernier lieu. Pour plus d’informations, consultez [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|scheduler_address|**varbinary (8)**|Adresse mémoire du planificateur. Pour plus d’informations, consultez [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|processor_group|**smallint**|Stocke l'ID de groupe du processeur attribué à ce thread.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="remarks"></a>Notes  
 Si le processus de travail est à l'état RUNNING et qu'il s'exécute de façon non préemptive, son adresse correspond à la valeur de active_worker_address dans sys.dm_os_schedulers.  
  
 Lorsqu'un processus de travail en attente sur un événement est signalé, il est placé en tête de la file d'attente exécutable. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise cette situation mille fois de suite, après quoi le processus de travail est placé à la fin de la file d'attente. Le fait de placer un processus de travail à la fin de la file d'attente a un impact sur les performances.  
  
## <a name="permissions"></a>Autorisations
Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] les niveaux Premium, requiert l' `VIEW DATABASE STATE` autorisation dans la base de données. Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] les niveaux standard et de base, nécessite l'  `Server Admin` appartenance au rôle ou un `Azure Active Directory admin` compte.   

## <a name="examples"></a>Exemples  
 Vous pouvez utiliser la requête suivante pour déterminer la durée d'exécution d'un processus de travail à l'état SUSPENDED ou RUNNABLE.  
  
```sql
SELECT   
    t1.session_id,  
    CONVERT(varchar(10), t1.status) AS status,  
    CONVERT(varchar(15), t1.command) AS command,  
    CONVERT(varchar(10), t2.state) AS worker_state,  
    w_suspended =   
      CASE t2.wait_started_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_started_ms_ticks  
      END,  
    w_runnable =   
      CASE t2.wait_resumed_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_resumed_ms_ticks  
      END  
  FROM sys.dm_exec_requests AS t1  
  INNER JOIN sys.dm_os_workers AS t2  
    ON t2.task_address = t1.task_address  
  CROSS JOIN sys.dm_os_sys_info AS t3  
  WHERE t1.scheduler_id IS NOT NULL;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 session_id status     command         worker_state w_suspended w_runnable  
 ---------- ---------- --------------- ------------ ----------- --------------------  
 4          background LAZY WRITER     SUSPENDED    688         688  
 6          background LOCK MONITOR    SUSPENDED    4657        4657
 19         background BRKR TASK       SUSPENDED    603820344   603820344  
 14         background BRKR EVENT HNDL SUSPENDED    63583641    63583641  
 51         running    SELECT          RUNNING      0           0  
 2          background RESOURCE MONITO RUNNING      0           603825954  
 3          background LAZY WRITER     SUSPENDED    422         422  
 7          background SIGNAL HANDLER  SUSPENDED    603820485   603820485  
 13         background TASK MANAGER    SUSPENDED    603824704   603824704  
 18         background BRKR TASK       SUSPENDED    603820407   603820407  
 9          background TRACE QUEUE TAS SUSPENDED    454         454  
 52         suspended  SELECT          SUSPENDED    35094       35094  
 1          background RESOURCE MONITO RUNNING      0           603825954  
```

 Dans le résultat, lorsque `w_runnable` et `w_suspended` sont identiques, la valeur représente la durée pendant laquelle le processus de travail est dans l'état SUSPENDED. Dans le cas contraire, `w_runnable` représente la durée pendant laquelle le processus de travail est dans l'état RUNNABLE. Dans le résultat, la session `52` est dans l'état `SUSPENDED` pendant `35,094` millisecondes.  
  
## <a name="see-also"></a>Voir aussi  
[SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Guide d’architecture de thread et de tâche](../../relational-databases/thread-and-task-architecture-guide.md)    

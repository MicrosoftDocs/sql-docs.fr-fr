---
description: sys.dm_os_waiting_tasks (Transact-SQL)
title: sys.dm_os_waiting_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_waiting_tasks
- sys.dm_os_waiting_tasks_TSQL
- dm_os_waiting_tasks_TSQL
- sys.dm_os_waiting_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_waiting_tasks dynamic management view
ms.assetid: ca5e6844-368c-42e2-b187-6e5f5afc8df3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0e751d000219fd585288cf5765b520a89fcbc2a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837846"
---
# <a name="sysdm_os_waiting_tasks-transact-sql"></a>sys.dm_os_waiting_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie les informations sur la file d'attente des tâches en attente de certaines ressources. Pour plus d’informations sur les tâches, consultez le [Guide d’architecture des threads et des tâches](../../relational-databases/thread-and-task-architecture-guide.md).
   
> [!NOTE]  
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_waiting_tasks**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**waiting_task_address**|**varbinary (8)**|Adresse de la tâche en attente.|  
|**session_id**|**smallint**|ID de la session associée à la tâche.|  
|**exec_context_id**|**int**|ID du contexte d'exécution associé à la tâche.|  
|**wait_duration_ms**|**bigint**|Temps d'attente total de ce type d'attente (en millisecondes). Cette heure est comprise entre **signal_wait_time**.|  
|**wait_type**|**nvarchar(60)**|Nom du type d'attente.|  
|**resource_address**|**varbinary (8)**|Adresse de la ressource que la tâche attend.|  
|**blocking_task_address**|**varbinary (8)**|Tâche qui mobilise actuellement cette ressource.|  
|**blocking_session_id**|**smallint**|ID de la session qui bloque la demande. Si cette colonne est NULL, la demande n'est pas bloquée, ou les informations de session de la session bloquant la demande ne sont pas disponibles (ou ne peuvent pas être identifiées).<br /><br /> -2 = La ressource qui bloque la demande appartient à une transaction distribuée orpheline.<br /><br /> -3 = La ressource qui bloque la demande appartient à une transaction de récupération différée.<br /><br /> -4 = L'ID de session du propriétaire du verrou qui bloque la demande n'a pas pu être déterminé en raison de transitions d'état de verrou interne.|  
|**blocking_exec_context_id**|**int**|ID du contexte d'exécution de la tâche bloquante.|  
|**resource_description**|**nvarchar (3072)**|Description de la ressource actuellement mobilisée. Pour plus d'informations, consultez la liste ci-dessous.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="resource_description-column"></a>Colonne resource_description  
 La colonne resource_description comporte les valeurs possibles suivantes.  
  
 **Propriétaire des ressources de pool de threads :**  
  
-   ID ThreadPool = planificateur\<hex-address>  
  
 **Propriétaire de ressources de requêtes parallèles :**  
  
-   exchangeEvent ID = {port | Pipe} \<hex-address> WaitType = \<exchange-wait-type> NodeId =\<exchange-node-id>  
  
 **Exchange-wait-type :**  
  
-   e_waitNone  
  
-   e_waitPipeNewRow  
  
-   e_waitPipeGetRow  
  
-   e_waitSynchronizeConsumerOpen  
  
-   e_waitPortOpen  
  
-   e_waitPortClose  
  
-   e_waitRange  
  
 **Propriétaire de ressources de verrouillage :**  
  
-   \<type-specific-description> ID = verrou \<lock-hex-address> mode = \<mode> associatedObjectId =\<associated-obj-id>  
  
     **\<type-specific-description> peut être :**  
  
    -   Pour la base de données : databaselock Resource = \<databaselock-subresource> dbid =\<db-id>  
  
    -   Pour le fichier : filelock fileid = \<file-id> Resource = \<filelock-subresource> dbid =\<db-id>  
  
    -   Pour Object : objectlock lockPartition = \<lock-partition-id> objID = \<obj-id> Resource = \<objectlock-subresource> dbid =\<db-id>  
  
    -   Pour la page : PageLock fileid = \<file-id> pageid = \<page-id> dbid = Resource \<db-id> =\<pagelock-subresource>  
  
    -   Pour Key : Keylock hobtid = \<hobt-id> dbid =\<db-id>  
  
    -   Pour extent : extentlock fileid = \<file-id> pageid = \<page-id> dbid =\<db-id>  
  
    -   Pour RID : ridlock fileid = \<file-id> pageid = \<page-id> dbid =\<db-id>  
  
    -   Pour APPLICATION : applicationlock hash = \<hash> databasePrincipalId = \<role-id> dbid =\<db-id>  
  
    -   Pour les métadonnées : metadatalock Resource = \<metadata-subresource> ClassID = \<metadatalock-description> dbid =\<db-id>  
  
    -   Pour HoBT : hobtlock hobtid = \<hobt-id> Resource = \<hobt-subresource> dbid =\<db-id>  
  
    -   Pour ALLOCATION_UNIT : allocunitlock hobtid = \<hobt-id> Resource = \<alloc-unit-subresource> dbid =\<db-id>  
  
     **\<mode> peut être :**  
  
     Sch-S, Sch-M, S, U, X, IS, IU, IX, SIU, SIX, UIX, BU, RangeS-S, RangeS-U, RangeI-N, RangeI-S, RangeI-U, RangeI-X, RangeX-, RangeX-U, RangeX-X  
  
 **Propriétaire de ressources externes :**  
  
-   Externalressource externe =\<wait-type>  
  
 **Propriétaire de ressources génériques :**  
  
-   Espace de travail TransactionInfo TransactionMutex =\<workspace-id>  
  
-   Mutex  
  
-   CLRTaskJoin  
  
-   CLRMonitorEvent  
  
-   CLRRWLockEvent  
  
-   resourceWait  
  
 **Propriétaire de ressources de verrou :**  
  
-   \<db-id>:\<file-id>:\<page-in-file>  
  
-   \<GUID>  
  
-   \<latch-class> (\<latch-address>)  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
 
## <a name="example"></a>Exemple
### <a name="a-identify-tasks-from-blocked-sessions"></a>R. Identifiez les tâches des sessions bloquées. 

```sql
SELECT * FROM sys.dm_os_waiting_tasks 
WHERE blocking_session_id IS NOT NULL; 
```   

### <a name="b-view-waiting-tasks-per-connection"></a>B. Afficher les tâches en attente par connexion

```sql
SELECT st.text AS [SQL Text], c.connection_id, w.session_id, 
  w.wait_duration_ms, w.wait_type, w.resource_address, 
  w.blocking_session_id, w.resource_description, c.client_net_address, c.connect_time
FROM sys.dm_os_waiting_tasks AS w
INNER JOIN sys.dm_exec_connections AS c ON w.session_id = c.session_id 
CROSS APPLY (SELECT * FROM sys.dm_exec_sql_text(c.most_recent_sql_handle)) AS st 
              WHERE w.session_id > 50 AND w.wait_duration_ms > 0
ORDER BY c.connection_id, w.session_id
GO
```

### <a name="c-view-waiting-tasks-for-all-user-processes-with-additional-information"></a>C. Afficher les tâches en attente pour tous les processus utilisateur avec des informations supplémentaires

```sql
SELECT 'Waiting_tasks' AS [Information], owt.session_id,
    owt.wait_duration_ms, owt.wait_type, owt.blocking_session_id,
    owt.resource_description, es.program_name, est.text,
    est.dbid, eqp.query_plan, er.database_id, es.cpu_time,
    es.memory_usage*8 AS memory_usage_KB
FROM sys.dm_os_waiting_tasks owt
INNER JOIN sys.dm_exec_sessions es ON owt.session_id = es.session_id
INNER JOIN sys.dm_exec_requests er ON es.session_id = er.session_id
OUTER APPLY sys.dm_exec_sql_text (er.sql_handle) est
OUTER APPLY sys.dm_exec_query_plan (er.plan_handle) eqp
WHERE es.is_user_process = 1
ORDER BY owt.session_id;
GO
```
  
## <a name="see-also"></a>Voir aussi  
[SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)      
[Guide d’architecture de thread et de tâche](../../relational-databases/thread-and-task-architecture-guide.md)     
   

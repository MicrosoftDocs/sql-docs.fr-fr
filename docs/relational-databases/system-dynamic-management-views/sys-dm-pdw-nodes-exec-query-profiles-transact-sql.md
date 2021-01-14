---
title: sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL) | Microsoft Docs
description: Vue de gestion dynamique qui peut être utilisée pour surveiller la progression des requêtes de l’entrepôt de données en temps réel pendant l’exécution de la requête.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: f1917899d58262f670b58843c118864618702950
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169961"
---
# <a name="sysdm_pdw_nodes_exec_query_profiles-transact-sql"></a>sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Surveille la progression des requêtes de l’entrepôt de données en temps réel pendant l’exécution de la requête.   
  
## <a name="table-returned"></a>Table retournée
  
Les compteurs retournés sont par opérateur par thread. Les résultats sont dynamiques et ne correspondent pas aux résultats des options existantes, telles que la création d’une `SET STATISTICS XML ON` sortie uniquement lorsque la requête est terminée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|ID numérique unique associé au nœud.|
|session_id|**smallint**|Identifie la session dans laquelle cette requête s'exécute. Référence dm_exec_sessions.session_id.|  
|request_id|**int**|Identifie la demande cible. Référence dm_exec_sessions.request_id.|  
|sql_handle|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée dont fait partie la requête. Référence dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary(64)**|Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan, ou qui est en cours d’exécution. Fait référence à dm_exec_query_stats. plan_handle.|  
|physical_operator_name|**nvarchar (256)**|Nom de l'opérateur physique.|  
|node_id|**int**|Identifie un nœud d'opérateur dans l'arborescence de requête.|  
|thread_id|**int**|Fait la distinction entre les threads (pour une requête parallèle) qui appartiennent au même nœud d'opérateur de requête.|  
|task_address|**varbinary (8)**|Identifie la tâche SQLOS utilisée par ce thread. Référence dm_os_tasks.task_address.|  
|row_count|**bigint**|Nombre de lignes retournées par l'opérateur jusqu'à présent.|  
|rewind_count|**bigint**|Nombre de rembobinages jusqu'à présent.|  
|rebind_count|**bigint**|Nombre de reliaisons jusqu'à présent.|  
|end_of_scan_count|**bigint**|Nombre de fins d'analyses jusqu'à présent.|  
|estimate_row_count|**bigint**|Nombre de lignes estimé. Il peut être utile pour comparer estimated_row_count à actual row_count réel.|  
|first_active_time|**bigint**|Heure du premier appel de l'opérateur en millisecondes.|  
|last_active_time|**bigint**|Heure du dernier appel de l'opérateur en millisecondes.|  
|open_time|**bigint**|Horodatage lors de l'ouverture (en millisecondes).|  
|first_row_time|**bigint**|Horodatage lors de l'ouverture de la première ligne (en millisecondes).|  
|last_row_time|**bigint**|Horodatage lors de l'ouverture de la dernière ligne (en millisecondes).|  
|close_time|**bigint**|Horodatage lors de la fermeture (en millisecondes).|  
|elapsed_time_ms|**bigint**|Temps total écoulé (en millisecondes) utilisé par les opérations du nœud cible jusqu’à présent.|  
|cpu_time_ms|**bigint**|Temps processeur total (en millisecondes) utilisé par les opérations du nœud cible jusqu’à présent.|  
|database_id|**smallint**|ID de la base de données qui contient l'objet sur lequel les opérations de lecture et d'écriture sont effectuées.|  
|object_id|**int**|Identificateur de l'objet sur lequel les opérations de lecture et écriture sont effectuées. Fait référence à sys.objects.object_id.|  
|index_id|**int**|Index (le cas échéant) dans lequel l'ensemble de lignes est ouvert.|  
|scan_count|**bigint**|Nombre d'analyses de tables ou d'index jusqu'à présent.|  
|logical_read_count|**bigint**|Nombre de lectures logiques jusqu'à présent.|  
|physical_read_count|**bigint**|Nombre de lectures physiques jusqu'à présent.|  
|read_ahead_count|**bigint**|Nombre de lectures anticipées jusqu'à présent.|  
|write_page_count|**bigint**|Nombre d'écritures de page jusqu'à présent en raison de débordement.|  
|lob_logical_read_count|**bigint**|Nombre de lectures logiques LOB jusqu'à présent.|  
|lob_physical_read_count|**bigint**|Nombre de lectures physiques LOB jusqu'à présent.|  
|lob_read_ahead_count|**bigint**|Nombre de lectures anticipées LOB jusqu'à présent.|  
|segment_read_count|**int**|Nombre de lectures anticipées de segment jusqu'à présent.|  
|segment_skip_count|**int**|Nombre de segments ignorés jusqu'à présent.| 
|actual_read_row_count|**bigint**|Nombre de lignes lues par un opérateur avant l’application du prédicat résiduel.| 
|estimated_read_row_count|**bigint**|**S’applique à :** À partir de [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1. <br/>Nombre de lignes dont la lecture est estimée par un opérateur avant l’application du prédicat résiduel.|  
  
## <a name="remarks"></a>Notes

Les mêmes remarques dans [sys.dm_exec_query_profiles](./sys-dm-exec-query-profiles-transact-sql.md) s’appliquent.  

## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  

## <a name="see-also"></a>Voir aussi

 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
   

 ## <a name="next-steps"></a>Étapes suivantes 

Vue d’ensemble du développement Azure Synapse Analytics] (/Azure/SQL-Data-Warehouse/SQL-Data-Warehouse-Overview-develop).
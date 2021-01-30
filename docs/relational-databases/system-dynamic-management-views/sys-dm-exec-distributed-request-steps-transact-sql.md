---
description: sys.dm_exec_distributed_request_steps (Transact-SQL)
title: sys.dm_exec_distributed_request_steps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23992d8ed0c2974c78f3fcd5db3686683b9d6ee3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160059"
---
# <a name="sysdm_exec_distributed_request_steps-transact-sql"></a>sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Contient des informations sur toutes les étapes qui composent une requête ou requête Polybase donnée. Elle répertorie une ligne par étape de requête.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id et step_index constituent la clé de cette vue. ID numérique unique associé à la demande.|Consultez l’ID dans [sys.dm_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|Position de cette étape dans la séquence d’étapes qui composent la demande.|0 à (n-1) pour une demande avec n étapes.|  
|operation_type|**nvarchar(128)**|Type de l’opération représentée par cette étape.|'MoveOperation', 'OnOperation', 'RandomIDOperation', 'RemoteOperation', 'ReturnOperation', 'ShuffleMoveOperation', 'TempTablePropertiesOperation', 'DropDiagnosticsNotifyOperation', 'HadoopShuffleOperation', 'HadoopBroadCastOperation', 'HadoopRoundRobinOperation'|  
|distribution_type|**nvarchar(32)**|Emplacement d’exécution de l’étape.|'AllComputeNodes', 'AllDistributions', 'ComputeNode', 'distribution', 'AllNodes', 'SubsetNodes', 'SubsetDistributions', 'Unspecified'.|  
|location_type|**nvarchar(32)**|Emplacement d’exécution de l’étape.|« COMPUTE », « Head » ou « DMS ». Toutes les étapes de déplacement de données affichent « DMS ».|  
|status|**nvarchar(32)**|État de cette étape|« Pending », « Running », « Complete », « failed », « UndoFailed », « PendingCancel », « Cancelled », « UNDONE », « Aborted »|  
|error_id|**nvarchar (36)**|ID unique de l’erreur associée à cette étape, le cas échéant|Consultez l’ID de [sys.dm_exec_compute_node_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure à laquelle l’étape a démarré l’exécution|Inférieure ou égale à l’heure actuelle et supérieure ou égale à end_compile_time de la requête à laquelle cette étape appartient.|  
|end_time|**datetime**|Heure à laquelle cette étape s’est terminée, a été annulée ou a échoué.|La valeur est inférieure ou égale à l’heure actuelle et supérieure ou égale à start_time, définie sur la valeur NULL pour les étapes en cours d’exécution ou en attente.|  
|total_elapsed_time|**int**|Durée totale d’exécution de l’étape de la requête, en millisecondes|Entre 0 et la différence entre end_time et start_time. 0 pour les étapes en file d’attente.|  
|row_count|**bigint**|Nombre total de lignes modifiées ou retournées par cette demande|0 pour les étapes qui ne sont pas modifiées ou qui renvoient des données, nombre de lignes affectées dans le cas contraire. Défini sur-1 pour les étapes DMS.|  
|.|nvarchar(4000)|Contient le texte complet de la commande de cette étape.|Toute chaîne de demande valide pour une étape. Tronqué si plus de 4000 caractères.|  
  
## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

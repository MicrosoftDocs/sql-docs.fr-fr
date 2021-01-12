---
description: sys.dm_exec_distributed_sql_requests (Transact-SQL)
title: sys.dm_exec_distributed_sql_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_distributed_requests management view
- dm_exec_distributed_requests management view
ms.assetid: d065dc01-35d4-472f-9554-53ac41e7d104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b03ee4a4c974be2b035074f03b8397df580f75d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099002"
---
# <a name="sysdm_exec_distributed_sql_requests-transact-sql"></a>sys.dm_exec_distributed_sql_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Contient des informations sur toutes les distributions de requêtes SQL dans le cadre d’une étape SQL de la requête.  Cette vue affiche les données des 1000 dernières requêtes ; les demandes actives comportent toujours les données présentes dans cette vue.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|execution_id et step_index constituent la clé de cette vue. ID numérique unique associé à la demande.|Consultez l’ID dans [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|  
|step_index|**int**|Index de l’étape de requête dont cette distribution fait partie.|Consultez step_index dans [sys.dm_exec_distributed_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|compute_node_id|**int**|Type de l’opération représentée par cette étape.|Consultez compute_node_id dans [sys.dm_exec_compute_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|**int**|Emplacement d’exécution de l’étape.|Affectez la valeur-1 pour les requêtes qui s’exécutent dans l’étendue du nœud et non dans l’étendue de la distribution.|  
|status|**nvarchar(32)**|État de cette étape|Actif, annulé, terminé, échec, en file d’attente|  
|error_id|**nvarchar (36)**|ID unique de l’erreur associée à cette étape, le cas échéant|Consultez l’ID de [sys.dm_exec_compute_node_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure à laquelle l’étape a démarré l’exécution|Inférieure ou égale à l’heure actuelle et supérieure ou égale à end_compile_time de la requête à laquelle cette étape appartient.|  
|end_time|**datetime**|Heure à laquelle cette étape s’est terminée, a été annulée ou a échoué.|La valeur est inférieure ou égale à l’heure actuelle et supérieure ou égale à start_time, définie sur la valeur NULL pour les étapes en cours d’exécution ou en attente.|  
|total_elapsed_time|**int**|Durée totale d’exécution de l’étape de la requête, en millisecondes|Entre 0 et la différence entre end_time et start_time. 0 pour les étapes en file d’attente.|  
|row_count|**bigint**|Nombre total de lignes modifiées ou retournées par cette demande|0 pour les étapes qui ne sont pas modifiées ou qui renvoient des données, nombre de lignes affectées dans le cas contraire. Défini sur-1 pour les étapes DMS.|  
|spid|**int**|ID de session sur l’instance de SQL Server exécutant la distribution des requêtes||  
|.|nvarchar(4000)|Contient le texte complet de la commande de cette étape.|Toute chaîne de demande valide pour une étape. Tronqué si plus de 4000 caractères.|  
  
## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

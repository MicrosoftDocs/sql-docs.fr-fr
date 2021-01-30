---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10f7f8ae36a35a39a303d9548b828768282df3f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159985"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Retourne des informations sur la charge de travail par travail, sur chaque nœud de calcul.  
  
 Sys.dm_exec_external_work d’interrogation pour identifier le travail qui doit communiquer avec la source de données externe (par exemple, Hadoop ou SQL Server externe).  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Identificateur unique de la requête Polybase associée.|Consultez *request_ID* dans [sys.dm_exec_requests &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|`int`|Requête exécutée par ce processus de travail.|Consultez *step_index* dans  [sys.dm_exec_requests &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|`int`|Étape du plan DMS que ce processus de travail exécute.|Consultez [sys.dm_exec_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|`int`|Nœud sur lequel le processus de travail s’exécute.|Consultez [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|type|`nvarchar(60)`|Type de travail externe.|'Fichier fractionné'|  
|work_id|`int`|ID du fractionnement réel.|Supérieur ou égal à 0.|  
|input_name|`nvarchar(4000)`|Nom de l’entrée à lire|Nom de fichier lors de l’utilisation de Hadoop.|  
|read_location|`bigint`|Emplacement de décalage ou de lecture.|Décalage du fichier à lire.|  
|bytes_processed|`bigint`|Nombre total d’octets alloués pour le traitement des données par ce Worker. Cela ne représente peut-être pas nécessairement le nombre total de données retournées par la requête. |Supérieur ou égal à 0.|  
|length|`bigint`|Longueur du bloc Split ou HDFS dans le cas de Hadoop|Définissable par l’utilisateur. La valeur par défaut est 64M|  
|status|`nvarchar(32)`|État du Worker|En attente, traitement, terminé, échec, abandonné|  
|start_time|`datetime`|Début du travail||  
|end_time|`datetime`|Fin du travail||  
|total_elapsed_time|`int`|Durée totale en millisecondes||
|compute_pool_id|`int`|Identificateur unique du pool.|

## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

---
description: sys.dm_pdw_dms_workers (Transact-SQL)
title: sys.dm_pdw_dms_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0a284d18-3c46-4ffa-bcc9-689e660ee8b4
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 735ab65bc39b4f4ffab93f1b2bf8a705c3d895c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99143269"
---
# <a name="sysdm_pdw_dms_workers-transact-sql"></a>sys.dm_pdw_dms_workers (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur tous les Workers effectuant les étapes DMS.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Requête dont fait partie ce Worker DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Étape de requête dont ce Worker DMS fait partie.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.|Consultez step_index dans [sys.dm_pdw_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Étape du plan DMS que ce processus de travail exécute.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.||  
|pdw_node_id|**int**|Nœud sur lequel le processus de travail s’exécute.|Consultez node_id dans [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**Int**|Distribution sur laquelle le processus de travail s’exécute, le cas échéant.|Consultez distribution_id dans [sys.pdw_distributions &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md).|  
|type|**nvarchar(32)**|Type de thread de travail DMS représenté par cette entrée.|'DIRECT_CONVERTER', 'DIRECT_READER', 'FILE_READER', 'HASH_CONVERTER', 'HASH_READER', 'ROUNDROBIN_CONVERTER', 'EXPORT_READER', 'EXTERNAL_READER', 'EXTERNAL_WRITER', 'PARALLEL_COPY_READER', 'REJECT_WRITER', 'WRITER'|  
|status|**nvarchar(32)**|État du Worker DMS.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|bytes_per_sec|**bigint**|Débit de lecture ou d’écriture au cours de la dernière seconde.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant l’exécution du processus de travail.|  
|bytes_processed|**bigint**|Nombre total d’octets traités par ce Worker.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant l’exécution du processus de travail.|  
|rows_processed|**bigint**|Nombre de lignes lues ou écrites pour ce Worker.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant l’exécution du processus de travail.|  
|start_time|**datetime**|Heure de début de l’exécution de ce Worker.|Supérieur ou égal à l’heure de début de l’étape de requête à laquelle ce Worker appartient. Consultez [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle l’exécution s’est terminée, a échoué ou a été annulée.|NULL pour les Workers en cours ou mis en file d’attente. Sinon, supérieur à start_time.|  
|total_elapsed_time|**int**|Durée totale d’exécution, en millisecondes.|Supérieur ou égal à 0.<br /><br /> Temps total écoulé depuis le démarrage ou le redémarrage du système. Si total_elapsed_time dépasse la valeur maximale d’un entier (24,8 jours en millisecondes), cela entraînera un échec de matérialisation en raison d’un dépassement de capacité.<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
|cpu_time|**bigint**|Temps processeur consommé par ce Worker, en millisecondes.|Supérieur ou égal à 0.|  
|query_time|**int**|Période de temps avant que SQL commence à retourner des lignes au thread, en millisecondes.|Supérieur ou égal à 0.|  
|buffers_available|**int**|Nombre de mémoires tampons inutilisées.| NULL si la requête a été annulée ou a échoué avant l’exécution du processus de travail.|  
|sql_spid|**int**|ID de session sur l’instance de SQL Server effectuant le travail pour ce Worker DMS.||  
|dms_cpid|**int**|ID de processus du thread réel en cours d’exécution.||  
|error_id|**nvarchar (36)**|Identificateur unique de l’erreur qui s’est produite pendant l’exécution de ce Worker, le cas échéant.|Consultez error_id dans [sys.dm_pdw_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|source_info|**nvarchar(4000)**|Pour un lecteur, spécification des tables et des colonnes sources.||  
|destination_info|**nvarchar(4000)**|Pour un enregistreur, spécification des tables de destination.||  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section métadonnées dans la rubrique [limites de capacité](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

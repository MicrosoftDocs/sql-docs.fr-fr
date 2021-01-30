---
description: sys.dm_pdw_dms_external_work (Transact-SQL)
title: sys.dm_pdw_dms_external_work (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 47345015-f861-451e-97c4-6e1cb81d1922
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: cfdccad0daadd45b16d2e910eed8c7b194182daf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99143628"
---
# <a name="sysdm_pdw_dms_external_work-transact-sql"></a>sys.dm_pdw_dms_external_work (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] vue système qui contient des informations sur toutes les étapes du service de déplacement des données (DMS) pour les opérations externes.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Requête qui utilise ce Worker DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.|Identique à request_id dans [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Étape de requête qui appelle ce Worker DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.|Identique à step_index dans [sys.dm_pdw_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Étape actuelle dans le plan DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé de cette vue.|Identique à dms___step_index dans [sys.dm_pdw_dms_workers &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md).|  
|pdw_node_id|**int**|Nœud qui exécute le processus de travail DMS.|Identique à node_id dans [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|type|**nvarchar(60)**|Type d’opération externe en cours d’exécution par ce nœud.<br /><br /> Le FRACTIONNEment de fichier est une opération sur un fichier Hadoop externe qui a été fractionné en plusieurs petites opérations.|'FICHIER FRACTIONNÉ'|  
|work_id|**int**|ID de fractionnement de fichier.|Supérieur ou égal à 0.<br /><br /> Unique par nœud de calcul.|  
|input_name|**nvarchar(60)**|Nom de chaîne de l’entrée en cours de lecture.|Pour un fichier Hadoop, il s’agit du nom de fichier Hadoop.|  
|read_location|**bigint**|Décalage de l’emplacement de lecture.||  
|estimated_bytes_processed|**bigint**|Nombre d’octets traités par ce Worker.|Supérieur ou égal à 0.|  
|length|**bigint**|Nombre d’octets dans le fractionnement du fichier.<br /><br /> Pour Hadoop, il s’agit de la taille du bloc HDFS.|Défini par l’utilisateur. La valeur par défaut est 64 Mo.|  
|status|**nvarchar(32)**|État du processus de travail.|En attente, traitement, terminé, échec, abandonné|  
|start_time|**datetime**|Heure de début de l’exécution de ce Worker.|Supérieur ou égal à l’heure de début de l’étape de requête à laquelle ce Worker appartient. Consultez [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle l’exécution s’est terminée, a échoué ou a été annulée.|NULL pour les Workers en cours ou mis en file d’attente. Sinon, supérieur à start_time.|  
|total_elapsed_time|**int**|Durée totale d’exécution, en millisecondes.|Supérieur ou égal à 0.<br /><br /> Si total_elapsed_time dépasse la valeur maximale d’un entier, total_elapsed_time sera toujours la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée ».<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section métadonnées dans la rubrique [limites de capacité](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  

---
description: sys.dm_pdw_sql_requests (Transact-SQL)
title: sys.dm_pdw_sql_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 44e19609-902c-46cf-acdf-19ea75011365
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: fbb924edb6bd5d6b246ee986b7f600576a179a61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138819"
---
# <a name="sysdm_pdw_sql_requests-transact-sql"></a>sys.dm_pdw_sql_requests (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur toutes les distributions de requêtes SQL Server dans le cadre d’une étape SQL de la requête.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Identificateur unique de la requête à laquelle appartient cette distribution de requêtes SQL.<br /><br /> request_id, step_index et distribution_id forment la clé de cette vue.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Index de l’étape de requête dont cette distribution fait partie.<br /><br /> request_id, step_index et distribution_id forment la clé de cette vue.|Consultez step_index dans [sys.dm_pdw_request_steps &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|pdw_node_id|**int**|Identificateur unique du nœud sur lequel cette distribution de requêtes est exécutée.|Consultez node_id dans [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**int**|Identificateur unique de la distribution sur laquelle cette distribution de requêtes est exécutée.<br /><br /> request_id, step_index et distribution_id forment la clé de cette vue.|Consultez distribution_id dans [sys.pdw_distributions &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md). Affectez la valeur-1 pour les requêtes qui s’exécutent au niveau de l’étendue du nœud, et non dans l’étendue de la distribution.|  
|status|**nvarchar(32)**|État actuel de la distribution de la requête.|En attente, en cours d’exécution, en échec, annulé, terminé, abandonné, CancelSubmitted|  
|error_id|**nvarchar (36)**|Identificateur unique de l’erreur associée à cette distribution de requête, le cas échéant.|Consultez error_id dans [sys.dm_pdw_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). Affectez la valeur NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure à laquelle la distribution des requêtes a commencé l’exécution.|Inférieure ou égale à l’heure actuelle et supérieure ou égale à start_time de l’étape de requête à laquelle cette distribution de requête appartient|  
|end_time|**datetime**|Heure à laquelle cette distribution de requête s’est terminée, a été annulée ou a échoué.|Supérieur ou égal à l’heure de début, ou valeur NULL si la distribution de la requête est en cours ou en file d’attente.|  
|total_elapsed_time|**int**|Représente l’heure d’exécution de la distribution de requêtes, en millisecondes.|Supérieur ou égal à 0. Égal au Delta de start_time et end_time pour les distributions de requêtes terminées, ayant échoué ou annulées.<br /><br /> Si total_elapsed_time dépasse la valeur maximale d’un entier, total_elapsed_time sera toujours la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée ».<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
|row_count|**bigint**|Nombre de lignes modifiées ou lues par cette distribution de requête.|-1 pour les opérations qui ne modifient pas ou ne retournent pas de données, telles que CREATE TABLE et DROP TABLE.|  
|spid|**int**|ID de session sur l’instance de SQL Server exécutant la distribution de la requête.||  
|.|**nvarchar(4000)**|Texte complet de la commande pour cette distribution de requête.|Toute requête ou chaîne de requête valide.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section métadonnées dans la rubrique [limites de capacité](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

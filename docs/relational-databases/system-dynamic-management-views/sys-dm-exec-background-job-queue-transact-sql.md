---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 07490d9e2a05e65fa90dd76cbdf51647f000bb3a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092911"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque travail du processeur de requêtes qui est planifié pour s'exécuter de façon asynchrone (en arrière-plan).  
  
> **REMARQUE** Pour appeler cette valeur à partir de **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** ou **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** , utilisez le nom **sys.dm_pdw_nodes_exec_background_job_queue**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|Moment auquel le travail a été ajouté à la file d'attente.|  
|**job_id**|**int**|Identificateur du travail.|  
|**database_id**|**int**|Base de données sur laquelle le travail doit s'exécuter.|  
|**object_id1**|**int**|Cette valeur dépend du type de travail. Pour plus d'informations, consultez la section Remarques.|  
|**object_id2**|**int**|Cette valeur dépend du type de travail. Pour plus d'informations, consultez la section Remarques.|  
|**object_id3**|**int**|Cette valeur dépend du type de travail. Pour plus d'informations, consultez la section Remarques.|  
|**object_id4**|**int**|Cette valeur dépend du type de travail. Pour plus d'informations, consultez la section Remarques.|  
|**error_code**|**int**|Code d'erreur si le travail a été réinséré à cause d'une erreur. NULL si le travail a été suspendu, n'a pas été récupéré ou est terminé.|  
|**request_type**|**smallint**|Type de demande du travail.|  
|**retry_count**|**smallint**|Nombre de fois où le travail a été récupéré et réinséré dans la file d'attente pour cause de manque de ressources ou pour d'autres raisons.|  
|**in_progress**|**smallint**|Indique si l'exécution du travail a commencé.<br /><br /> 1 = Démarré<br /><br /> 0 = en attente|  
|**session_id**|**smallint**|Identificateur de la session.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="remarks"></a>Notes  
 Seules les informations pour les travaux de mise à jour des statistiques asynchrone apparaissent dans cette vue. Pour plus d’informations sur les statistiques des mises à jour asynchrones, consultez [Statistics](../../relational-databases/statistics/statistics.md).  
  
 Les valeurs de **object_id1** par **object_id4** dépendent du type de la demande de travail. Le tableau suivant récapitule la signification de ces colonnes pour différents types de travaux.  
  
|Type de demande|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|Statistiques de mises à jour asynchrones|Identificateur de table ou de vue|Identificateur de statistiques|Non utilisé|Non utilisé|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne le nombre de travaux asynchrones actifs dans la file d'attente en arrière plan pour chaque base de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
  




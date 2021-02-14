---
description: sys.dm_exec_background_job_queue_stats (Transact-SQL)
title: sys.dm_exec_background_job_queue_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_background_job_queue_stats
- sys.dm_exec_background_job_queue_stats
- dm_exec_background_job_queue_stats_TSQL
- sys.dm_exec_background_job_queue_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue_stats dynamic management view
ms.assetid: 27f62ab5-46c4-417e-814d-8d6437034d1c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4fd61039a4377850089abade5c851a3e016ee91b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343018"
---
# <a name="sysdm_exec_background_job_queue_stats-transact-sql"></a>sys.dm_exec_background_job_queue_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne qui fournit des statistiques agrégées pour chaque travail du processeur de requêtes soumis pour une exécution asynchrone (en arrière-plan).  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_exec_background_job_queue_stats**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**queue_max_len**|**int**|Longueur maximale de la file d'attente.|  
|**enqueued_count**|**int**|Nombre de demandes placées dans la file d'attente.|  
|**started_count**|**int**|Nombre de demandes dont l'exécution a commencé.|  
|**ended_count**|**int**|Nombre de demandes dont l'exécution s'est terminée sur un succès ou un échec.|  
|**failed_lock_count**|**int**|Nombre de demandes ayant échoué à cause d'un problème de contention de verrouillage ou de blocage.|  
|**failed_other_count**|**int**|Nombre de demandes ayant échoué pour d'autres raisons.|  
|**failed_giveup_count**|**int**|Nombre de demandes ayant échoué parce que le nombre limite de tentatives était atteint.|  
|**enqueue_failed_full_count**|**int**|Nombre de tentatives d'empilement ayant échoué parce que la file d'attente était saturée.|  
|**enqueue_failed_duplicate_count**|**int**|Nombre de tentatives d'empilement en double.|  
|**elapsed_avg_ms**|**int**|Temps moyen écoulé par demande en millisecondes.|  
|**elapsed_max_ms**|**int**|Temps écoulé pour la demande la plus longue, en millisecondes.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="remarks"></a>Notes  
 Seules les informations pour les travaux de mise à jour des statistiques asynchrone apparaissent dans cette vue. Pour plus d’informations sur les statistiques des mises à jour asynchrones, consultez [Statistics](../../relational-databases/statistics/statistics.md).  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="a-determining-the-percentage-of-failed-background-jobs"></a>R. Détermination du pourcentage des travaux d'arrière-plan ayant échoué  
 L'exemple suivant retourne le pourcentage de travaux d'arrière-plan ayant échoué pour toutes les requêtes exécutées.  
  
```  
SELECT   
        CASE ended_count WHEN 0   
                THEN 'No jobs ended'   
                ELSE CAST((failed_lock_count + failed_giveup_count + failed_other_count) / CAST(ended_count AS float) * 100 AS varchar(20))   
        END AS [Percent Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
### <a name="b-determining-the-percentage-of-failed-enqueue-attempts"></a>B. Détermination du pourcentage des tentatives d'empilement ayant échoué  
 L'exemple suivant retourne le pourcentage des tentatives d'empilement ayant échoué pour toutes les requêtes exécutées.  
  
```  
SELECT   
        CASE enqueued_count WHEN 0   
                THEN 'No jobs posted'   
                ELSE CAST((enqueue_failed_full_count + enqueue_failed_duplicate_count) / CAST(enqueued_count + enqueue_failed_full_count + enqueue_failed_duplicate_count AS float) * 100 AS varchar(20))   
        END AS [Percent Enqueue Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



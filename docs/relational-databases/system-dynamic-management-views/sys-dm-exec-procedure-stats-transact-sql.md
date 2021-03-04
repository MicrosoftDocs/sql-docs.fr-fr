---
description: sys.dm_exec_procedure_stats (Transact-SQL)
title: sys.dm_exec_procedure_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ac7de1b87411c62c97041c92efafa8bd53ba0009
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839274"
---
# <a name="sysdm_exec_procedure_stats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne les statistiques sur les performances des agrégats pour les procédures stockées mises en cache. La vue retourne une ligne pour chaque plan de procédure stockée mise en cache et la durée de vie de la ligne correspond à celle pendant laquelle la procédure stockée reste mise en cache. Lorsqu'une procédure stockée est supprimée du cache, la ligne correspondante est éliminée de cette vue. Un événement de trace SQL de statistiques de performances similaire à **sys.dm_exec_query_stats** est alors déclenché.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  
  
> [!NOTE]
> Les résultats de **sys.dm_exec_procedure_stats**  peuvent varier en fonction de chaque exécution, car les données reflètent uniquement les requêtes terminées, et non celles toujours en cours.
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_exec_procedure_stats**. 

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------| 
|**database_id**|**int**|ID de base de données dans lequel réside la procédure stockée.|  
|**object_id**|**int**|Numéro d'identification d'objet de la procédure stockée.|  
|**type**|**char(2)**|Type de l'objet :<br /><br /> P = Procédure stockée SQL<br /><br /> PC = Procédure stockée d'assembly (CLR)<br /><br /> X = Procédure stockée étendue|  
|**type_desc**|**nvarchar(60)**|Description du type d'objet :<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary(64)**|Cela peut être utilisé pour établir une corrélation avec des requêtes dans **sys.dm_exec_query_stats** qui ont été exécutées à partir de cette procédure stockée.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la vue de gestion dynamique **sys.dm_exec_cached_plans** .<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**cached_time**|**datetime**|Heure à laquelle la procédure stockée a été ajoutée au cache.|  
|**last_execution_time**|**datetime**|Heure de dernière exécution de la procédure stockée.|  
|**execution_count**|**bigint**|Nombre de fois où la procédure stockée a été exécutée depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Temps processeur total, en microsecondes, consommé par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Pour les procédures stockées compilées en mode natif, **total_worker_time** peut être inexact si plusieurs exécutions sont réalisées en moins d’une milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution de la procédure stockée. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Temps processeur minimal, en microsecondes, consommé par cette procédure stockée lors d’une seule exécution. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, consommé par cette procédure stockée lors d’une seule exécution. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution de la procédure stockée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Nombre de pages du pool de mémoires tampons modifiées lors de la dernière exécution du plan. Si une page est déjà modifiée, aucune écriture n'est comptée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Nombre minimal d’écritures logiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Nombre maximal d’écritures logiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution de la procédure stockée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, en microsecondes, pour les exécutions de cette procédure stockée.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, en microsecondes, pour la dernière exécution de cette procédure stockée.|  
|**min_elapsed_time**|**bigint**|Temps minimal écoulé, en microsecondes, pour toutes les exécutions de cette procédure stockée.|  
|**max_elapsed_time**|**bigint**|Temps maximal écoulé, en microsecondes, pour toutes les exécutions de cette procédure stockée.|  
|**total_spills**|**bigint**|Nombre total de pages vidées par l’exécution de cette procédure stockée depuis sa compilation.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Nombre de pages débordées lors de la dernière exécution de la procédure stockée.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Nombre minimal de pages que cette procédure stockée a déjà renversées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Nombre maximal de pages que cette procédure stockée a déjà débordées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Identificateur du nœud sur lequel cette distribution se trouve.<br /><br />**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
|**total_page_server_reads**|**bigint**|Nombre total de lectures du serveur de pages effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**last_page_server_reads**|**bigint**|Nombre de lectures du serveur de pages effectuées lors de la dernière exécution de la procédure stockée.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**min_page_server_reads**|**bigint**|Nombre minimal de lectures du serveur de pages effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**max_page_server_reads**|**bigint**|Nombre maximal de lectures du serveur de pages effectuées par cette procédure stockée lors d’une seule exécution.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
  
 <sup>1</sup> pour les procédures stockées compilées en mode natif lorsque la collecte de statistiques est activée, le temps de travail est collecté en millisecondes. Si la requête s'exécute en moins d'une milliseconde, la valeur est 0.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Les statistiques de la vue sont mises à jour lorsqu'une exécution de procédure stockée se termine.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les dix principales procédures stockées identifiées d'après le temps moyen écoulé.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  

---
description: sys.dm_exec_trigger_stats (Transact-SQL)
title: sys.dm_exec_trigger_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd36c570985d5b8df124a8ff793cdeee9330e128
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342882"
---
# <a name="sysdm_exec_trigger_stats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne les statistiques sur les performances des agrégats pour les déclencheurs mis en cache. La vue contient une ligne par déclencheur, et la durée de vie de la ligne correspond à celle pendant laquelle le déclencheur reste mis en cache. Lorsqu'un déclencheur est supprimé du cache, la ligne correspondante est éliminée de cette vue. Un événement de trace SQL de statistiques de performances similaire à **sys.dm_exec_query_stats** est alors déclenché.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de base de données dans lequel réside le déclencheur.|  
|**object_id**|**int**|Numéro d'identification d'objet du déclencheur.|  
|**type**|**char(2)**|Type de l'objet :<br /><br /> TA = Déclencheur assembly (CLR)<br /><br /> TR = Déclencheur SQL|  
|**Type_desc**|**nvarchar(60)**|Description du type d'objet :<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary(64)**|Cela peut être utilisé pour établir une corrélation avec des requêtes dans **sys.dm_exec_query_stats** qui ont été exécutées à partir de ce déclencheur.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la vue de gestion dynamique **sys.dm_exec_cached_plans** .|  
|**cached_time**|**datetime**|Heure à laquelle le déclencheur a été ajouté au cache.|  
|**last_execution_time**|**datetime**|Heure de dernière exécution du déclencheur.|  
|**execution_count**|**bigint**|Nombre de fois où le déclencheur a été exécuté depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Temps processeur total, en microsecondes, consommé par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution du déclencheur.|  
|**min_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, consommé par ce déclencheur lors d’une seule exécution.|  
|**max_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, consommé par ce déclencheur lors d’une seule exécution.|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution du déclencheur.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_logical_writes**|**bigint**|Nombre d’écritures logiques effectuées lors de la dernière exécution du déclencheur.|  
|**min_logical_writes**|**bigint**|Nombre minimal d’écritures logiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**max_logical_writes**|**bigint**|Nombre maximal d’écritures logiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution du déclencheur.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées par ce déclencheur lors d’une seule exécution.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, en microsecondes, pour les exécutions de ce déclencheur.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, en microsecondes, pour la dernière exécution de ce déclencheur.|  
|**min_elapsed_time**|**bigint**|Temps minimal écoulé, en microsecondes, pour toutes les exécutions de ce déclencheur.|  
|**max_elapsed_time**|**bigint**|Temps maximal écoulé, en microsecondes, pour toutes les exécutions de ce déclencheur.| 
|**total_spills**|**bigint**|Nombre total de pages déduites par l’exécution de ce déclencheur depuis sa compilation.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Nombre de pages débordées lors de la dernière exécution du déclencheur.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Nombre minimal de pages que ce déclencheur a déjà renversées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Nombre maximal de pages que ce déclencheur a déjà renversées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**total_page_server_reads**|**bigint**|Nombre total de lectures du serveur de pages effectuées par les exécutions de ce déclencheur depuis sa compilation.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**last_page_server_reads**|**bigint**|Nombre de lectures du serveur de pages effectuées lors de la dernière exécution du déclencheur.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**min_page_server_reads**|**bigint**|Nombre minimal de lectures de serveur de pages effectuées par ce déclencheur lors d’une seule exécution.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  
|**max_page_server_reads**|**bigint**|Nombre maximal de lectures de serveur de pages effectuées par ce déclencheur lors d’une seule exécution.<br /><br /> **S’applique à**: Azure SQL Database hyperscale|  

  
## <a name="remarks"></a>Notes  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  

Les statistiques de la vue sont actualisées lorsqu'une requête est terminée.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les cinq principaux déclencheurs identifiés d'après le temps moyen écoulé.  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  

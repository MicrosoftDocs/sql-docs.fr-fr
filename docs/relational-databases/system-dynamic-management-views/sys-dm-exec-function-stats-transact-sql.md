---
description: sys.dm_exec_function_stats (Transact-SQL)
title: sys.dm_exec_function_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6acd8d9cdc09bcffc9e5ef95f6d3e7ed700ffafb
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836566"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Retourne des statistiques de performances agrégées pour les fonctions mises en cache. La vue retourne une ligne pour chaque plan de fonction mis en cache, et la durée de vie de la ligne est tant que la fonction reste en cache. Lorsqu’une fonction est supprimée du cache, la ligne correspondante est éliminée de cette vue. Un événement de trace SQL de statistiques de performances similaire à **sys.dm_exec_query_stats** est alors déclenché. Retourne des informations sur les fonctions scalaires, y compris les fonctions en mémoire et les fonctions scalaires CLR. Ne retourne pas d’informations sur les fonctions table et sur les fonctions scalaires qui sont Inline avec l' [incorporation de FDU scalaire](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  
  
> [!NOTE]
> Les résultats de **sys.dm_exec_function_stats**  peuvent varier en fonction de chaque exécution, car les données reflètent uniquement les requêtes terminées, et non celles toujours en cours. 

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de base de données dans lequel la fonction réside.|  
|**object_id**|**int**|Numéro d’identification d’objet de la fonction.|  
|**type**|**char(2)**|Type de l’objet : FN = fonctions scalaires|  
|**type_desc**|**nvarchar(60)**|Description du type d’objet : SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary(64)**|Cela peut être utilisé pour établir une corrélation avec des requêtes dans **sys.dm_exec_query_stats** qui ont été exécutées à partir de cette fonction.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la vue de gestion dynamique **sys.dm_exec_cached_plans** .<br /><br /> Sera toujours 0x000 quand une fonction compilée en mode natif interroge une table optimisée en mémoire.|  
|**cached_time**|**datetime**|Heure à laquelle la fonction a été ajoutée au cache.|  
|**last_execution_time**|**datetime**|Heure à laquelle la fonction a été exécutée pour la dernière fois.|  
|**execution_count**|**bigint**|Nombre de fois où la fonction a été exécutée depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Temps processeur total, en microsecondes, consommé par les exécutions de cette fonction depuis sa compilation.<br /><br /> Pour les fonctions compilées en mode natif, **total_worker_time** peut ne pas être précis si de nombreuses exécutions prennent moins de 1 milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution de la fonction. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Temps processeur minimal, en microsecondes, que cette fonction a jamais consommé lors d’une seule exécution. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, consommé par cette fonction lors d’une seule exécution. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de cette fonction depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution de la fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de cette fonction depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Numéro du nombre de pages du pool de mémoires tampons modifiées lors de la dernière exécution du plan. Si une page est déjà modifiée, aucune écriture n'est comptée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Nombre minimal d’écritures logiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Nombre maximal d’écritures logiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de cette fonction depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution de la fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées par cette fonction lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, en microsecondes, pour les exécutions de cette fonction.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, en microsecondes, pour la dernière exécution de cette fonction.|  
|**min_elapsed_time**|**bigint**|Temps minimum écoulé, en microsecondes, pour toutes les exécutions de cette fonction.|  
|**max_elapsed_time**|**bigint**|Temps maximal écoulé, en microsecondes, pour toutes les exécutions de cette fonction.|  
|**total_page_server_reads**|**bigint**|Nombre total de lectures du serveur de pages effectuées par les exécutions de cette fonction depuis sa compilation.<br /><br /> **S’applique à :** Azure SQL Database hyperscale.|  
|**last_page_server_reads**|**bigint**|Nombre de lectures du serveur de pages effectuées lors de la dernière exécution de la fonction.<br /><br /> **S’applique à :** Azure SQL Database hyperscale.|  
|**min_page_server_reads**|**bigint**|Nombre minimal de lectures du serveur de pages effectuées par cette fonction lors d’une seule exécution.<br /><br /> **S’applique à :** Azure SQL Database hyperscale.|  
|**max_page_server_reads**|**bigint**|Nombre maximal de lectures du serveur de pages effectuées par cette fonction lors d’une seule exécution.<br /><br /> **S’applique à :** Azure SQL Database hyperscale.|
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne des informations sur les dix principales fonctions identifiées par le temps moyen écoulé.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  

---
description: sys.query_store_query (Transact-SQL)
title: sys.query_store_query (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_QUERY
- SYS.QUERY_STORE_QUERY_TSQL
- SYS.QUERY_STORE_QUERY
- QUERY_STORE_QUERY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_query catalog view
- sys.query_store_query catalog view
ms.assetid: bdee149e-7556-4fc3-8242-925dd4b7b6ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e767ee7ceae909637e7675d536cbac7e47715ccd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095457"
---
# <a name="sysquery_store_query-transact-sql"></a>sys.query_store_query (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contient des informations sur la requête et ses statistiques d’exécution agrégées globales associées.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**query_id**|**bigint**|Clé primaire|  
|**query_text_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)|  
|**context_settings_id**|**bigint**|Clé étrangère. Jointures à [sys.query_context_settings &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md).<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**object_id**|**bigint**|ID de l’objet de base de données qui fait partie de la requête (procédure stockée, déclencheur, UDF CLR/UDAgg, etc.). 0 si la requête n’est pas exécutée dans le cadre d’un objet de base de données (requête ad hoc).<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**batch_sql_handle**|**varbinary(64)**|ID du lot d’instructions dont fait partie la requête. Renseigné uniquement si la requête fait référence à des tables temporaires ou des variables de table.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la *valeur null*.|  
|**query_hash**|**Binary(8**|Hachage MD5 de la requête individuelle, basé sur l’arborescence de requêtes logique. Comprend des indicateurs d’optimiseur.|  
|**is_internal_query**|**bit**|La requête a été générée en interne.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**query_parameterization_type**|**tinyint**|Genre de paramétrage :<br /><br /> 0 - Aucun<br /><br /> 1-utilisateur<br /><br /> 2-simple<br /><br /> 3-forcé<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**query_parameterization_type_desc**|**nvarchar(60)**|Description textuelle pour le type de paramétrage.<br/>**Remarque :** Azure Synapse Analytics ne retourne jamais *aucun*.|  
|**initial_compile_start_time**|**datetimeoffset**|Heure de début de la compilation.|  
|**last_compile_start_time**|**datetimeoffset**|Heure de début de la compilation.|  
|**last_execution_time**|**datetimeoffset**|La dernière heure d’exécution fait référence à la dernière heure de fin de la requête ou du plan.|  
|**last_compile_batch_sql_handle**|**varbinary(64)**|Handle du dernier traitement SQL dans lequel la requête a été utilisée pour la dernière fois. Il peut être fourni comme entrée à [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) pour obtenir le texte complet du lot.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la *valeur null*.|  
|**last_compile_batch_offset_start**|**bigint**|Informations qui peuvent être fournies pour sys.dm_exec_sql_text avec last_compile_batch_sql_handle.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**last_compile_batch_offset_end**|**bigint**|Informations qui peuvent être fournies pour sys.dm_exec_sql_text avec last_compile_batch_sql_handle.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**count_compiles**|**bigint**|Statistiques de compilation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours un (1).|  
|**avg_compile_duration**|**float**|Statistiques de compilation en microsecondes.|  
|**last_compile_duration**|**bigint**|Statistiques de compilation en microsecondes.|  
|**avg_bind_duration**|**float**|Statistiques de liaison en microsecondes.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**last_bind_duration**|**bigint**|Statistiques de liaison.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**avg_bind_cpu_time**|**float**|Statistiques de liaison.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**last_bind_cpu_time**|**bigint**|Statistiques de liaison.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**avg_optimize_duration**|**float**|Statistiques d’optimisation en microsecondes.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**last_optimize_duration**|**bigint**|Statistiques d’optimisation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**avg_optimize_cpu_time**|**float**|Statistiques d’optimisation en microsecondes.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**last_optimize_cpu_time**|**bigint**|Statistiques d’optimisation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**avg_compile_memory_kb**|**float**|Statistiques de la mémoire de compilation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**last_compile_memory_kb**|**bigint**|Statistiques de la mémoire de compilation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**max_compile_memory_kb**|**bigint**|Statistiques de la mémoire de compilation.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
|**is_clouddb_internal_query**|**bit**|Toujours 0 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le local.<br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **View Database State** .  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  

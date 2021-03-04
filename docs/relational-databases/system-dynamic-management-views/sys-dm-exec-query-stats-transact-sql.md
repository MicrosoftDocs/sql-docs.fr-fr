---
description: sys.dm_exec_query_stats (Transact-SQL)
title: sys.dm_exec_query_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_stats_TSQL
- dm_exec_query_stats
- sys.dm_exec_query_stats
- sys.dm_exec_query_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_stats dynamic management view
ms.assetid: eb7b58b8-3508-4114-97c2-d877bcb12964
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 91b6aed3ba70b9fab4d161f5f70a2db5ab8c28a1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837992"
---
# <a name="sysdm_exec_query_stats-transact-sql"></a>sys.dm_exec_query_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne les statistiques sur les performances des agrégats pour les plans de requête mis en cache dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La vue contient une ligne par instruction de requête dans le plan en cache et la durée de vie des lignes est liée au plan lui-même. Lorsqu'un plan est supprimé du cache, les lignes correspondantes sont éliminées de cette vue.  
  
> [!NOTE]
> - Les résultats de **sys.dm_exec_query_stats**  peuvent varier en fonction de chaque exécution, car les données reflètent uniquement les requêtes terminées, et non celles toujours en cours.
> - Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_exec_query_stats**.    

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**sql_handle**|**varbinary(64)**  |Jeton qui identifie de façon unique le lot ou la procédure stockée dont fait partie la requête.<br /><br /> **sql_handle**, combiné avec **statement_start_offset** et **statement_end_offset**, peut s'utiliser pour extraire le texte SQL de la requête en appelant la fonction de gestion dynamique **sys.dm_exec_sql_text**.|  
|**statement_start_offset**|**int**|Indique, en octets, la position de début (à partir de 0) de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant.|  
|**statement_end_offset**|**int**|Indique, en octets, la position de fin (à partir de 0) de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant. Pour les versions antérieures à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , la valeur-1 indique la fin du traitement. Les commentaires de fin ne sont plus inclus.|  
|**plan_generation_num**|**bigint**|Numéro de séquence permettant de distinguer les instances de plans après une recompilation.|  
|**plan_handle**|**varbinary(64)**|Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan, ou qui est en cours d’exécution. Cette valeur peut être passée à la fonction de gestion dynamique [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) pour obtenir le plan de requête.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**creation_time**|**datetime**|Heure de compilation du plan.|  
|**last_execution_time**|**datetime**|Heure de début de la dernière exécution du plan.|  
|**execution_count**|**bigint**|Nombre d'exécutions du plan depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Temps processeur total, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé par les exécutions de ce plan depuis sa compilation.<br /><br /> Pour les procédures stockées compilées en mode natif, **total_worker_time** peut être inexact si plusieurs exécutions sont réalisées en moins d’une milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé lors de la dernière exécution du plan. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Temps processeur minimum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan en une seule exécution. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan en une seule exécution. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution du plan.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d'écritures logiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Nombre de pages du pool de mémoires tampons modifiées lors de la dernière exécution du plan.<br /><br />Après la lecture d’une page, la page n’est modifiée que lors de sa première modification. Quand une page est modifiée, ce nombre est incrémenté. Les modifications ultérieures d’une page déjà modifiée n’affectent pas ce nombre.<br /><br />Ce nombre sera toujours égal à 0 lors de l’interrogation d’une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Nombre minimal d'écritures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Nombre maximal d'écritures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution du plan.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées par ce plan lors d'une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_clr_time**|**bigint**|Temps, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé dans les [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] objets Common Language Runtime (CLR) par les exécutions de ce plan depuis sa compilation. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**last_clr_time**|**bigint**|Temps, indiqué en microsecondes (mais précis uniquement en millisecondes), utilisé par l'exécution dans les objets CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pendant la dernière exécution de ce plan. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**min_clr_time**|**bigint**|Temps minimum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan dans les objets CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] en une seule exécution. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**max_clr_time**|**bigint**|Temps maximum, indiqué en microsecondes (mais précis uniquement en millisecondes), jamais utilisé par ce plan dans les objets CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] en une seule exécution. Les objets CLR peuvent être des procédures stockées, des fonctions, des déclencheurs, des types et des agrégats.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les exécutions de ce plan.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour la dernière exécution de ce plan.|  
|**min_elapsed_time**|**bigint**|Temps minimum écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les différentes exécutions de ce plan.|  
|**max_elapsed_time**|**bigint**|Temps maximum écoulé, indiqué en microsecondes (mais précis uniquement en millisecondes), pour les différentes exécutions de ce plan.|  
|**query_hash**|**Binaire (8)**|La valeur de hachage binaire calculée sur la requête et utilisée pour identifier des requêtes avec une logique similaire. Vous pouvez utiliser le hachage de requête pour déterminer l'utilisation des ressources globale pour les requêtes qui diffèrent uniquement par les valeurs littérales.|  
|**query_plan_hash**|**Binary(8**|Valeur de hachage binaire calculée sur le plan d'exécution de requête et utilisée pour identifier des plans d'exécution de requête semblables. Vous pouvez utiliser le hachage de plan de requête pour rechercher le coût cumulatif de requêtes avec les plans d'exécution semblables.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**total_rows**|**bigint**|Nombre total de lignes renvoyées par la requête. Ne peut pas être null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**last_rows**|**bigint**|Nombre de lignes renvoyées par la dernière exécution de la requête. Ne peut pas être null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**min_rows**|**bigint**|Nombre minimal de lignes retournées par la requête au cours d’une exécution. Ne peut pas être null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**max_rows**|**bigint**|Nombre maximal de lignes retournées par la requête au cours d’une exécution. Ne peut pas être null.<br /><br /> Sa valeur est toujours 0 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**statement_sql_handle**|**varbinary(64)**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Renseigné avec des valeurs non NULL uniquement si Magasin des requêtes est activé et collecte les statistiques de cette requête particulière.|  
|**statement_context_id**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Renseigné avec des valeurs non NULL uniquement si Magasin des requêtes est activé et collecte les statistiques de cette requête particulière.|  
|**total_dop**|**bigint**|Somme totale du degré de parallélisme utilisé par ce plan depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_dop**|**bigint**|Degré de parallélisme de la dernière exécution de ce plan. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_dop**|**bigint**|Degré minimal de parallélisme que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_dop**|**bigint**|Degré maximal de parallélisme que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_grant_kb**|**bigint**|Quantité totale d’allocation de mémoire réservée en Ko reçue par ce plan depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_grant_kb**|**bigint**|Quantité d’allocation de mémoire réservée en Ko lorsque ce plan a été exécuté pour la dernière fois. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_grant_kb**|**bigint**|Quantité minimale d’allocation de mémoire réservée, en Ko, que ce plan a jamais reçu pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_grant_kb**|**bigint**|Quantité maximale d’allocation de mémoire réservée, en Ko, que ce plan a jamais reçue pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_used_grant_kb**|**bigint**|Quantité totale d’allocation de mémoire réservée, en Ko, utilisée par ce plan depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_used_grant_kb**|**bigint**|Quantité d’allocation de mémoire utilisée en Ko lorsque ce plan a été exécuté pour la dernière fois. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_used_grant_kb**|**bigint**|Quantité minimale d’allocation de mémoire utilisée en Ko que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_used_grant_kb**|**bigint**|Quantité maximale d’allocation de mémoire utilisée en Ko que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_ideal_grant_kb**|**bigint**|Quantité totale d’allocation de mémoire idéale en Ko ce plan est estimé depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_ideal_grant_kb**|**bigint**|Quantité d’allocation de mémoire idéale en Ko lorsque ce plan a été exécuté pour la dernière fois. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_ideal_grant_kb**|**bigint**|Quantité minimale d’allocation de mémoire idéale en Ko que ce plan a jamais estimé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_ideal_grant_kb**|**bigint**|Quantité maximale d’allocation de mémoire idéale en Ko que ce plan a jamais estimé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_reserved_threads**|**bigint**|Somme totale des threads parallèles réservés que ce plan a jamais utilisés depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_reserved_threads**|**bigint**|Nombre de threads parallèles réservés lorsque ce plan a été exécuté pour la dernière fois. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_reserved_threads**|**bigint**|Nombre minimal de threads parallèles réservés que ce plan a jamais utilisé pendant une exécution.  Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_reserved_threads**|**bigint**|Nombre maximal de threads parallèles réservés que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_used_threads**|**bigint**|Somme totale des threads parallèles utilisés que ce plan a jamais utilisé depuis sa compilation. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**last_used_threads**|**bigint**|Nombre de threads parallèles utilisés lors de la dernière exécution de ce plan. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**min_used_threads**|**bigint**|Nombre minimal de threads parallèles utilisés que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**max_used_threads**|**bigint**|Nombre maximal de threads parallèles utilisés que ce plan a jamais utilisé pendant une exécution. Elle sera toujours 0 pour l’interrogation d’une table optimisée en mémoire.<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
|**total_columnstore_segment_reads**|**bigint**|Somme totale des segments ColumnStore lus par la requête. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_reads**|**bigint**|Nombre de segments ColumnStore lus par la dernière exécution de la requête. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_reads**|**bigint**|Nombre minimal de segments ColumnStore lus par la requête au cours d’une exécution. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_reads**|**bigint**|Nombre maximal de segments ColumnStore lus par la requête au cours d’une exécution. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**total_columnstore_segment_skips**|**bigint**|Somme totale des segments ColumnStore ignorés par la requête. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_skips**|**bigint**|Nombre de segments ColumnStore ignorés par la dernière exécution de la requête. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_skips**|**bigint**|Nombre minimal de segments ColumnStore ignorés par la requête au cours d’une exécution. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_skips**|**bigint**|Nombre maximal de segments ColumnStore jamais ignorés par la requête pendant une exécution. Ne peut pas être null.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|
|**total_spills**|**bigint**|Nombre total de pages déduites par l’exécution de cette requête depuis sa compilation.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Nombre de pages débordées lors de la dernière exécution de la requête.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Nombre minimal de pages que cette requête a déjà débordées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Nombre maximal de pages que cette requête a déjà débordées lors d’une seule exécution.<br /><br /> **S’applique à**: à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Identificateur du nœud sur lequel cette distribution se trouve.<br /><br /> **S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]| 
|**total_page_server_reads**|**bigint**|Nombre total de lectures du serveur de pages distantes effectuées par les exécutions de ce plan depuis sa compilation.<br /><br /> **S’applique à :** Azure SQL Database hyperscale |  
|**last_page_server_reads**|**bigint**|Nombre de lectures du serveur de pages distantes effectuées lors de la dernière exécution du plan.<br /><br /> **S’applique à :** Azure SQL Database hyperscale |  
|**min_page_server_reads**|**bigint**|Nombre minimal de lectures du serveur de pages distantes effectuées par ce plan lors d’une seule exécution.<br /><br /> **S’applique à :** Azure SQL Database hyperscale |  
|**max_page_server_reads**|**bigint**|Nombre maximal de lectures du serveur de pages distantes effectuées par ce plan lors d’une seule exécution.<br /><br /> **S’applique à :** Azure SQL Database hyperscale |  
> [!NOTE]
> <sup>1</sup> pour les procédures stockées compilées en mode natif lorsque la collecte de statistiques est activée, le temps de travail est collecté en millisecondes. Si la requête s’exécute en moins d’une milliseconde, la valeur sera 0.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Les statistiques de la vue sont actualisées lorsqu'une requête est terminée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-top-n-queries"></a>R. Recherche des N premières requêtes (TOP N)  
 L’exemple suivant renvoie des informations relatives aux cinq premières requêtes, classées sur la base du temps processeur moyen. Cet exemple regroupe les requêtes d'après leur hachage de requête afin que les requêtes logiquement équivalentes soient groupées par leur consommation de ressources cumulative.  
  
```sql  
SELECT TOP 5 query_stats.query_hash AS "Query Hash",   
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",  
    MIN(query_stats.statement_text) AS "Statement Text"  
FROM   
    (SELECT QS.*,   
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,  
    ((CASE statement_end_offset   
        WHEN -1 THEN DATALENGTH(ST.text)  
        ELSE QS.statement_end_offset END   
            - QS.statement_start_offset)/2) + 1) AS statement_text  
     FROM sys.dm_exec_query_stats AS QS  
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats  
GROUP BY query_stats.query_hash  
ORDER BY 2 DESC;  
```  
  
### <a name="b-returning-row-count-aggregates-for-a-query"></a>B. Retour des agrégats de nombre de lignes à une requête  
 L'exemple suivant retourne les informations d'agrégation du nombre de lignes (nombre total de lignes, nombre minimal de lignes, nombre maximal de lignes et dernières lignes) pour les requêtes.  
  
```sql  
SELECT qs.execution_count,  
    SUBSTRING(qt.text,qs.statement_start_offset/2 +1,   
                 (CASE WHEN qs.statement_end_offset = -1   
                       THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2   
                       ELSE qs.statement_end_offset end -  
                            qs.statement_start_offset  
                 )/2  
             ) AS query_text,   
     qt.dbid, dbname= DB_NAME (qt.dbid), qt.objectid,   
     qs.total_rows, qs.last_rows, qs.min_rows, qs.max_rows  
FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS qt   
WHERE qt.text like '%SELECT%'   
ORDER BY qs.execution_count DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)    
[sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)     
[sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)     
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    

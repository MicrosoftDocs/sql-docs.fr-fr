---
title: sys.database_query_store_options (Transact-SQL)
description: sys.database_query_store_options (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DATABASE_QUERY_STORE_OPTIONS_TSQL
- DATABASE_QUERY_STORE_OPTIONS
- SYS.DATABASE_QUERY_STORE_OPTIONS_TSQL
- SYS.DATABASE_QUERY_STORE_OPTIONS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_query_store_options catalog view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 1/8/2021
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 273e5c4446853c3f44d0c99535880c9c9da2aa5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098056"
---
# <a name="sysdatabase_query_store_options-transact-sql"></a>sys.database_query_store_options (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Retourne les options de Magasin des requêtes pour cette base de données.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**desired_state**|**smallint**|Indique le mode d’opération souhaité de Magasin des requêtes, défini explicitement par l’utilisateur.<br /> 0 = désactivé <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE|  
|**desired_state_desc**|**nvarchar(60)**|Description textuelle du mode d’opération souhaité de Magasin des requêtes :<br />OFF<br />READ_ONLY<br />READ_WRITE|  
|**actual_state**|**smallint**|Indique le mode d’opération de Magasin des requêtes. En plus de la liste des États souhaités requis par l’utilisateur, l’état réel peut être un état d’erreur.<br /> 0 = désactivé <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE<br /> 3 = ERREUR|  
|**actual_state_desc**|**nvarchar(60)**|Description textuelle du mode d’opération réel de Magasin des requêtes.<br />OFF<br />READ_ONLY<br />READ_WRITE<br />ERROR<br /><br /> Il existe des situations dans lesquelles l’état réel diffère de l’état souhaité :<br />-Si la base de données est définie en mode lecture seule ou si Magasin des requêtes taille dépasse son quota configuré, Magasin des requêtes peut fonctionner en mode lecture seule même si l’utilisateur a spécifié l’accès en lecture-écriture.<br />-Dans les scénarios extrêmes Magasin des requêtes pouvez entrer un état d’erreur en raison d’erreurs internes. À compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] , si cela se produit, magasin des requêtes peut être récupéré en exécutant la `sp_query_store_consistency_check` procédure stockée dans la base de données affectée. Si l’exécution `sp_query_store_consistency_check` ne fonctionne pas, ou si vous utilisez [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] , vous devrez effacer les données en exécutant `ALTER DATABASE [YourDatabaseName] SET QUERY_STORE CLEAR ALL;`|  
|**readonly_reason**|**int**|Lorsque le **desired_state_desc** est READ_WRITE et que le **actual_state_desc** est READ_ONLY, **readonly_reason** retourne une table de bits pour indiquer la raison pour laquelle le magasin des requêtes est en mode lecture seule.<br /><br /> **1** -la base de données est en mode lecture seule<br /><br /> **2** -la base de données est en mode mono-utilisateur<br /><br /> **4** -la base de données est en mode urgence<br /><br /> **8** -la base de données est un réplica secondaire (s’applique à la Always on et à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] la géo-réplication). Cette valeur peut être observée efficacement uniquement sur les réplicas secondaires **accessibles en lecture** .<br /><br /> **65536** -la magasin des requêtes a atteint la limite de taille définie par l' `MAX_STORAGE_SIZE_MB` option. Pour plus d’informations sur cette option, consultez [options SET de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).<br /><br /> **131072** -le nombre d’instructions différentes dans magasin des requêtes a atteint la limite de mémoire interne. Envisagez de supprimer les requêtes dont vous n’avez pas besoin ou de procéder à une mise à niveau vers un niveau de service supérieur pour permettre le transfert de Magasin des requêtes en mode lecture-écriture.<br /><br /><br /> **262144** -la taille des éléments en mémoire en attente de conservation sur le disque a atteint la limite de mémoire interne. Magasin des requêtes sera temporairement en mode lecture seule jusqu’à ce que les éléments en mémoire soient conservés sur le disque. <br /><br /><br /> **524288** -la base de données a atteint la limite de taille de disque. Magasin des requêtes fait partie de la base de données utilisateur, si bien qu’il n’y a plus d’espace disponible pour une base de données, cela signifie que les Magasin des requêtes ne peuvent plus croître.<br /><br /> <br /> Pour rebasculer le mode d’opération Magasin des requêtes en lecture-écriture, consultez **Verify magasin des requêtes collecte des données de requête en continu** [dans la pratique recommandée avec la magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md#Verify).|  
|**current_storage_size_mb**|**bigint**|Taille de Magasin des requêtes sur le disque en mégaoctets.|  
|**flush_interval_seconds**|**bigint**|Période de vidage régulier des données de Magasin des requêtes sur le disque, en secondes. La valeur par défaut est **900** (15 minutes).<br /><br /> Modifiez à l’aide de l' `ALTER DATABASE <database> SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS  = <interval>)` instruction.|  
|**interval_length_minutes**|**bigint**|Intervalle d’agrégation des statistiques en minutes. Les valeurs arbitraires ne sont pas autorisées. Utilisez l’une des valeurs suivantes : 1, 5, 10, 15, 30, 60 et 1440 minutes. La valeur par défaut est de **60** minutes.|  
|**max_storage_size_mb**|**bigint**|Taille de disque maximale en mégaoctets (Mo) pour le Magasin des requêtes. La valeur par défaut est de **100** Mo jusqu’à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] , et **1 Go** à partir de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] .<br />Pour l' [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition Premium, la valeur par défaut est 1 Go et pour l' [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition de base, la valeur par défaut est 10 Mo.<br /><br /> Modifiez à l’aide de l' `ALTER DATABASE <database> SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <size>)` instruction.|  
|**stale_query_threshold_days**|**bigint**|Nombre de jours pendant lesquels les informations d’une requête sont conservées dans le Magasin des requêtes. La valeur par défaut est **30**. Affectez la valeur 0 pour désactiver la stratégie de rétention.<br />Pour l’édition [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] de base, la valeur par défaut est 7 jours.<br /><br /> Modifiez à l’aide de l' `ALTER DATABASE <database> SET QUERY_STORE ( CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = <value> ) )` instruction.|  
|**max_plans_per_query**|**bigint**|Limite le nombre maximal de plans stockés. La valeur par défaut est **200**. Si la valeur maximale est atteinte, Magasin des requêtes cesse de capturer de nouveaux plans pour cette requête. La valeur 0 supprime la limitation en ce qui concerne le nombre de plans capturés.<br /><br /> Modifiez à l’aide de l' `ALTER DATABASE<database> SET QUERY_STORE (MAX_PLANS_PER_QUERY = <n>)` instruction.|  
|**query_capture_mode**|**smallint**|Mode de capture de requête actuellement actif :<br /><br /> **1** = toutes les requêtes sont capturées. Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures).<br /><br /> 2 = capturer automatiquement les requêtes pertinentes en fonction du nombre d’exécutions et de la consommation des ressources. Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].<br /><br /> 3 = aucun-arrêter la capture des nouvelles requêtes. Le magasin de requêtes continuera à recueillir des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer des requêtes importantes. <br /><br /> 4 = personnalisé : permet un contrôle supplémentaire sur la stratégie de capture de requêtes à l’aide des [options de QUERY_CAPTURE_POLICY](../../t-sql/statements/alter-database-transact-sql-set-options.md#SettingOptions).<br /> **S’applique à** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et versions ultérieures.|  
|**query_capture_mode_desc**|**nvarchar(60)**|Description textuelle du mode de capture réel de Magasin des requêtes :<br /><br /> ALL (valeur par défaut pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] )<br /><br /> **Auto** (valeur par défaut pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] )<br /><br /> NONE <br /><br /> CUSTOM|  
|**capture_policy_execution_count**|**int**|Option de stratégie personnalisée du mode de capture de requête. Définit le nombre d’exécutions d’une requête pendant la période d’évaluation. La valeur par défaut est 30.<br />**S’applique à** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et versions ultérieures.| 
|**capture_policy_total_compile_cpu_time_ms**|**bigint**|Option de stratégie personnalisée du mode de capture de requête. Définit le temps UC de compilation écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 1000.<br /> **S’applique à** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et versions ultérieures.|
|**capture_policy_total_execution_cpu_time_ms**|**bigint**|Option de stratégie personnalisée du mode de capture de requête. Définit le temps UC d’exécution écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 100.<br /> **S’applique à** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et versions ultérieures.|
|**capture_policy_stale_threshold_hours**|**int**|Option de stratégie personnalisée du mode de capture de requête. Définit la période d’évaluation pour déterminer si une requête doit être capturée. La valeur par défaut est 24 heures.<br /> **S’applique à** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et versions ultérieures.|
|**size_based_cleanup_mode**|**smallint**|Contrôle si le nettoyage est activé automatiquement quand la quantité totale de données approche de la taille maximale :<br /><br /> 0 = le nettoyage basé sur une taille hors tension n’est pas activé automatiquement.<br /><br /> **1** = le nettoyage basé sur le format automatique est activé automatiquement lorsque la taille sur le disque atteint **90%** de *max_storage_size_mb*. Il s’agit de la valeur de configuration par défaut.<br /><br />Le nettoyage basé sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête quand environ **80%** de *max_storage_size_mb* sont atteints.|  
|**size_based_cleanup_mode_desc**|**nvarchar(60)**|Description textuelle du mode de nettoyage basé sur la taille réel de Magasin des requêtes :<br /><br /> OFF <br /> **Automatique** (par défaut)|  
|**wait_stats_capture_mode**|**smallint**|Contrôle si Magasin des requêtes effectue la capture des statistiques d’attente : <br /><br /> 0 = désactivé <br /> **1** = activé<br /> **S’applique à** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures.|
|**wait_stats_capture_mode_desc**|**nvarchar(60)**|Description textuelle du mode réel de capture des statistiques d’attente : <br /><br /> OFF <br /> **Activé** (par défaut)<br /> **S’applique à** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures.|
|**actual_state_additional_info**|**nvarchar (8000)**|Actuellement inutilisé. Peut être implémentée à l’avenir.|
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  

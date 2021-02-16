---
title: Infrastructure du profilage de requête | Microsoft Docs
description: Découvrez comment le moteur de base de données SQL Server accède aux informations d’exécution sur les plans d’exécution de requête pour comprendre la charge de travail et la façon dont l’utilisation des ressources est pilotée.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- query profiling
- lightweight query profiling
- lightweight profiling
- lwp
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 39e32e8715c92a14faf68fd22464a50fed349997
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347691"
---
# <a name="query-profiling-infrastructure"></a>Infrastructure du profilage de requête
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] offre la possibilité d’accéder aux informations d’exécution sur les plans d’exécution de requête. L’une des actions les plus importantes en cas de problème de performances consiste à bien comprendre la nature de la charge de travail en cours d’exécution et la façon dont est pilotée l’utilisation des ressources. Pour cela, l’accès au [plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md) est important.

Bien que la complétion de requête soit un prérequis à la disponibilité d’un plan de requête réel, les [statistiques des requêtes actives](../../relational-databases/performance/live-query-statistics.md) peuvent fournir des insights en temps réel concernant le processus d’exécution de requête à mesure que les données transitent d’un [opérateur de plan de requête](../../relational-databases/showplan-logical-and-physical-operators-reference.md) vers un autre. Le plan de requête active affiche la progression globale de la requête ainsi que des statistiques d’exécution de niveau opérateur telles que le nombre de lignes produites, le temps écoulé, la progression de l’opérateur, etc. Vous pouvez accéder à ces données en temps réel sans avoir à attendre l’exécution de la requête ; ces statistiques d’exécution se révèlent donc extrêmement utiles pour résoudre les problèmes de performances de requêtes, tels que les longues requêtes et celles qui s’exécutent indéfiniment sans jamais se terminer.

## <a name="the-standard-query-execution-statistics-profiling-infrastructure"></a>Infrastructure de profilage des statistiques d’exécution de requête standard

L’*infrastructure de profil de statistiques d’exécution de requête*, ou profilage standard, doit être activée pour collecter des informations sur les plans d’exécution, à savoir le nombre de lignes et l’utilisation de l’UC et des E/S. Les méthodes suivantes de collecte d’informations sur les plans d’exécution pour une **session cible** tirent parti de l’infrastructure de profilage standard :

- [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 
- [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)
- [Statistiques des requêtes dynamiques](../../relational-databases/performance/live-query-statistics.md)

> [!NOTE]
> Cliquer sur le bouton *inclure les statistiques des requêtes dynamiques* dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] s’appuie sur l’infrastructure de profilage standard.    
> Dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l’[infrastructure de profilage légère](#lwp) est activée, elle est exploitée par les statistiques des requêtes dynamiques au lieu du profilage standard lorsqu’elle est affichée par le biais du [moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md) ou que la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) est directement interrogée. 

Les méthodes suivantes de collecte globale d’informations sur les plans d’exécution pour **toutes les sessions** tirent parti de l’infrastructure de profilage standard :

-  L’événement étendu ***query_post_execution_showplan***. Pour activer les événements étendus, consultez [Monitor System Activity Using Extended Events](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
- L’événement de trace **Showplan XML** dans [Trace SQL](../../relational-databases/sql-trace/sql-trace.md) et [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md). Pour plus d’informations sur cet événement de trace, consultez [Classe d’événements Showplan XML](../../relational-databases/event-classes/showplan-xml-event-class.md).

Lors de l’exécution d’une session d’événements étendus qui utilise l’événement *query_post_execution_showplan*, la vue de gestion dynamique [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) est également renseignée, ce qui active les statistiques des requêtes actives pour tous les sessions, à l’aide du [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md) ou par interrogation directe de la vue de gestion dynamique. Pour plus d’informations, voir [Live Query Statistics](../../relational-databases/performance/live-query-statistics.md).

## <a name="the-lightweight-query-execution-statistics-profiling-infrastructure"></a><a name="lwp"></a> Infrastructure légère de profilage des statistiques sur l’exécution des requêtes

À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], une nouvelle *infrastructure légère de profilage des statistiques sur l’exécution des requêtes* (ou **profilage léger**), a été introduite. 

> [!NOTE]
> Les procédures stockées compilées en mode natif ne sont pas prises en charge avec le profilage léger.  

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v1"></a>Infrastructure légère de profilage des statistiques sur l’exécution des requêtes v1

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 à [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]). 
  
À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la surcharge de performances liée à la collecte des informations sur les plans d’exécution a été réduite avec l’introduction du profilage léger. Contrairement au profilage standard, le profilage léger ne collecte pas d’informations sur l’exécution de l’UC. Toutefois, le profilage léger collecte toujours les informations sur le nombre de lignes et l’utilisation des E/S.

Un nouvel événement étendu ***query_thread_profile*** reposant sur le profilage léger a également été introduit. Cet événement étendu expose les statistiques d’exécution par opérateur, ce qui permet de bénéficier d’insights supplémentaires sur les performances de chaque nœud et chaque thread. Un exemple de session utilisant cet événement étendu peut être configuré comme dans l’exemple ci-dessous :

```sql
CREATE EVENT SESSION [NodePerfStats] ON SERVER
ADD EVENT sqlserver.query_thread_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

> [!NOTE]
> Pour plus d’informations sur la surcharge de performances liée au profilage de requête, consultez le billet de blog [Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere). 

Lors de l’exécution d’une session d’événements étendus qui utilise l’événement *query_thread_profile*, la vue de gestion dynamique [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) est également renseignée à l’aide du profilage léger, ce qui active les statistiques des requêtes actives pour tous les sessions, à l’aide du [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md) ou par interrogation directe de la vue de gestion dynamique.

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v2"></a>Infrastructure légère de profilage des statistiques sur l’exécution des requêtes v2

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). 

[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 inclut une version révisée du profilage léger avec une surcharge minimale. Vous pouvez aussi activer le profilage léger de manière globale à l’aide de l’[indicateur de trace 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) pour les versions mentionnées ci-dessus dans *S’applique à*. Une nouvelle fonction de gestion dynamique [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) a été introduite afin de retourner le plan d’exécution de requête pour les requêtes en cours.

À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11, si le profilage léger n’est pas activé globalement, le nouvel argument d’[indicateur de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)**QUERY_PLAN_PROFILE** peut être utilisé pour activer le profilage léger au niveau de la requête, pour toute session. Quand une requête contenant ce nouvel indicateur se termine, un nouvel événement étendu **_query_plan_profile_* _ est également généré. Il fournit le code XML d’un plan d’exécution réel semblable à l’événement étendu _query_post_execution_showplan*. 

> [!NOTE]
> L’événement étendu *query_plan_profile* s’appuie également sur le profilage léger même si l’indicateur de requête n’est pas utilisé. 

Un exemple de session avec l’événement étendu *query_plan_profile* peut être configuré comme dans l’exemple ci-dessous :

```sql
CREATE EVENT SESSION [PerfStats_LWP_Plan] ON SERVER
ADD EVENT sqlserver.query_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v3"></a>Infrastructure légère de profilage des statistiques sur l’exécution des requêtes v3

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] incluent une version nouvellement révisée du profilage léger qui collecte des informations sur le nombre de lignes pour toutes les exécutions. Le profilage léger est maintenant activé par défaut sur [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], l’indicateur de trace 7412 n’a pas d’effet. Le profilage léger peut être désactivé au niveau de la base de données à l’aide de la [configuration étendue à la base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LIGHTWEIGHT_QUERY_PROFILING : `ALTER DATABASE SCOPED CONFIGURATION SET LIGHTWEIGHT_QUERY_PROFILING = OFF;`.

Une nouvelle DMF [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md) est introduite pour retourner l’équivalent du dernier plan d’exécution réel connu pour la plupart des requêtes. Elle s’appelle *Dernières statistiques de plan de requête*. Les dernières statistiques de plan de requête peuvent être activées au niveau de la base de données à l’aide de la [configuration étendue à la base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LAST_QUERY_PLAN_STATS : `ALTER DATABASE SCOPED CONFIGURATION SET LAST_QUERY_PLAN_STATS = ON;`.

Un nouvel événement étendu *query_post_execution_plan_profile* collecte l’équivalent d’un plan d’exécution réel basé sur le profilage léger, contrairement à *query_post_execution_showplan* qui utilise le profilage standard. [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] offre également cet événement à compter de CU14. Un exemple de session avec l’événement étendu *query_post_execution_plan_profile* peut être configuré comme dans l’exemple ci-dessous :

```sql
CREATE EVENT SESSION [PerfStats_LWP_All_Plans] ON SERVER
ADD EVENT sqlserver.query_post_execution_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

#### <a name="example-1---extended-event-session-using-standard-profiling"></a>Exemple 1 - Session d’événements étendus utilisant le profilage standard

```sql
CREATE EVENT SESSION [QueryPlanOld] ON SERVER 
ADD EVENT sqlserver.query_post_execution_showplan(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename = N'C:\Temp\QueryPlanStd.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

#### <a name="example-2---extended-event-session-using-lightweight-profiling"></a>Exemple 2 - Session d’événements étendus utilisant le profilage léger

```sql
CREATE EVENT SESSION [QueryPlanLWP] ON SERVER 
ADD EVENT sqlserver.query_post_execution_plan_profile(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename=N'C:\Temp\QueryPlanLWP.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

## <a name="query-profiling-infrastruture-usage-guidance"></a>Conseils d’utilisation de l’infrastructure de profilage de requête
La table suivante récapitule les actions pour activer le profilage standard ou le profilage léger, à la fois globalement (au niveau du serveur) ou dans une seule session. Inclut également la version la plus ancienne pour laquelle l’action est disponible. 

|Étendue|Profilage standard|Profilage léger|
|---------------|---------------|---------------|
|Global|session xEvent avec le XE; `query_post_execution_showplan` commençant par [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Indicateur de trace 7412; commençant par SP1 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|
|Global|Trace SQL et SQL Server Profiler avec l’événement de trace `Showplan XML`; commençant par SQL Server 2000|session xEvent avec le XE; `query_thread_profile` commençant par SP2 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|
|Global|-|Session xEvent avec `query_post_execution_plan_profile` XE ; à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU14 et [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|
|session|Utiliser `SET STATISTICS XML ON`; commençant par SQL Server 2000|Utiliser l’indicateur de requête `QUERY_PLAN_PROFILE` avec une session xEvent avec le XE; `query_plan_profile` commençant par [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11|
|session|Utiliser `SET STATISTICS PROFILE ON`; commençant par SQL Server 2000|-|
|session|Cliquer sur le bouton [Statistiques des requêtes en direct](../../relational-databases/performance/live-query-statistics.md) dans SSMS; commençant par SP2 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|-|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> En raison d’un éventuel AV aléatoire pendant l’exécution d’une procédure stockée de supervision qui référence [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md), vous devez vérifier que le correctif [KB 4078596](https://support.microsoft.com/help/4078596) est installé dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

À compter du profilage léger v2 et de sa faible surcharge, tout serveur qui n’est pas encore lié à l’UC peut exécuter le profilage léger **de manière continue**, ce qui permet aux spécialistes des bases de données d’explorer toute exécution en cours à tout moment, par exemple à l’aide du Moniteur d’activité ou en interrogeant directement `sys.dm_exec_query_profiles`, et d’obtenir le plan de requête avec les statistiques d’exécution.

Pour plus d’informations sur la surcharge de performances liée au profilage de requête, consultez le billet de blog [Developers Choice: Query progress - anytime, anywhere](https://techcommunity.microsoft.com/t5/SQL-Server/Developers-Choice-Query-progress-anytime-anywhere/ba-p/385004). 

> [!NOTE]
> Les événements étendus avec profilage léger utilisent les informations du profilage standard quand l’infrastructure de celui-ci est déjà activée. Par exemple, une session d’événements étendus utilisant `query_post_execution_showplan` est exécutée, et une autre session utilisant `query_post_execution_plan_profile` est démarrée. La deuxième session continuera d’utiliser les informations du profilage standard.

> [!NOTE]
> Sur [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], le profilage léger est désactivé par défaut, mais il est activé quand une trace XEvent basée sur `query_post_execution_plan_profile` est démarrée, puis est de nouveau désactivée quand la trace est arrêtée. Par conséquent, si les suivis XEvent basés sur `query_post_execution_plan_profile` sont souvent démarrés et arrêtés sur une instance de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], il est vivement recommandé d’activer le profilage léger au niveau global avec l’indicateur de trace 7412 pour éviter la surcharge due à des activations/désactivations répétées. 

## <a name="see-also"></a>Voir aussi  
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Ouvrir le Moniteur d’activité &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Superviser l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)      
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
 [Plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md)    
 [Statistiques des requêtes dynamiques](../../relational-databases/performance/live-query-statistics.md)
---
title: ALTER DATABASE SCOPED CONFIGURATION
description: Activez plusieurs paramètres de configuration de base de données au niveau de chaque base de données.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- ALTER_DATABASE_SCOPED_CONFIGURATION
- ALTER_DATABASE_SCOPED_CONFIGURATION_TSQL
- DATABASE_SCOPED_CONFIGURATION_TSQL
- SCOPED_CONFIGURATION_TSQL
- SCOPED_TSQL
- ALTER_DATABASE_SCOPED_TSQL
- DATABASE_SCOPED_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CONFIGURATION statement
- configuration [SQL Server], ALTER DATABASE SCOPED CONFIGURATION statement
ms.assetid: 63373c2f-9a0b-431b-b9d2-6fa35641571a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||=azure-sqldw-latest
ms.openlocfilehash: 8d314e90bbe869dec9ccdc31c45996c4834fc829
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170421"
---
# <a name="alter-database-scoped-configuration-transact-sql"></a>ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Cette commande active plusieurs paramètres de configuration de base de données au niveau de la **base de données individuelle**. 

Les paramètres suivants sont pris en charge dans [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] et dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme indiqué par la ligne **S’APPLIQUE À** de chaque paramètre de la section [Arguments](#arguments) : 

- Effacer le cache de procédures.
- Affecter au paramètre MAXDOP une valeur arbitraire (1, 2, etc.) adaptée à la base de données primaire et affecter une autre valeur (telle que 0) à toutes les bases de données secondaires utilisées (par exemple, pour les requêtes de rapport).
- Définir le modèle d’estimation de la cardinalité de l’optimiseur de requête indépendant de la base de données au niveau de compatibilité.
- Activer ou désactiver la détection de paramètres au niveau de la base de données.
- Activer ou désactiver les correctifs d’optimisation des requêtes au niveau de la base de données.
- Activer ou désactiver le cache d’identité au niveau de la base de données
- Activer ou désactiver un stub de plan compilé à stocker dans le cache lorsqu’un lot est compilé pour la première fois
- Activer ou désactiver la collecte de statistiques d’exécution pour les modules T-SQL compilés en mode natif.
- Activer ou désactiver les options par défaut « online » pour les instructions DDL qui prennent en charge la syntaxe `ONLINE =`.
- Activer ou désactiver les options par défaut « resumable » pour les instructions DDL qui prennent en charge la syntaxe `RESUMABLE =`.
- Activer ou désactiver les fonctionnalités de [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md).
- Activer ou désactiver le forçage du plan accéléré.
- Activer ou désactiver la fonctionnalité de suppression automatique des tables temporaires globales.
- Activer ou désactiver l’[infrastructure de profilage de requête léger](../../relational-databases/performance/query-profiling-infrastructure.md).
- Activer ou désactiver le nouveau message d’erreur `String or binary data would be truncated`.
- Active ou désactive la collection du dernier plan d’exécution actuel dans [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).
- Spécifiez la durée, en minutes, pendant laquelle une opération d’index reprenable peut être mise en pause avant d’être automatiquement abandonnée par le moteur SQL Server.
- Activer ou désactiver l’attente des verrous en priorité basse pour la mise à jour asynchrone des statistiques

Ce paramètre n’est disponible que dans Azure Synapse Analytics.
- Définir le niveau de compatibilité d’une base de données utilisateur

![Icône de lien](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

ALTER DATABASE SCOPED CONFIGURATION
{
    { [ FOR SECONDARY] SET <set_options>}
}
| CLEAR PROCEDURE_CACHE [plan_handle]
| SET < set_options >
[;]

< set_options > ::=
{
    MAXDOP = { <value> | PRIMARY}
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
    | INTERLEAVED_EXECUTION_TVF = { ON | OFF }
    | BATCH_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ADAPTIVE_JOINS = { ON | OFF }
    | TSQL_SCALAR_UDF_INLINING = { ON | OFF }
    | ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | ELEVATE_RESUMABLE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | OPTIMIZE_FOR_AD_HOC_WORKLOADS = { ON | OFF }
    | XTP_PROCEDURE_EXECUTION_STATISTICS = { ON | OFF }
    | XTP_QUERY_EXECUTION_STATISTICS = { ON | OFF }
    | ROW_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ON_ROWSTORE = { ON | OFF }
    | DEFERRED_COMPILATION_TV = { ON | OFF }
    | ACCELERATED_PLAN_FORCING = { ON | OFF }
    | GLOBAL_TEMPORARY_TABLE_AUTO_DROP = { ON | OFF }
    | LIGHTWEIGHT_QUERY_PROFILING = { ON | OFF }
    | VERBOSE_TRUNCATION_WARNINGS = { ON | OFF }
    | LAST_QUERY_PLAN_STATS = { ON | OFF }
    | PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = <time>
    | ISOLATE_SECURITY_POLICY_CARDINALITY  = { ON | OFF }
    | ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY = { ON | OFF }
}
```

> [!IMPORTANT]
> À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], certains noms d’options ont été changés :      
> -  `DISABLE_INTERLEAVED_EXECUTION_TVF` est devenu `INTERLEAVED_EXECUTION_TVF`
> -  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` est devenu `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
> -  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` est devenu `BATCH_MODE_ADAPTIVE_JOINS`

```SQL
-- Syntax for Azure Synapse Analytics

ALTER DATABASE SCOPED CONFIGURATION
{
    SET <set_options>
}
[;]

< set_options > ::=
{
    DW_COMPATIBILITY_LEVEL = { AUTO | 10 | 20 } 
}
```

## <a name="arguments"></a>Arguments

FOR SECONDARY

Spécifie les paramètres des bases de données secondaires (toutes les bases de données secondaires doivent avoir des valeurs identiques).

CLEAR PROCEDURE_CACHE [plan_handle]

Efface le cache (du plan) de procédure pour la base de données et peut être exécuté sur les bases de données primaires et secondaires.

Spécifiez un descripteur de plan de requête pour effacer un seul plan de requête du cache de plan.

**S’APPLIQUE À** : La spécification d’un descripteur de plan de requête est disponible dans les bases de données SQL Azure et SQL Server 2019 ou ultérieures.

MAXDOP **=** {\<value> | PRIMARY } **\<value>**

Spécifiez le **degré maximal de parallélisme (MAXDOP)** par défaut qui doit être utilisé pour les instructions. 0 est la valeur par défaut, et indique que la configuration du serveur doit être utilisée. Le paramètre MAXDOP défini au niveau de la base de données remplace (sauf si sa valeur est 0) le paramètre **max degree of parallelism** défini au niveau du serveur par sp_configure. Les indicateurs de requête peuvent tout de même remplacer le paramètre MAXDOP défini au niveau de la base de données afin de configurer les requêtes qui nécessitent un paramétrage différent. Tous ces paramètres sont limités par le degré maximal de parallélisme (MAXDOP) défini pour le [groupe de charge de travail](create-workload-group-transact-sql.md).

Vous pouvez utiliser l'option MAXDOP pour limiter le nombre de processeurs à utiliser dans une exécution de plans parallèles. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en compte les plans d’exécution parallèle pour les requêtes, les opérations du langage de définition de données (DDL) d’index, l’insertion parallèle, la modification de colonne en ligne, la collecte de statistiques parallèle et l’alimentation des curseurs statiques et de jeux de clés.

> [!NOTE]
> La limite du **degré maximal de parallélisme (MAXDOP)**  est spécifiée par [tâche](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Il ne s’agit pas d’une limite par [requête](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). Cela signifie que lors d’une exécution de requête parallèle, une requête unique peut générer plusieurs tâches qui sont affectées à un [planificateur](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Pour plus d’informations, consultez le [Guide de l’architecture des threads et des tâches](../../relational-databases/thread-and-task-architecture-guide.md). 

Pour définir cette option au niveau de l’instance, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

> [!NOTE]
> Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], la configuration de l’étendue de base de données MAXDOP pour les nouvelles bases de données de pools élastiques et uniques est définie sur 8 par défaut. MAXDOP peut être configuré pour chaque base de données, comme décrit dans cet article. Pour obtenir des recommandations sur la configuration optimale de MAXDOP, consultez la section [Ressources supplémentaires](#additional-resources).

> [!TIP]
> Pour définir cette option au niveau de la requête, utilisez l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**.    
> Pour ce faire au niveau du serveur, utilisez l’[option de configuration serveur](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) du **degré maximal de parallélisme (MAXDOP)** .     
> Pour ce faire au niveau de la charge de travail, utilisez l’[option de configuration de groupe de charge de travail Resource Governor](../../t-sql/statements/create-workload-group-transact-sql.md) **MAX_DOP**.    

PRIMARY

Peut uniquement être défini pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que la configuration est celle définie pour la base de données primaire. Si la configuration de la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. **PRIMARY** est le paramètre par défaut des bases de données secondaires.

LEGACY_CARDINALITY_ESTIMATION **=** { ON | **OFF** | PRIMARY }

Permet de définir le modèle d’estimation de la cardinalité de l’optimiseur de requête sur SQL Server 2012 ou antérieur selon le niveau de compatibilité de la base de données. La valeur par défaut est **OFF**, ce qui définit le modèle d’estimation de la cardinalité de l’optimiseur de requête en fonction du niveau de compatibilité de la base de données. Définir LEGACY_CARDINALITY_ESTIMATION sur **ON** équivaut à activer [indicateur de Trace 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Pour définir cette option au niveau de la requête, ajoutez l’[indicateur de requête](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> Avec [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, ajoutez l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT** au lieu de l’indicateur de trace.

PRIMARY

Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que le paramètre du modèle d’estimation de la cardinalité de l’optimiseur de requête de toutes les bases de données secondaires est défini sur la valeur de la base de données primaire. Si la configuration du modèle d’estimation de la cardinalité de l’optimiseur de requête qui est définie dans la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence. **PRIMARY** est le paramètre par défaut des bases de données secondaires.

PARAMETER_SNIFFING **=** { **ON** | OFF | PRIMARY}

Active ou désactive la [détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing). La valeur par défaut est ON. La définition de PARAMETER_SNIFFING sur ON équivaut à activer [l’indicateur de trace 4136](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Pour définir cette option au niveau de la requête, consultez l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **OPTIMIZE FOR UNKNOWN**.
> Avec [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, vous pouvez également utiliser l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT**.

PRIMARY

Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que, sur toutes les bases de données secondaires, ce paramètre est défini sur la valeur de la base de données primaire. Si la configuration de la [détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) est modifiée sur la base de données primaire, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. PRIMARY est le paramètre par défaut des bases de données secondaires.

<a name="qo_hotfixes"></a> QUERY_OPTIMIZER_HOTFIXES **=** { ON | **OFF** | PRIMARY }

Active ou désactive les correctifs logiciels d’optimisation de requête, quel que soit le niveau de compatibilité de la base de données. La valeur par défaut est **OFF**, laquelle désactive les correctifs logiciels d’optimisation des requêtes qui ont été publiés après l’arrivée du plus haut niveau de compatibilité d’une version donnée (post-RTM). L’utilisation de la valeur **ON** équivaut à activer [l’indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

> [!TIP]
> Pour définir cette option au niveau de la requête, ajoutez l’[indicateur de requête](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> Avec [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md#use_hint) USE HINT au lieu de l’indicateur de trace.

PRIMARY

Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que, sur toutes les bases de données secondaires, ce paramètre est défini sur la valeur de la base de données primaire. Si la configuration de la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. PRIMARY est le paramètre par défaut des bases de données secondaires.

IDENTITY_CACHE **=** { **ON** | OFF }

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Active ou désactive le cache d’identité au niveau de la base de données. La valeur par défaut est **ON**. La mise en cache d’identité est utilisée pour améliorer les performances INSERT sur les tables comprenant des colonnes d’identité. Pour éviter les écarts dans les valeurs des colonnes d’identité si un serveur redémarre de façon inattendue ou bascule vers un serveur secondaire, désactivez l’option IDENTITY_CACHE. Cette option est similaire à [l’indicateur de trace 272](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) existant, sauf qu’elle peut être définie au niveau de la base de données et non uniquement au niveau du serveur.

> [!NOTE]
> Cette option peut uniquement être définie sur la valeur PRIMARY. Pour plus d’informations, consultez [Colonnes d’identité](create-table-transact-sql-identity-property.md).

INTERLEAVED_EXECUTION_TVF **=** { **ON** | OFF }

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet d’activer ou de désactiver l’exécution entrelacée pour les fonctions table à instructions multiples dans l’étendue de la base de données ou de l’instruction, tout en maintenant le niveau de compatibilité de base de données 140 et au-delà. L’exécution entrelacée est une fonctionnalité qui fait partie du traitement de requêtes adaptatif dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Pour plus d’informations, consultez [Traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 130, cette configuration étendue à la base de données n’a aucun effet.
>
> Dans SQL Server 2017 (14.x) uniquement, l’option INTERLEAVED_EXECUTION_TVF portait l’ancien nom **DISABLE** _INTERLEAVED_EXECUTION_TVF.

BATCH_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet d’activer ou de désactiver la rétroaction d’allocation de mémoire en mode batch dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 140. La rétroaction d’allocation de mémoire en mode batch est une fonctionnalité qui fait partie du [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md) introduit dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 130, cette configuration étendue à la base de données n’a aucun effet.

BATCH_MODE_ADAPTIVE_JOINS **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet d’activer ou de désactiver les jointures adaptatives en mode batch dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 140. Les jointures adaptatives en mode batch sont une fonctionnalité qui fait partie du [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md) introduit dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 130, cette configuration étendue à la base de données n’a aucun effet.

TSQL_SCALAR_UDF_INLINING **=** { **ON** | OFF }

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Vous permet d’activer ou de désactiver l’incorporation (inlining) des fonctions UDF scalaires T-SQL dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 150. L’incorporation (inlining) des fonctions UDF scalaires T-SQL fait partie de la famille des fonctionnalités de [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 140, cette configuration étendue à la base de données n’a aucun effet.

ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Permet de sélectionner les options destinées à forcer le moteur à élever automatiquement les opérations prises en charge pour une exécution en ligne. La valeur par défaut est OFF, ce qui signifie que les opérations ne sont pas élevées pour une exécution en ligne, sauf si cela est spécifié dans l’instruction. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) reflète la valeur actuelle de ELEVATE_ONLINE. Ces options s’appliquent uniquement aux opérations prises en charge pour une exécution en ligne.

FAIL_UNSUPPORTED

Cette valeur élève toutes les opérations DDL prises en charge pour une exécution en ligne (option ONLINE). Les opérations qui ne prennent pas en charge l’exécution en ligne échouent et génèrent un avertissement.

WHEN_SUPPORTED

Cette valeur élève les opérations qui prennent en charge l’option ONLINE. Les opérations qui ne prennent pas en charge une exécution en ligne sont exécutées en mode hors connexion.

> [!NOTE]
> Vous pouvez remplacer le paramètre par défaut en envoyant une instruction avec l’option ONLINE spécifiée.

ELEVATE_RESUMABLE= { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Permet de sélectionner des options pour forcer le moteur à élever automatiquement les opérations prises en charge pour une exécution pouvant être reprise. La valeur par défaut est OFF, ce qui signifie que les opérations ne sont pas élevées pour une exécution pouvant être reprise, sauf si cela est spécifié dans l’instruction. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) reflète la valeur actuelle de ELEVATE_RESUMABLE. Ces options s’appliquent uniquement aux opérations prises en charge pour une exécution pouvant être reprise.

FAIL_UNSUPPORTED

Cette valeur élève toutes les opérations DDL prises en charge pour une exécution pouvant être reprise (option RESUMABLE). Les opérations qui ne prennent pas en charge l’exécution pouvant être reprise échouent et génèrent un avertissement.

WHEN_SUPPORTED

Cette valeur élève les opérations qui prennent en charge l’option RESUMABLE. Les opérations qui ne prennent pas en charge l’exécution pouvant être reprise sont exécutées en tant que telles.

> [!NOTE]
> Vous pouvez remplacer le paramètre par défaut en envoyant une instruction avec l’option RESUMABLE spécifiée.

OPTIMIZE_FOR_AD_HOC_WORKLOADS **=** { ON | **OFF** }

**S’applique à** : [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]

Active ou désactive un stub de plan compilé à stocker dans le cache lorsqu’un lot est compilé pour la première fois. La valeur par défaut est OFF. Une fois que la configuration étendue à la base de données OPTIMIZE_FOR_AD_HOC_WORKLOADS est activée pour une base de données, un stub de plan compilé est stocké dans le cache lorsqu’un lot est compilé pour la première fois. Les stubs de plan ont un encombrement mémoire moins important que celui des plans compilés complets. Si un lot est compilé ou réexécuté, le stub de plan compilé est supprimé et remplacé par un plan compilé complet.

XTP_PROCEDURE_EXECUTION_STATISTICS **=** { ON | **OFF** }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]

Active ou désactive la collecte de statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md).

Les statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_proc_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).

XTP_QUERY_EXECUTION_STATISTICS **=** { ON | **OFF** }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]

Active ou désactive la collecte de statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) et dans le [magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

Les statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

Pour plus d’informations sur la supervision des performances des modules [!INCLUDE[tsql](../../includes/tsql-md.md)] compilés en mode natif, consultez [Surveillance des performances des procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md).

ROW_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Vous permet d’activer ou de désactiver la rétroaction d’allocation de mémoire en mode ligne dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 150. La rétroaction d’allocation de mémoire en mode ligne est une fonctionnalité qui fait partie du [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md) introduit dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (le mode ligne est pris en charge dans [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 140, cette configuration étendue à la base de données n’a aucun effet.

BATCH_MODE_ON_ROWSTORE **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Vous permet d’activer ou de désactiver le mode batch sur rowstore dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 150. Le mode batch sur rowstore est une fonctionnalité qui fait partie de la famille de fonctionnalités de [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 140, cette configuration étendue à la base de données n’a aucun effet.

DEFERRED_COMPILATION_TV **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Vous permet d’activer ou de désactiver la compilation différée de variables de table dans l’étendue de la base de données tout en maintenant le niveau de compatibilité de la base de données à au moins 150. La compilation différée de variables de table est une fonctionnalité qui fait partie de la famille de fonctionnalités de [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Pour le niveau de compatibilité de la base de données inférieur ou égal à 140, cette configuration étendue à la base de données n’a aucun effet.

ACCELERATED_PLAN_FORCING **=** { **ON** | OFF }

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Active un mécanisme optimisé pour le forçage du plan de requête, applicable à toutes les formes de forçage de plan, comme le [plan de forçage du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), l’[optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) ou l’indicateur de requête [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md#use-plan). La valeur par défaut est ON.

> [!NOTE]
> Il n’est pas recommandé de désactiver le forçage du plan accéléré.

GLOBAL_TEMPORARY_TABLE_AUTO_DROP **=** { **ON** | OFF }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Permet de définir la fonctionnalité de suppression automatique pour les [tables temporaires globales](../../t-sql/statements/create-table-transact-sql.md#temporary-tables). La valeur par défaut est ON, ce qui signifie que les tables temporaires globales sont automatiquement supprimées lorsqu’elles ne sont pas utilisées dans une session. Lorsqu’elles sont définies sur OFF, les tables temporaires globales doivent être supprimées explicitement à l’aide d’une instruction DROP TABLE ou sont automatiquement supprimées au redémarrage du serveur.

- Avec les bases de données uniques/pools élastiques [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette option peut être définie dans les bases de données utilisateur individuelles du serveur SQL Database.
- Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Azure SQL Managed Instance, cette option est définie dans `TempDB` et le paramètre des bases de données utilisateur individuelles n’a aucun effet.

<a name="lqp"></a>

LIGHTWEIGHT_QUERY_PROFILING **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet d’activer ou de désactiver l’[infrastructure de profilage de requête léger](../../relational-databases/performance/query-profiling-infrastructure.md). L’infrastructure de profilage de requête léger (LWP) fournit les données de performances de requête plus efficacement que les mécanismes de profilage standard et est activée par défaut.

<a name="verbose-truncation"></a>

VERBOSE_TRUNCATION_WARNINGS **=** { **ON** | OFF}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet d’activer ou de désactiver le nouveau message d’erreur `String or binary data would be truncated`. [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] introduit un nouveau message d’erreur (2628), plus spécifique, dans ce scénario :

`String or binary data would be truncated in table '%.*ls', column '%.*ls'. Truncated value: '%.*ls'.`

Quand sa valeur est définie sur ON pour un niveau de compatibilité de la base de données en dessous de 150, les erreurs de troncation déclenchent le nouveau message d’erreur 2628 pour fournir plus de contexte et simplifier le dépannage.

Quand sa valeur est définie sur OFF pour un niveau de compatibilité de la base de données en dessous de 150, les erreurs de troncation déclenchent l’ancien message d’erreur 8152.

Pour un niveau de compatibilité de la base de données égal ou inférieur à 140, le message d’erreur 2628 reste un message d’erreur d’activation qui nécessite l’activation de l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 460 ; cette configuration au niveau de la de base de données n’a alors aucun effet.

LAST_QUERY_PLAN_STATS **=** { ON | **OFF**}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) (fonctionnalité en préversion publique)

Permet d’activer ou désactiver la collection des statistiques du dernier plan de requête (équivalent à un plan d’exécution réel) dans [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).

PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES

**S’APPLIQUE À** : Azure SQL Database uniquement

L’option `PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES` détermine la durée (en minutes) pendant laquelle l’index reprenable peut être mis en pause avant d’être automatiquement abandonné par le moteur.

- La valeur par défaut est définie sur 1 jour (1 440 minutes).
- La durée minimale est définie sur 1 minute
- La durée maximale est de 71 582 minutes.
- Lorsque la valeur est définie sur 0, les opérations suspendues ne sont jamais abandonnées automatiquement

La valeur actuelle de cette option s’affiche dans [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

ISOLATE_SECURITY_POLICY_CARDINALITY **=** { ON | **OFF**}

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous permet de déterminer si un prédicat de [sécurité au niveau des lignes (RLS)](../../relational-databases/security/row-level-security.md) affecte la cardinalité du plan d’exécution de la requête utilisateur globale. Si ISOLATE_SECURITY_POLICY_CARDINALITY est activé, un prédicat RLS n’affecte pas la cardinalité d’un plan d’exécution. Prenons l’exemple d’une table contenant 1 million de lignes et d’un prédicat RLS qui limite le résultat à 10 lignes pour l’utilisateur qui envoie la requête. Si la configuration étendue à la base de données est désactivée, l’estimation de la cardinalité de ce prédicat sera de 10. Si cette configuration étendue à la base de données est activée, l’optimisation des requêtes est estimée à 1 million de lignes. Il est recommandé d’utiliser la valeur par défaut pour la plupart des charges de travail.

DW_COMPATIBILITY_LEVEL **=** {**AUTO** | 10 | 20 }

**S’APPLIQUE À** : Azure Synapse Analytics uniquement

Définit les comportements de Transact-SQL et du traitement des requêtes pour qu’ils soient compatibles avec la version spécifiée du moteur de base de données.  Une fois qu’il est défini, quand une requête est exécutée sur cette base de données, seules les fonctionnalités compatibles sont mises en œuvre.  Le niveau de compatibilité d’une base de données est défini par défaut sur AUTO lors de sa création.  Le niveau de compatibilité est conservé même après une les opérations d’interruption/reprise et de sauvegarde/restauration de la base de données. 

|Niveau de compatibilité    |   Commentaires|  
|-----------------------|--------------|
|**AUTO**| Par défaut.  Sa valeur est mise à jour automatiquement par le moteur Synapse Analytics.  La valeur actuelle est 20.|
|**10**| Met en œuvre les comportements de Transact-SQL et de traitement des requêtes avant l’introduction de la prise en charge du niveau de compatibilité.|
|**20**| Premier niveau de compatibilité qui inclut les comportements contrôlés de Transact-SQL et de traitement des requêtes. |

ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY **=** { ON | **OFF**}

**S’APPLIQUE À** : Azure SQL Database uniquement (la fonctionnalité est disponible en préversion publique)

Si la mise à jour asynchrone des statistiques est activée, l’activation de cette configuration impose à la demande en arrière-plan mettant à jour les statistiques d’attendre un verrou Sch-M sur une file d’attente à basse priorité, afin d’éviter le blocage des autres sessions dans les scénarios de concurrence élevée. Pour plus d’informations, consultez [AUTO_UPDATE_STATISTICS_ASYNC](../../relational-databases/statistics/statistics.md#auto_update_statistics_async).

## <a name="permissions"></a><a name="Permissions"></a> Autorisations

Nécessite `ALTER ANY DATABASE SCOPED CONFIGURATION` sur la base de données. Cette autorisation peut être accordée par un utilisateur disposant de l’autorisation CONTROL pour une base de données.

## <a name="general-remarks"></a>Remarques d'ordre général

Même si vous pouvez configurer des bases de données secondaires avec des paramètres différents de ceux de la base de données primaire, toutes les bases de données secondaires doivent utiliser la même configuration. Vous ne pouvez pas configurer des paramètres différents pour chaque base de données secondaire.

L’exécution de cette instruction efface le contenu du cache de procédures de la base de données actuelle, ce qui signifie que toutes les requêtes doivent être recompilées.

Pour les requêtes de noms en trois parties, les paramètres de connexion de la base de données actuelle sont honorés (autres que ceux des modules SQL tels que les procédures, les fonctions et les déclencheurs, qui sont compilés dans le contexte d’une autre base de données). Les options de la base de données dans laquelle ils résident sont donc utilisées. De même, lors de la mise à jour asynchrone des statistiques, la configuration d’ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY pour la base de données dans laquelle résident les statistiques est honorée.

L’événement `ALTER_DATABASE_SCOPED_CONFIGURATION` est ajouté en tant qu’événement DDL qui peut être utilisé pour déclencher un déclencheur DDL, et il s’agit d’un enfant du groupe de déclencheurs `ALTER_DATABASE_EVENTS`.

Les paramètres de configuration étendus de la base de données seront reportés avec la base de données, ce qui signifie que lorsqu’une base de données est restaurée ou attachée, les paramètres de configuration existants demeurent.

À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], certains noms d’options ont été changés :      
-  `DISABLE_INTERLEAVED_EXECUTION_TVF` est devenu `INTERLEAVED_EXECUTION_TVF`
-  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` est devenu `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
-  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` est devenu `BATCH_MODE_ADAPTIVE_JOINS`

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

### <a name="maxdop"></a>MAXDOP

Les paramètres granulaires peuvent remplacer les paramètres globaux, et le gouverneur de ressources peut limiter tous les autres paramètres MAXDOP. La logique du paramètre MAXDOP est la suivante :

- L’indicateur de requête remplace `sp_configure` et la configuration étendue à la base de données. Si le groupe de ressources MAXDOP est défini pour le groupe de charge de travail :

  - Si l’indicateur de requête est défini sur zéro (0), il est remplacé par le paramètre Resource Governor.

  - Si l’indicateur de requête n’est pas défini sur zéro (0), il est limité par le paramètre Resource Governor.

- La configuration étendue à la base de données (à moins d’être définie sur 0) remplace le paramètre `sp_configure`, sauf s’il existe un indicateur de requête et qu’il est limité par le paramètre Resource Governor.

- Le paramètre `sp_configure` est remplacé par le paramètre Resource Governor.

### <a name="query_optimizer_hotfixes"></a>QUERY_OPTIMIZER_HOTFIXES

Quand l’indicateur `QUERYTRACEON` est utilisé pour activer l’optimiseur de requête par défaut de SQL Server 7.0 à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou les correctifs logiciels de l’optimiseur de requête, une condition OR lie l’indicateur de requête et le paramètre configuration étendue à la base de données, ce qui signifie que si l’un d’eux est activé, les configurations étendues à la base de données s’appliquent.

### <a name="geo-dr"></a>Reprise d’activité géographique

Les bases de données secondaires accessibles en lecture (par exemple, les groupes de disponibilité AlwaysOn et les bases de données géorépliquées [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]) utilisent la valeur de la base de données secondaire en vérifiant l’état de la base de données. Même si la recompilation ne se produit pas lors du basculement et même si, techniquement, la nouvelle base de données primaire comprend des requêtes qui utilisent les paramètres des bases de données secondaires, l’idée est que le paramètre entre les bases de données primaires et secondaires varient uniquement lorsque la charge de travail est différente, et donc, que les requêtes mises en cache utilisent les paramètres optimaux, tandis que les nouvelles requêtes choisissent les nouveaux paramètres qui leur conviennent.

### <a name="dacfx"></a>DacFx

`ALTER DATABASE SCOPED CONFIGURATION` étant une nouvelle fonctionnalité dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) qui affecte le schéma de base de données, les exportations du schéma (avec ou sans données) ne peuvent pas être importées dans une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telle que [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Par exemple, une exportation vers un [DACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md) ou un [BACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) à partir d’une base de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ayant utilisé cette nouvelle fonctionnalité ne peut pas être importée dans un serveur de niveau inférieur.

### <a name="elevate_online"></a>ELEVATE_ONLINE

Cette option s’applique uniquement aux instructions DDL qui prennent en charge la syntaxe `WITH (ONLINE = <syntax>)`. Les index XML ne sont pas affectés.

### <a name="elevate_resumable"></a>ELEVATE_RESUMABLE

Cette option s’applique uniquement aux instructions DDL qui prennent en charge la syntaxe `WITH (RESUMABLE = <syntax>)`. Les index XML ne sont pas affectés.

## <a name="metadata"></a>Métadonnées

La vue système [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) fournit des informations sur les configurations étendues à une base de données. Les options de configuration définies au niveau de la base de données s’affichent dans sys.database_scoped_configurations parce qu’elles remplacent les paramètres par défaut définis au niveau du serveur. La vue système [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) affiche uniquement les paramètres définis au niveau du serveur.

## <a name="examples"></a>Exemples

Ces exemples illustrent l’utilisation de ALTER DATABASE SCOPED CONFIGURATION.

### <a name="a-grant-permission"></a>R. Accorder l’autorisation

Cet exemple accorde à l’utilisateur Joe l’autorisation nécessaire pour exécuter ALTER DATABASE SCOPED CONFIGURATION.

```sql
GRANT ALTER ANY DATABASE SCOPED CONFIGURATION to [Joe] ;
```

### <a name="b-set-maxdop"></a>B. Définir MAXDOP

Cet exemple définit MAXDOP = 1 pour une base de données primaire et MAXDOP = 4 pour la base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 1 ;
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = 4 ;
```

Cet exemple définit MAXDOP de sorte que sa valeur soit la même pour la base de données primaire et pour la base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY ;
```

### <a name="c-set-legacy_cardinality_estimation"></a>C. Définir LEGACY_CARDINALITY_ESTIMATION

Cet exemple définit LEGACY_CARDINALITY_ESTIMATION sur ON pour une base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = ON ;
```

Cet exemple définit LEGACY_CARDINALITY_ESTIMATION de la même manière pour la base de données primaire et pour une base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = PRIMARY ;
```

### <a name="d-set-parameter_sniffing"></a>D. Définir PARAMETER_SNIFFING

Cet exemple définit PARAMETER_SNIFFING sur OFF pour une base de données primaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING = OFF ;
```

Cet exemple définit PARAMETER_SNIFFING sur OFF pour une base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = OFF ;
```

Cet exemple définit PARAMETER_SNIFFING de la même manière pour la base de données primaire et pour une base de données secondaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = PRIMARY ;
```

### <a name="e-set-query_optimizer_hotfixes"></a>E. Définir QUERY_OPTIMIZER_HOTFIXES

Cet exemple définit QUERY_OPTIMIZER_HOTFIXES sur ON pour la base de données primaire dans un scénario de géoréplication.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES = ON ;
```

### <a name="f-clear-procedure-cache"></a>F. Effacer le contenu du cache de procédures

Cet exemple efface le contenu du cache de procédures (possible uniquement pour la base de données primaire).

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
```

### <a name="g-set-identity_cache"></a>G. Définir IDENTITY_CACHE

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Cet exemple désactive le cache d’identité.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE = OFF ;
```

### <a name="h-set-optimize_for_ad_hoc_workloads"></a>H. Définir OPTIMIZE_FOR_AD_HOC_WORKLOADS

**S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Cet exemple permet à un stub de plan compilé d’être stocké dans le cache lorsqu’un lot est compilé pour la première fois.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET OPTIMIZE_FOR_AD_HOC_WORKLOADS = ON;
```

### <a name="i-set-elevate_online"></a>I. Définir ELEVATE_ONLINE

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Cet exemple définit ELEVATE_ONLINE sur FAIL_UNSUPPORTED.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_ONLINE = FAIL_UNSUPPORTED ;
```

### <a name="j-set-elevate_resumable"></a>J. Définir ELEVATE_RESUMABLE

**S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssNoVersion](../../includes/sssqlv15-md.md)] (fonctionnalité en préversion publique)

Cet exemple affecte la valeur WHEN_SUPPORTED à ELEVEATE_RESUMABLE.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_RESUMABLE = WHEN_SUPPORTED ;
```

### <a name="k-clear-a-query-plan-from-the-plan-cache"></a>K. Effacer un plan de requête du cache du plan

**S’APPLIQUE À** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Cet exemple efface un plan spécifique à du cache de procédure

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE 0x06000500F443610F003B7CD12C02000001000000000000000000000000000000000000000000000000000000;
```

### <a name="l-set-paused-duration"></a>L. Définir la durée de pause

**S’APPLIQUE À** : Azure SQL Database uniquement

Cet exemple définit la durée de pause de l’index reprenable sur 60 minutes.

```sql
ALTER DATABASE SCOPED CONFIGURATION
SET PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = 60
```

## <a name="additional-resources"></a>Ressources supplémentaires

### <a name="maxdop-resources"></a>Ressources MAXDOP

- [Degré de parallélisme](../../relational-databases/query-processing-architecture-guide.md#DOP)
- [Recommandations et directives pour l’option de configuration « max degree of parallelism » dans SQL Server](https://support.microsoft.com/kb/2806535)

### <a name="legacy_cardinality_estimation-resources"></a>Ressources LEGACY_CARDINALITY_ESTIMATION

- [Estimation de la cardinalité (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator (Optimiser vos plans de requêtes avec l’Estimateur de la cardinalité de SQL Server 2014)](/previous-versions/dn673537(v=msdn.10))

### <a name="parameter_sniffing-resources"></a>Ressources PARAMETER_SNIFFING

- [Détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)
- [I smell a parameter!](/archive/blogs/queryoptteam/i-smell-a-parameter)

### <a name="query_optimizer_hotfixes-resources"></a>Ressources QUERY_OPTIMIZER_HOTFIXES

- [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [Modèle de service de l’indicateur de trace 4199 pour les correctifs de l’optimiseur de requête SQL Server](https://support.microsoft.com/kb/974006)

### <a name="elevate_online-resources"></a>Ressources ELEVATE_ONLINE

[Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

### <a name="elevate_resumable-resources"></a>Ressources ELEVATE_RESUMABLE

[Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

## <a name="more-information"></a>Informations complémentaires   
 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)      
 [Recommandations et directives pour l’option de configuration Degré maximal de parallélisme dans SQL Server](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)      
 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)    
 [Affichages catalogue de bases de données et de fichiers](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)    
 [Options de configuration de serveur](../../database-engine/configure-windows/server-configuration-options-sql-server.md)    
 [Fonctionnement des opérations d'index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md)    
 [Exécuter des opérations d’index en ligne](../../relational-databases/indexes/perform-index-operations-online.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)    
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)
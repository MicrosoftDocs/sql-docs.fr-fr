---
title: Options SET d’ALTER DATABASE (Transact-SQL) | Microsoft Docs
description: Découvrez comment définir des options de base de données comme l’optimisation automatique, le chiffrement, le Magasin des requêtes dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 09/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- Automatic tuning
- " Data Retention Policy"
- query plan regression correction
- auto_create_statistics
- auto_update_statistics
- Query Store options
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current
ms.openlocfilehash: 97139ed9331853a5d84d261c69510993fb7231a2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642261"
---
# <a name="alter-database-set-options-transact-sql"></a>Options SET d’ALTER DATABASE (Transact-SQL)

Définit les options de base de données dans Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Pour connaître les autres options d’ALTER DATABASE, voir [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Sélectionnez un des onglets suivants pour connaître la syntaxe, les arguments, les remarques, les autorisations et des exemples propres à la version de SQL que vous utilisez.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!NOTE]
> La définition de certaines options avec ALTER DATABASE peut demander un accès exclusif à la base de données. Si l’instruction ALTER DATABASE ne se termine pas en temps voulu, vérifiez si d’autres sessions dans la base de données bloquent la session ALTER DATABASE.

## <a name="select-a-product"></a>Sélectionner un produit

Dans la ligne suivante, sélectionnez le nom du produit qui vous intéresse. Ceci affiche un contenu différent ici dans cette page web, approprié pour le produit que vous sélectionnez.

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

La mise en miroir de bases de données, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] et les niveaux de compatibilité sont des options `SET` mais sont décrits dans des rubriques distinctes en raison de leur longueur. Pour plus d’informations, consultez [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md), [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) et [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

Les configurations de niveau base de données sont utilisées pour définir plusieurs configurations de base de données au niveau de la base de données individuelle. Pour plus d’informations, consultez [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

> [!NOTE]
> De nombreuses options SET de base de données sont configurables pour la session en cours avec des [Instructions SET](../../t-sql/statements/set-statements-transact-sql.md), et sont souvent configurées par les applications quand elles se connectent. Les options SET de niveau session remplacent les valeurs **ALTER DATABASE SET**. Les options de base de données décrites dans les sections suivantes sont des valeurs que vous pouvez définir pour les sessions qui ne fournissent pas explicitement d’autres valeurs pour les options SET.

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE { database_name | CURRENT }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}

<option_spec> ::=
{
    <accelerated_database_recovery>
  | <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <containment_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <external_access_option>
  | FILESTREAM ( <FILESTREAM_option> )
  | <HADR_options>
  | <mixed_page_allocation_option>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <remote_data_archive_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
}
;

<accelerated_database_recovery> ::=
{
    ACCELERATED_DATABASE_RECOVERY = { ON | OFF }
     [ ( PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name } ) ];
}

<auto_option> ::=
{
    AUTO_CLOSE { ON | OFF }
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
   {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
   }
}

<change_tracking_option_list> ::=
{
   AUTO_CLEANUP = { ON | OFF }
 | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
}

<containment_option> ::=
   CONTAINMENT = { NONE | PARTIAL }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
  | CURSOR_DEFAULT { LOCAL | GLOBAL }
}

<database_mirroring_option>
  ALTER DATABASE Database Mirroring

<date_correlation_optimization_option> ::=
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }
  
<db_encryption_option> ::=
    ENCRYPTION { ON | OFF | SUSPEND | RESUME }

<db_state_option> ::=
    { ONLINE | OFFLINE | EMERGENCY }

<db_update_option> ::=
    { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::=
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<external_access_option> ::=
{
    DB_CHAINING { ON | OFF }
  | TRUSTWORTHY { ON | OFF }
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | NESTED_TRIGGERS = { OFF | ON }
  | TRANSFORM_NOISE_WORDS = { OFF | ON }
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }
}

<FILESTREAM_option> ::=
{
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL
  | DIRECTORY_NAME = <directory_name>
}

<HADR_options> ::=
    ALTER DATABASE SET HADR

<mixed_page_allocation_option> ::=
    MIXED_PAGE_ALLOCATION { OFF | ON }

<parameterization_option> ::=
    PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
    QUERY_STORE
    {
          = OFF [ ( FORCED ) ] 
        | = ON [ ( <query_store_option_list> [,...n] ) ]
        | ( < query_store_option_list> [,...n] )
        | CLEAR [ ALL ]
    }
}

<query_store_option_list> ::=
{
      OPERATION_MODE = { READ_WRITE | READ_ONLY }
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
    | DATA_FLUSH_INTERVAL_SECONDS = number
    | MAX_STORAGE_SIZE_MB = number
    | INTERVAL_LENGTH_MINUTES = number
    | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
    | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
    | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
      STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<recovery_option> ::=
{
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }
  | TORN_PAGE_DETECTION { ON | OFF }
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }
}

<remote_data_archive_option> ::=
{
    REMOTE_DATA_ARCHIVE =
    {
        ON ( SERVER = <server_name> ,
             { 
                  CREDENTIAL = <db_scoped_credential_name>
                  | FEDERATED_SERVICE_ACCOUNT = ON | OFF
             }
        )
        | OFF
    }
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | DISABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
  | HONOR_BROKER_PRIORITY { ON | OFF}
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT { ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = { ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<target_recovery_time_option> ::=
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }

<termination>::=
{
    ROLLBACK AFTER number [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention> ::=
    TEMPORAL_HISTORY_RETENTION { ON | OFF }

<data_retention_policy> ::=
    DATA_RETENTION { ON | OFF }

```

## <a name="arguments"></a>Arguments

*database_name*     
Nom de la base de données à modifier.

CURRENT     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Exécute l’action dans la base de données actuelle. `CURRENT` n’est pas pris en charge pour toutes les options dans tous les contextes. Si `CURRENT` échoue, fournissez le nom de la base de données.

**\<accelerated_database_recovery> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Active la [récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-management.md) pour chaque base de données. ADR est défini par défaut sur OFF dans [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]. À l’aide de cette syntaxe, vous pouvez choisir de désigner un groupe de fichiers spécifique pour les données du magasin de versions persistantes. Si aucun groupe de fichiers n’est spécifié, le magasin de versions persistantes est stocké dans le groupe de fichiers PRIMARY. Pour obtenir des exemples et des informations supplémentaires, consultez [Récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-management.md).

**\<auto_option> ::=**      
Contrôle les options automatiques.

<a name="auto_close"></a> AUTO_CLOSE { ON | **OFF** }     
ACTIVÉ     
La base de données est arrêtée correctement et ses ressources sont libérées dès que le dernier utilisateur l'a quittée.

La base de données est rouverte automatiquement lorsqu'un utilisateur tente de la réutiliser. Par exemple, ce comportement de réouverture se produit quand un utilisateur émet une instruction `USE database_name`. La base de données peut être arrêtée proprement avec AUTO_CLOSE défini sur la valeur ON. Dans ce cas, la base de données n’est pas rouverte jusqu’à ce qu’un utilisateur tente d’utiliser la base de données au redémarrage suivant du [!INCLUDE[ssDE](../../includes/ssde-md.md)].

OFF     
La base de données reste ouverte après que le dernier utilisateur l'a quittée.

L'option AUTO_CLOSE est utile pour les bases de données bureautiques, puisqu'elle permet aux fichiers de base de données d'être gérés comme des fichiers normaux. Ils peuvent être déplacés, copiés pour faire une sauvegarde ou même envoyés par e-mail à d’autres utilisateurs. Le processus AUTO_CLOSE est asynchrone ; l’ouverture et la fermeture répétées de la base de données n’ont aucune incidence sur les performances.

> [!NOTE]
> L’option AUTO_CLOSE n’est pas disponible dans une base de données autonome, ni sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_close_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsAutoClose` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).
>
> Quand AUTO_CLOSE est défini sur ON, certaines colonnes de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) retournent une valeur NULL, car la base de données est indisponible pour l’extraction des données. Pour résoudre ce problème, exécutez une instruction USE pour ouvrir la base de données.
>
> La mise en miroir de bases de données nécessite que AUTO_CLOSE soit défini sur OFF.

Quand la base de données est définie sur `AUTOCLOSE = ON`, une opération qui initie un arrêt de la base de données automatique efface le cache du plan pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2, pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d’information suivant : `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ACTIVÉ     
L’optimiseur de requête crée des statistiques sur les colonnes uniques des prédicats de requête, en fonction des besoins, afin d’améliorer les plans de requête et les performances des requêtes. Ces statistiques de colonnes uniques sont créées quand l’optimiseur de requête compile les requêtes. Les statistiques de colonnes uniques sont créées uniquement sur les colonnes qui ne constituent pas déjà la première colonne d’un objet de statistiques existant.

Le paramètre par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

OFF     
L’optimiseur de requête ne crée pas de statistiques sur les colonnes uniques des prédicats de requête quand il compile les requêtes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_create_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoCreateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).

INCREMENTAL = ON | **OFF**     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Définissez AUTO_CREATE_STATISTICS sur la valeur ON, et INCREMENTAL sur la valeur ON. Ceci définit les statistiques créées automatiquement comme étant incrémentielles, dès lors que celles-ci sont prises en charge. La valeur par défaut est OFF. Pour plus d’informations, voir [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVÉ     
Les fichiers de base de données peuvent faire l'objet d'une réduction périodique.

Les fichiers de données et les fichiers journaux peuvent être réduits automatiquement. AUTO_SHRINK ne réduit la taille du journal des transactions que si vous définissez la base de données sur le mode de récupération SIMPLE ou si vous sauvegardez le journal. Quand vous définissez AUTO_SHRINK sur OFF, les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l’espace inutilisé.

L’option AUTO_SHRINK réduit les fichiers quand ceux-ci contiennent plus de 25 % d’espace inutilisé. Elle réduit le fichier à une des deux tailles suivantes (selon la valeur la plus grande) :

- La taille à laquelle 25 pour cent du fichier est de l’espace inutilisé
- La taille du fichier quand il a été créé

Vous ne pouvez pas réduire une base de données en lecture seule.

OFF     
Les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l'espace inutilisé.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_shrink_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoShrink` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L’option AUTO_SHRINK n’est pas disponible dans une base de données autonome.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVÉ     
Spécifie que l’optimiseur de requête met à jour les statistiques quand elles sont utilisées par une requête et quand elles sont susceptibles d’être obsolètes. Les statistiques deviennent obsolètes après que des opérations d'insertion, de mise à jour, de suppression ou de fusion ont modifié la distribution des données dans la table ou la vue indexée. L’optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.

L’optimiseur de requête vérifie s’il existe des statistiques obsolètes avant de compiler une requête et d’exécuter un plan de requête mis en cache. L’optimiseur de requête utilise les colonnes, les tables et les vues indexées du prédicat de requête pour identifier les statistiques susceptibles d’être obsolètes. L’optimiseur de requête détermine ces informations avant de compiler une requête. Avant d’exécuter un plan de requête mis en cache, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que le plan de requête référence des statistiques à jour.

L'option AUTO_UPDATE_STATISTICS s'applique aux statistiques créées pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l'aide de l'instruction CREATE STATISTICS. Cette option s'applique également aux statistiques filtrées.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

Utilisez l'option AUTO_UPDATE_STATISTICS_ASYNC pour spécifier si les statistiques doivent être mises à jour en mode synchrone ou asynchrone.

OFF     
Spécifie que l’optimiseur de requête ne met pas à jour les statistiques quand elles sont utilisées par une requête. L’optimiseur de requête ne met pas non plus à jour les statistiques quand elles sont susceptibles d’être obsolètes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_update_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoUpdateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVÉ     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont asynchrones. L’optimiseur de requête n’attend pas la fin des mises à jour des statistiques pour compiler les requêtes.

Affecter la valeur ON à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Par défaut, l’option AUTO_UPDATE_STATISTICS_ASYNC est OFF ; l’optimiseur de requête met à jour les statistiques de façon synchrone.

OFF     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont synchrones. L’optimiseur de requête attend la fin des mises à jour des statistiques pour compiler les requêtes.

> [!NOTE]
> Affecter la valeur OFF à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_update_stats_async_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Pour plus d’informations décrivant quand utiliser des mises à jour de statistiques synchrones ou asynchrones, consultez la section « Options des statistiques » dans [Statistiques](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)])

Active ou désactive l’option `FORCE_LAST_GOOD_PLAN` [Optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ACTIVÉ     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] force automatiquement le dernier plan correct connu sur les requêtes [!INCLUDE[tsql-md](../../includes/tsql-md.md)] là où le nouveau plan de requête provoque des régressions des performances. Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervise en permanence les performances de la requête [!INCLUDE[tsql-md](../../includes/tsql-md.md)] avec le plan forcé.

S’il existe des gains de performances, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continue à utiliser le dernier plan correct connu. Si aucun gain de performances n’est détecté, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] génère un nouveau plan de requête. L’instruction échoue si le Magasin des requêtes n’est pas activé ou s’il n’est pas en mode *Lecture-écriture*.

OFF     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] indique les régressions des performances de requêtes potentielles dues à des changements de plan de requête dans la vue [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). Toutefois, ces recommandations ne sont pas appliquées automatiquement. Les utilisateurs peuvent superviser les recommandations actives et résoudre les problèmes identifiés en appliquant les scripts [!INCLUDE[tsql-md](../../includes/tsql-md.md)] qui sont montrés dans la vue. La valeur par défaut est OFF.

**\<change_tracking_option> ::=**      
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSFull](../../includes/sssds-md.md)]

Contrôle les options de suivi des modifications. Vous pouvez activer le suivi des modifications, définir des options, modifier des options et désactiver le suivi des modifications. Pour des exemples, consultez la section « Exemples » plus loin dans cet article.

ACTIVÉ     
Active le suivi des modifications pour la base de données Lorsque vous activez le suivi des modifications, vous pouvez également définir les options AUTO CLEANUP et CHANGE RETENTION.

AUTO_CLEANUP = { ON | **OFF** }     
ACTIVÉ     
Les informations de suivi des modifications sont supprimées automatiquement à l'issue de la période de rétention spécifiée.

OFF     
Les données de suivi des modifications ne sont pas supprimées automatiquement de la base de données.

CHANGE_RETENTION = *période_conservation* { **DAYS** | HOURS | MINUTES }     
Spécifie la période minimale de conservation des informations de suivi des modifications dans la base de données. Les données sont supprimées uniquement lorsque AUTO_CLEANUP a la valeur ON.

*retention_period* est un entier qui spécifie la composante numérique de la période de rétention.

La période de conservation par défaut est **2 jours**. La période de rétention minimale est 1 minute. Le type de conservation par défaut est **DAYS**.

OFF     
Désactive le suivi des modifications pour la base de données. Désactivez le suivi des modifications sur toutes les tables avant de le désactiver sur la base de données.

**\<containment_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Contrôle des options de la relation contenant-contenu de la base de données.

CONTAINMENT = { **NONE** | PARTIAL}     
Aucune     
La base de données n’est pas une base de données autonome.

PARTIAL     
La base de données est une base de données autonome. La définition de la relation contenant-contenu de base de données sur la valeur partielle échouera si l'option de réplication, de capture des données modifiées ou de suivi des modifications est activée. La vérification des erreurs prend fin après un échec. Pour plus d'informations sur les bases de données autonomes, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md).

**\<cursor_option> ::=**     

Contrôle les options de curseur.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVÉ     
Les curseurs ouverts quand vous validez ou restaurez une transaction sont fermés.

OFF     
Les curseurs restent ouverts lorsqu'une transaction est validée. La restauration d'une transaction ferme tous les curseurs à l'exception de ceux définis avec la valeur INSENSITIVE ou STATIC.

Les paramètres de niveau connexion définis à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de CURSOR_CLOSE_ON_COMMIT. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui désactive l’option CURSOR_CLOSE_ON_COMMIT pour la session (valeur OFF) par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_cursor_close_on_commit_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsCloseCursorsOnCommitEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

CURSOR_DEFAULT { LOCAL | GLOBAL }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Détermine si l'étendue du curseur utilise LOCAL ou GLOBAL.

LOCAL     
Quand vous spécifiez LOCAL et que vous ne définissez pas de curseur comme GLOBAL lors de la création du curseur, le curseur est d’étendue locale. Plus précisément, l’étendue du curseur est locale pour le lot, la procédure stockée ou le déclencheur dans lequel vous avez créé le curseur. Le nom du curseur n'est valide que dans cette étendue.

Le curseur peut être référencé par des variables de curseur locales dans le traitement, la procédure stockée ou le déclencheur, ou bien par un paramètre OUTPUT d'une procédure stockée. Le curseur est libéré implicitement à la fin du lot, de la procédure stockée ou du déclencheur. Le curseur est libéré à moins d’avoir été retourné dans un paramètre OUTPUT. Le curseur peut avoir été retourné dans un paramètre OUTPUT. Si le curseur est retourné de cette manière, il est libéré lorsque la dernière variable qui fait référence au curseur est libérée ou est hors de portée.

GLOBAL     
Si GLOBAL est spécifié et qu’aucun curseur n’est défini comme LOCAL lors de sa création, le curseur est d’étendue globale pour la connexion. Toute procédure stockée ou tout lot exécuté par la connexion peut faire référence au nom du curseur.

Le curseur n'est libéré implicitement qu'au moment de la déconnexion. Pour plus d’informations, voir [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_local_cursor_default` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsLocalCursorsDefault` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<data_retention_policy> ::=**

**S’applique à** : Azure SQL Edge *uniquement*

DATA_RETENTION { ON | OFF }   
ACTIVÉ    
Active le nettoyage basé sur la stratégie de rétention des données sur une base de données.

OFF   
Désactive le nettoyage basé sur la stratégie de rétention des données sur une base de données.

**\<database_mirroring>**     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Pour une description des arguments, voir [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md).

**\<date_correlation_optimization_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Contrôle l'option date_correlation_optimization.

DATE_CORRELATION_OPTIMIZATION { ON | **OFF** }     
ACTIVÉ     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve les statistiques de corrélation dans laquelle une contrainte FOREIGN KEY lie deux tables quelconques de la base de données et où les tables ont des colonnes **datetime**.

OFF     
Les statistiques de corrélation ne sont pas conservées.

Pour pouvoir définir DATE_CORRELATION_OPTIMIZATION sur ON, il ne doit exister aucune connexion active à la base de données, à l’exception de celle qui exécute l’instruction ALTER DATABASE. Ensuite, différentes connexions peuvent être prises en charge.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_date_correlation_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<db_encryption_option> ::=**      
Contrôle l'état de chiffrement de la base de données.

ENCRYPTION { ON \| **OFF** \| SUSPEND \| RESUME }     
ACTIVÉ     
Indique que la base de données doit être chiffrée.

OFF     
Indique que la base de données ne doit pas être chiffrée.

SUSPEND     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])     

Peut être utilisée pour suspendre l’analyse du chiffrement après l’activation ou la désactivation du chiffrement TDE, ou après un changement de la clé de chiffrement.

RESUME     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Peut être utilisée pour reprendre une analyse du chiffrement.

Pour plus d’informations sur le chiffrement de base de données, voir [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) et [Transparent Data Encryption avec Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quand le chiffrement est activé au niveau de la base de données, tous les groupes de fichiers sont chiffrés. Tous les nouveaux groupes de fichiers héritent de la propriété chiffrée. Si des groupes de fichiers dans la base de données sont définis sur READ ONLY, l’opération de chiffrement de la base de données échoue.

Vous pouvez voir l’état du chiffrement de la base de données et l’état de l’analyse du chiffrement en utilisant la vue de gestion dynamique [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_state_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Contrôle l'état de la base de données.

OFFLINE     
La base de données est fermée et arrêtée correctement, puis marquée comme étant déconnectée. Tant que la base de données est hors connexion, elle ne peut pas être modifiée.

ONLINE     
La base de données est ouverte et peut être utilisée.

EMERGENCY     
La base de données est marquée READ_ONLY, la journalisation est désactivée et l'accès est restreint aux membres du rôle serveur fixe sysadmin. EMERGENCY est principalement utilisé à des fins de dépannage. Par exemple, une base de données marquée comme suspecte en raison d’un fichier journal corrompu peut se voir affecter l’état EMERGENCY. Ce paramètre peut permettre à l’administrateur système d’accéder en lecture seule à la base de données. Seuls les membres du rôle serveur fixe sysadmin peuvent définir l'état EMERGENCY pour une base de données.

Nécessite l’autorisation `ALTER DATABASE` pour la base de données concernée afin de changer l’état d’une base de données en hors connexion ou en urgence, et l’autorisation `ALTER ANY DATABASE` au niveau du serveur pour faire passer une base de données de hors connexion à en ligne.

Vous pouvez déterminer l’état de cette option en consultant les colonnes `state` et `state_desc` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `Status` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Pour plus d'informations, consultez [Database States](../../relational-databases/databases/database-states.md).

Une base de données marquée RESTORING ne peut pas se voir affecter la valeur OFFLINE, ONLINE ou EMERGENCY. Une base de données peut être à l'état RESTORING durant une opération de restauration active, ou lorsqu'une opération de restauration d'un fichier de base de données ou d'un fichier journal échoue car un fichier de sauvegarde est corrompu.

**\<db_update_option> ::=**

Contrôle si des mises à jour sont autorisées dans la base de données.

READ_ONLY     
Les utilisateurs peuvent lire des données dans la base de données mais ils n'ont pas le droit de les modifier.

> [!NOTE]
> Pour améliorer les performances des requêtes, mettez à jour les statistiques avant de définir une base de données à READ_ONLY. Si des statistiques supplémentaires sont nécessaires après qu'une base de données est définie à READ_ONLY, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des statistiques dans tempdb. Pour plus d’informations sur les statistiques pour une base de données en lecture seule, consultez [Statistiques](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de données est accessible aux opérations de lecture et d'écriture.

Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

> [!NOTE]
> `SET { READ_ONLY | READ_WRITE }` est désactivé sur les bases de données fédérées [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)].

**\<db_user_access_option> ::=**      
Contrôle l'accès utilisateur à la base de données.

SINGLE_USER     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Indique que l'accès à la base de données n'est autorisé qu'à un seul utilisateur à la fois. Si vous spécifiez SINGLE_USER et que d’autres utilisateurs se connectent à la base de données, l’instruction ALTER DATABASE est bloquée jusqu’à ce que tous les autres utilisateurs se déconnectent de la base de données spécifiée. Pour remplacer ce comportement, examinez la clause WITH \<termination>.

La base de données demeure en mode SINGLE_USER même si l'utilisateur qui a défini l'option se déconnecte. À ce stade, un autre utilisateur (et un seul) peut se connecter à la base de données.

Avant d'affecter la valeur SINGLE_USER à la base de données, vérifiez que l'option AUTO_UPDATE_STATISTICS_ASYNC a la valeur OFF. Si la valeur spécifiée est ON, le thread d’arrière-plan utilisé pour mettre à jour les statistiques se connecte à la base de données et vous ne pourrez pas accéder à celle-ci en mode mono-utilisateur. Pour consulter l’état de cette option, interrogez la colonne `is_auto_update_stats_async_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Si l'option a la valeur ON, effectuez les tâches suivantes :

1. Affectez la valeur OFF à AUTO_UPDATE_STATISTICS_ASYNC.

2. Recherchez les travaux des statistiques asynchrones actifs en interrogeant la vue de gestion dynamique [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md).

Si des travaux sont actifs, laissez ces travaux se terminer ou arrêtez-les manuellement à l’aide de [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md).

RESTRICTED_USER     
Autorise uniquement les membres du rôle de base de données fixe `db_owner` et des rôles serveur fixes `dbcreator` et `sysadmin` à se connecter à la base de données. RESTRICTED_USER n’en limite pas le nombre. Fermez toutes les connexions à la base de données dans la plage de temps spécifiée par la clause d’arrêt de l’instruction ALTER DATABASE. Après que la base est passée à l'état RESTRICTED_USER, toute tentative de connexion par des utilisateurs non qualifiés est refusée.

MULTI_USER     
Tous les utilisateurs qui bénéficient des autorisations appropriées peuvent se connecter à la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `user_access` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `UserAccess` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

Contrôle si les transactions sont validées de manière entièrement durable ou durable différée.

DISABLED     
Toutes les transactions suivant `SET DISABLED` sont entièrement durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

ALLOWED     
Toutes les transactions suivant `SET ALLOWED` sont soit entièrement durables, soit retardées durables, en fonction de l’option de durabilité définie dans l’instruction de validation ou de bloc atomique.

FORCED     
Toutes les transactions suivant `SET FORCED` sont retardées durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

**\<external_access_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Contrôle si des ressources externes, par exemple des objets d'une autre base de données, peuvent accéder à la base de données.

DB_CHAINING { ON | **OFF** }     
ACTIVÉ     
La base de données peut être la source ou la cible d'une chaîne de propriétés des bases de données croisées.

OFF     
La base de données ne peut pas prendre part à un chaînage des propriétés des bases de données croisées.

> [!IMPORTANT]
> L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconnaît ce paramètre lorsque l'option de serveur cross db ownership chaining a la valeur 0 (OFF). Lorsque cross db ownership chaining a la valeur 1 (ON), toutes les bases de données utilisateur peuvent participer aux chaînages des propriétés des bases de données croisées, quelle que soit la valeur de cette option. Cette option est configurée à l’aide de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

Pour définir cette option, l'autorisation `CONTROL SERVER` est nécessaire sur la base de données.

L’option DB_CHAINING ne peut pas être définie sur les bases de données système master, model et tempdb.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_db_chaining_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

TRUSTWORTHY { ON | **OFF** }     
ACTIVÉ     
Les modules de base de données (par exemple, les procédures stockées ou les fonctions définies par l'utilisateur) qui utilisent un contexte d'emprunt d'identité peuvent accéder à des ressources en dehors de la base de données.

OFF     
Les modules de base de données qui utilisent l’emprunt d’identité ne peuvent pas accéder à des ressources externes à la base de données.

TRUSTWORTHY prend la valeur OFF chaque fois que la base de données est attachée.

Par défaut, pour toutes les bases de données système, sauf pour la base msdb, l'option TRUSTWORTHY est définie à OFF (désactivé). La valeur ne peut pas être modifiée pour les bases de données model et tempdb. Nous vous recommandons de ne jamais définir l'option TRUSTWORTHY à ON (activé) pour la base de données master.

Pour définir cette option, l'autorisation `CONTROL SERVER` est nécessaire sur la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_trustworthy_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

DEFAULT_FULLTEXT_LANGUAGE     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Spécifie la valeur de langue par défaut pour les colonnes indexées de texte intégral.

> [!IMPORTANT]
> Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

DEFAULT_LANGUAGE     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Spécifie la langue par défaut de toutes les nouvelles connexions. La langue peut être spécifiée en indiquant l’ID local (lcid), son nom ou son alias. Pour connaître la liste des noms et des alias de langue acceptables, voir [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

NESTED_TRIGGERS     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Spécifie si un déclencheur AFTER peut s'exécuter en cascade et, par conséquent, réaliser une action qui initialise un autre déclencheur, lequel initialise un autre déclencheur, etc. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

TRANSFORM_NOISE_WORDS     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Utilisé pour supprimer un message d'erreur si des mots parasites ou des mots vides provoquent l'échec d'une opération booléenne sur une requête de texte intégral. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

TWO_DIGIT_YEAR_CUTOFF     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Spécifie un entier compris entre 1 753 et 9 999 qui représente l'année de coupure permettant d'interpréter les années à deux chiffres comme des années à quatre chiffres. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

**\<FILESTREAM_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Contrôle les paramètres des FileTables.

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }     
OFF     
L'accès non transactionnel aux données FileTable est désactivé.

READ_ONLY     
Les données FILESTREAM dans les FileTables de cette base de données peuvent être lues par des processus non transactionnels.

FULL     
Active l’accès non transactionnel complet aux données FILESTREAM dans les FileTables.

DIRECTORY_NAME = *\<directory_name>*      
Nom de répertoire compatible avec Windows. Ce nom doit être unique parmi tous les noms de répertoire au niveau de la base de données dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement. Cette option doit être définie avant de créer un FileTable dans cette base de données.

**\<HADR_options> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Voir [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md).

**\<mixed_page_allocation_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])

Contrôle si la base de données peut créer des pages initiales à l’aide d’une extension mixte pour les huit premières pages d’une table ou d’un index.

MIXED_PAGE_ALLOCATION { OFF | **ON** }     
OFF     
La base de données crée toujours les pages initiales à l’aide d’extensions uniformes. OFF est la valeur par défaut.

ACTIVÉ     
La base de données peut créer des pages initiales à l’aide d’extensions mixtes.

Ce paramètre est ON pour toutes les bases de données système. **tempdb** est la seule base de données système qui prend en charge la valeur OFF.

**\<PARAMETERIZATION_option> ::=**      
Contrôle l'option de paramétrage. Pour plus d’informations sur le paramétrage, consultez [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Les requêtes sont paramétrables en fonction du comportement par défaut de la base de données.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paramètre toutes les requêtes de la base de données.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_parameterization_forced column` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])

ON | **OFF** [ ( FORCED )  ] | CLEAR [ ALL ]     
Contrôle si le Magasin des requêtes est activé dans cette base de données et contrôle la suppression du contenu du Magasin des requêtes. Pour plus d’informations, consultez [Scénarios d’utilisation du magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md).

ACTIVÉ     
Active le magasin des requêtes.

OFF [ ( FORCED ) ]      
Désactive le magasin des requêtes. OFF est la valeur par défaut. FORCED est facultatif. FORCED abandonne toutes les tâches en arrière-plan du Magasin des requêtes en cours d’exécution, et ignore le vidage synchrone lorsque le Magasin des requêtes est désactivé. Entraîne l’arrêt du Magasin des requêtes aussi vite que possible. FORCED s’applique à [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 CU14, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU21, [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CU6 et aux builds ultérieures.

> [!NOTE]  
> Le Magasin des requêtes ne peut pas être désactivé dans la base de données unique [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et le pool élastique. L’exécution de `ALTER DATABASE [database] SET QUERY_STORE = OFF` renverra l’avertissement `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR [ ALL ]     
Supprime les données relatives aux requêtes du Magasin des requêtes. ALL est facultatif. ALL supprime les données et métadonnées relatives aux requêtes du Magasin des requêtes.

OPERATION_MODE { READ_ONLY | READ_WRITE }     
Décrit le mode de fonctionnement du magasin des requêtes.

READ_WRITE     
Le magasin des requêtes collecte et conserve les informations sur les plans de requête et les statistiques d’exécution.

READ_ONLY     
Les informations peuvent être lues à partir du Magasin des requêtes, mais les nouvelles informations ne sont pas ajoutées. Si l’espace maximal alloué au magasin des requêtes est atteinte, le mode d’opération du magasin des requêtes passe en READ_ONLY.

CLEANUP_POLICY     
Décrit la stratégie de conservation des données du magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS détermine le nombre de jours pendant lesquels les informations d’une requête sont conservées dans le magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS est de type **bigint**. La valeur par défaut est 30.

DATA_FLUSH_INTERVAL_SECONDS     
Détermine la fréquence à laquelle les données écrites dans le magasin des requêtes sont stockées sur le disque. Pour optimiser les performances, les données collectées par le magasin des requêtes sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée à l'aide de l'argument DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS est de type **bigint**. La valeur par défaut est **900** (15 minutes).

MAX_STORAGE_SIZE_MB     
Détermine l’espace alloué au magasin des requêtes. MAX_STORAGE_SIZE_MB est de type **bigint**. La valeur par défaut est de **100 Mo** pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)](de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], la valeur par défaut est de **1 Go**.

> [!NOTE]
> La limite `MAX_STORAGE_SIZE_MB` n’est pas strictement appliquée. La taille de stockage est vérifiée seulement quand le Magasin des requêtes écrit des données sur le disque. Cet intervalle est défini par l’option `DATA_FLUSH_INTERVAL_SECONDS` ou l’option de la boîte de dialogue Magasin des requêtes [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] **Intervalle de vidage des données**. La valeur par défaut de l’intervalle est 900 secondes (ou 15 minutes).
> Si le Magasin des requêtes a enfreint la limite `MAX_STORAGE_SIZE_MB` entre des vérifications de la taille de stockage, il passe en mode lecture seule. Si l’option `SIZE_BASED_CLEANUP_MODE` est activée, le mécanisme de nettoyage permettant d’appliquer la limite `MAX_STORAGE_SIZE_MB` est également déclenché.
> Une fois que suffisamment d’espace a été libéré, le mode Magasin des requêtes revient automatiquement en lecture-écriture.

> [!IMPORTANT]
> Si vous pensez que la capture de votre charge de travail nécessite plus de 10 Go d’espace disque, vous devez probablement repenser et optimiser votre charge de travail pour réutiliser les plans de requête (par exemple, en utilisant le [paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) ou ajuster les configurations du Magasin des requêtes.    
> À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez définir `QUERY_CAPTURE_MODE` sur CUSTOM pour disposer d’un contrôle supplémentaire sur la stratégie de capture des requêtes.

INTERVAL_LENGTH_MINUTES     
Détermine l’intervalle de temps auquel les données des statistiques d’exécution du runtime sont agrégées dans le magasin des requêtes. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Cette fenêtre de temps fixe est configurée à l'aide de l'argument INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES est de type **bigint**. La valeur par défaut est **60**.

SIZE_BASED_CLEANUP_MODE { **AUTO** | OFF }     
Contrôle si le nettoyage s’active automatiquement quand la quantité totale de données approche de la taille maximale.

AUTO     
Le nettoyage basé sur la taille est activé automatiquement quand la taille sur le disque atteint 90 % de **MAX_STORAGE_SIZE_MB**. Le nettoyage basé sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête à environ 80 % de **MAX_STORAGE_SIZE_MB**. Il s’agit de la valeur de configuration par défaut.

OFF     
Le nettoyage basé sur la taille n’est pas activé automatiquement.

SIZE_BASED_CLEANUP_MODE est de type **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Désigne le mode de capture de requête actif actuellement. Chaque mode définit des stratégies de capture de requête spécifiques.

> [!NOTE]
> Les curseurs, les requêtes dans les procédures stockées et les requêtes compilées en mode natif sont toujours capturés lorsque le mode de capture de requête est défini sur All (Tous), Auto ou Custom (Personnalisé).

ALL     
Capture toutes les requêtes. **ALL** est la valeur de configuration par défaut pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]).

AUTO     
Capturer les requêtes pertinentes en fonction du nombre d’exécutions et de la consommation de ressources. Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Aucune     
Cesser la capture des nouvelles requêtes. Le Magasin des requêtes continue de collecter des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer la capture de requêtes importantes.

CUSTOM     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Permet de contrôler les options QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE est de type **nvarchar**. 

max_plans_per_query     
Définit le nombre maximal de plans gérés pour chaque requête. MAX_PLANS_PER_QUERY est de type **int**. La valeur par défaut est **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

Contrôle si les statistiques d’attente seront capturées par requête.

ACTIVÉ    
Les informations des statistiques d’attente par requête sont capturées. Il s’agit de la valeur de configuration par défaut.

OFF    
Les informations des statistiques d’attente par requête ne seront pas capturées.

**\<query_capture_policy_option_list> :: =**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

Contrôle les options de stratégie de capture du magasin des requêtes. Sauf pour STALE_CAPTURE_POLICY_THRESHOLD, ces options définissent dans la valeur de la durée de validité de la stratégie de capture les conditions OR qui doivent intervenir pour que les requêtes soient capturées.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Définit la période d’évaluation pour déterminer si une requête doit être capturée. La valeur par défaut est de 1 jour, les valeurs possibles oscillant entre 1 heure et sept jours. *number* est de type **int**.

EXECUTION_COUNT     
Définit le nombre d’exécutions d’une requête pendant la période d’évaluation. La valeur par défaut est 30, ce qui signifie que pour la durée de validité par défaut de la stratégie de capture, une requête doit s’exécuter au moins 30 fois dans la même journée pour devenir persistante dans le Magasin des requêtes. EXECUTION_COUNT est de type **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Définit le temps UC de compilation écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 1000 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins une seconde de temps processeur écoulé pendant la compilation de la requête dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_COMPILE_CPU_TIME_MS est de type **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Définit le temps UC d’exécution écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 100 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins 100 ms de temps processeur écoulé pendant l’exécution dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_EXECUTION_CPU_TIME_MS est de type **int**.

**\<recovery_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Contrôle les options de récupération de base de données et la vérification des erreurs d'E/S disque.

FULL     
Assure une récupération complète après la défaillance d'un support à l'aide des sauvegardes de journaux des transactions. Si un fichier de données est endommagé, la récupération des supports peut restaurer toutes les transactions validées. Pour plus d’informations, voir [Modes de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md).

BULK_LOGGED     
Assure une récupération après la défaillance d’un support. Cette option associe des performances optimales et une utilisation minimale de l’espace réservé aux fichiers journaux pour certaines opérations en bloc ou de grande échelle. Pour plus d’informations sur les opérations pouvant faire l’objet d’une journalisation minimale, voir [Journal des transactions](../../relational-databases/logs/the-transaction-log-sql-server.md). Avec le mode de récupération BULK_LOGGED, ces opérations font l'objet d'une journalisation minimale. Pour plus d’informations, voir [Modes de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md).

SIMPLE     
Une stratégie de sauvegarde simple utilisant un espace de journalisation minimal est fournie. L’espace réservé aux fichiers journaux peut être automatiquement réutilisé lorsqu’il n’est plus utilisé pour la récupération des défaillances serveur. Pour plus d’informations, voir [Modes de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md).

> [!IMPORTANT]
> Le mode de récupération simple est plus facile à gérer que les deux autres modes, mais le risque de perte de données est plus élevé lorsqu'un fichier de données est endommagé. Toutes les modifications apportées depuis la dernière sauvegarde de la base de données ou de la sauvegarde différentielle de la base de données sont perdues et doivent être réintroduites manuellement.

Le mode de récupération par défaut dépend du mode de récupération de la base de données **model** . Pour plus d’informations sur le choix du modèle de récupération, voir [Modèles de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md).

Vous pouvez déterminer l’état de cette option en consultant les colonnes **recovery_model** et **recovery_model_desc** dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `Recovery` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

TORN_PAGE_DETECTION { ON | **OFF** }     
ACTIVÉ     
Les pages incomplètes peuvent être détectées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].

OFF     
Les pages incomplètes ne peuvent pas être détectées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].

> [!IMPORTANT]
> La structure syntaxique TORN_PAGE_DETECTION ON | OFF sera supprimée dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette structure syntaxique dans le développement de nouvelles applications et envisagez de modifier celles qui l'utilisent actuellement. Utilisez l'option PAGE_VERIFY à la place.

<a name="page_verify"></a> PAGE_VERIFY { **CHECKSUM** | TORN_PAGE_DETECTION | NONE }     
Détecte les pages de base de données endommagées résultant d'erreurs de chemin d'E/S disque. Les erreurs de chemin d’E/S disque peuvent être la cause de problèmes de corruption de base de données. Ces erreurs sont le plus souvent provoquées par des pannes d’alimentation ou des défaillances matérielles du disque qui se produisent au moment où la page est écrite sur le disque.

CHECKSUM     
Calcule une somme de contrôle du contenu d'une page entière et stocke la valeur dans l'en-tête de page lorsque celle-ci est écrite sur le disque. Lorsque la page est ensuite lue à partir du disque, la somme de contrôle est recalculée et le résultat est comparé à la valeur préalablement stockée dans l'en-tête de la page. Si les valeurs diffèrent, le message d’erreur 824 (indiquant l’échec d’une somme de contrôle) est signalé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le journal des événements Windows. Un échec de somme de contrôle indique un problème de chemin d'E/S. Pour en déterminer la cause, vous devez examiner le matériel, les pilotes de microprogrammes, le BIOS, les pilotes de filtre (par exemple un logiciel antivirus) et d'autres composants de chemin d'E/S.

TORN_PAGE_DETECTION     
Enregistre un modèle spécifique de 2 bits pour chaque secteur de 512 octets dans la page de base de données de 8 kilo-octets (Ko) et le stocke dans l'en-tête de page de base de données au moment où la page est écrite sur le disque. Lorsque la page est ensuite lue à partir du disque, les bits endommagés stockés dans l'en-tête de la page sont comparés aux informations réelles du secteur concerné.

Lorsque les valeurs ne concordent pas, cela signifie que seule une partie de la page a été écrite sur le disque. Dans un tel cas, le message d'erreur 824 (indiquant une erreur de page endommagée) est signalé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le journal des événements Windows. Les pages endommagées sont généralement détectées par la récupération de base de données s’il s’agit réellement d’une écriture de page incomplète. Toutefois, les échecs de chemin d'E/S peuvent donner lieu à tout moment à une page endommagée.

Aucune     
Les écritures de page de base de données ne génèrent pas de valeur CHECKSUM ni TORN_PAGE_DETECTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vérifie pas une somme de contrôle ou une page endommagée au cours d'une lecture, même si l'en-tête de page comporte une valeur CHECKSUM ou TORN_PAGE_DETECTION.

Prenez en considération les points suivants lorsque vous utilisez l'option PAGE_VERIFY :

- La valeur par défaut est **CHECKSUM**.
- Quand une base de données utilisateur ou système est mise à niveau vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou une version ultérieure, la valeur de PAGE_VERIFY (NONE ou TORN_PAGE_DETECTION) n’est pas changée. Nous vous recommandons de changer pour CHECKSUM.

    > [!NOTE]
    > Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’option de base de données PAGE_VERIFY est définie par la valeur NONE pour la base de données TempDB et ne peut pas être modifiée. À compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], la valeur par défaut pour la base de données TempDB est CHECKSUM pour les nouvelles installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand vous mettez à niveau une installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la valeur par défaut reste NONE. L'option peut être modifiée. Nous vous recommandons d'utiliser CHECKSUM pour la base de données tempdb.

- Même si la valeur TORN_PAGE_DETECTION utilise moins de ressources, elle ne fournit qu'un sous-ensemble limité de la protection offerte par CHECKSUM.
- Il est possible de définir PAGE_VERIFY sans mettre la base de données hors connexion, la verrouiller ni empêcher d'une quelconque façon la concurrence pour cette base de données.
- CHECKSUM et TORN_PAGE_DETECTION s'excluent mutuellement. Les deux options ne peuvent pas être activées en même temps.

Lors de la détection d'une page endommagée ou d'un échec de somme de contrôle, vous pouvez procéder à une récupération en restaurant les données ou en reconstruisant éventuellement l'index si la défaillance se limite à des pages d'index. En présence d'une erreur de somme de contrôle, pour déterminer le type de la ou des pages de données affectées, exécutez DBCC CHECKDB. Pour plus d’informations sur les options de restauration, voir [Arguments RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Bien que la restauration des données permette de résoudre le problème d'endommagement des données, la cause première, par exemple une défaillance matérielle du disque, doit être identifiée et corrigée le plus rapidement possible pour éviter que ces erreurs persistent.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procède à quatre nouvelles tentatives pour une lecture qui échoue avec une erreur de somme de contrôle, de page endommagée ou d'E/S disque. Si la lecture réussit lors de l’une de ces tentatives, un message est écrit dans le journal des erreurs. La commande qui a déclenché la lecture continue. La commande échoue avec le message d’erreur 824 si les tentatives de lecture échouent.

Pour plus d’informations sur les messages d’erreur 823, 824 et 825, consultez :

- [How to troubleshoot a Msg 823 error in SQL Server](https://support.microsoft.com/help/2015755) (Comment faire pour dépanner une erreur Msg 823 dans SQL Server)
- [How to troubleshoot a Msg 824 error in SQL Server](https://support.microsoft.com/help/2015756) (Comment faire pour dépanner une erreur Msg 824 dans SQL Server)
- [Comment résoudre les problèmes de Msg – nouvelle tentative de lecture](https://support.microsoft.com/help/2015757).

Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne `page_verify_option` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsTornPageDetectionEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<remote_data_archive_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])

Active ou désactive Stretch Database pour la base de données. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

REMOTE_DATA_ARCHIVE = { ON ( SERVER = \<server_name> , { CREDENTIAL = \<db_scoped_credential_name> \| FEDERATED_SERVICE_ACCOUNT = ON \| OFF } )\| **OFF**     
ACTIVÉ     
Active Stretch Database pour la base de données. Pour plus d’informations, notamment sur les prérequis supplémentaires, consultez [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

Nécessite l’autorisation `db_owner` pour activer Stretch Database pour une table. Nécessite les autorisations `db_owner` et `CONTROL DATABASE` pour activer Stretch Database pour une base de données.

SERVER = \<server_name>     
Spécifie l’adresse du serveur Azure. Incluez la partie `.database.windows.net` du nom. Par exemple : `MyStretchDatabaseServer.database.windows.net`.

CREDENTIAL = \<db_scoped_credential_name>     
Spécifie les informations d’identification délimitées à la base de données utilisées par l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter au serveur Azure. Vérifiez que les informations d’identification existent avant d’exécuter cette commande. Pour plus d’informations, voir [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

FEDERATED_SERVICE_ACCOUNT = { ON | OFF }     
Vous pouvez utiliser un compte de service fédéré pour que le serveur SQL Server local communique avec le serveur Azure distant quand les conditions suivantes sont toutes remplies.

- Le compte de service sous lequel l’instance de SQL Server s’exécute est un compte de domaine.
- Le compte de domaine appartient à un domaine dont Active Directory est fédéré avec Azure Active Directory.
- Le serveur Azure distant est configuré pour prendre en charge l’authentification Azure Active Directory.
- Le compte de service sous lequel s’exécute l’instance de SQL Server doit être configuré comme un compte `dbmanager` ou `sysadmin` sur le serveur Azure distant.

En outre, si vous spécifiez que le compte de service fédéré est ON, vous ne pouvez pas spécifier l’argument CREDENTIAL. Vous devez fournir l’argument CREDENTIAL si vous spécifiez OFF.

OFF     
Désactive Stretch Database pour la base de données. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

Vous pouvez uniquement désactiver Stretch Database pour une base de données une fois que celle-ci ne contient plus de tables qui sont activées pour Stretch Database. Lorsque vous désactivez Stretch Database, la migration des données s’arrête. En outre, les résultats de la requête n’incluent plus les résultats de tables distantes.

La désactivation de Stretch ne supprime pas la base de données distante. Pour supprimer la base de données distante, supprimez-la en utilisant le portail Azure.

**\<service_broker_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Contrôle les options [!INCLUDE[ssSB](../../includes/sssb-md.md)] suivantes : active ou désactive la remise de messages, définit un nouvel identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou définit les priorités de conversation sur ON ou OFF.

ENABLE_BROKER     
Spécifie que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activé pour la base de données spécifiée. La remise des messages est démarrée et l’indicateur is_broker_enabled est défini sur true dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). La base de données conserve l’identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant. Service Broker ne peut pas être activé lorsque la base de données est la base de données principale dans une configuration de mise en miroir de bases de données.

> [!NOTE]
> ENABLE_BROKER requiert un verrou de base de données exclusif. Si d'autres sessions ont verrouillé des ressources dans la base de données, ENABLE_BROKER attend jusqu'à ce que les autres sessions libèrent leurs verrous. Pour activer [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans une base de données utilisateur, vérifiez qu'aucune autre session n'utilise la base de données avant d'exécuter l'instruction ALTER DATABASE SET ENABLE_BROKER, comme le fait de mettre la base de données en mode mono-utilisateur. Pour activer [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la base de données msdb, arrêtez d'abord l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], afin que [!INCLUDE[ssSB](../../includes/sssb-md.md)] puisse obtenir le verrou nécessaire.

DISABLE_BROKER     
Spécifie que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est désactivé pour la base de données spécifiée. La remise des messages est arrêtée et l’indicateur is_broker_enabled est défini sur false dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). La base de données conserve l’identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant.

NEW_BROKER     
Spécifie que la base de données doit recevoir un nouvel identificateur Service Broker. La base de données agit comme un nouveau Service Broker. Toutes les conversations existantes dans la base de données sont ainsi immédiatement supprimées sans générer de message de fin de dialogue. Tout itinéraire qui fait référence à l'ancien identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être recréé avec le nouvel identificateur.

ERROR_BROKER_CONVERSATIONS     
Spécifie que la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activée. Ce paramètre préserve l’identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant de la base de données. [!INCLUDE[ssSB](../../includes/sssb-md.md)] met fin à toutes les conversations de la base de données avec une erreur. Ce paramètre permet aux applications d’exécuter un nettoyage régulier des conversations existantes.

HONOR_BROKER_PRIORITY {ON | OFF}     
ACTIVÉ     
Les opérations d'envoi prennent en considération les niveaux de priorité assignés aux conversations. Les messages issus de conversations dont les niveaux de priorité sont élevés sont envoyés avant ceux issus de conversations dont les niveaux de priorité sont faibles.

OFF     
Les opérations d'envoi s'exécutent comme si toutes les conversations étaient dotées du niveau de priorité par défaut.

Les modifications apportées à l'option HONOR_BROKER_PRIORITY prennent effet immédiatement pour les nouveaux dialogues ou les dialogues qui n'ont pas de messages en attente d'envoi. Les dialogues avec des messages à envoyer lorsque l’instruction ALTER DATABASE est exécutée ne récupéreront pas le nouveau paramètre tant que certains messages pour le dialogue ne sont pas envoyés. La durée nécessaire pour que tous les dialogues commencent à utiliser le nouveau paramètre peut varier considérablement.

La valeur actuelle de cette propriété est indiqué dans la colonne `is_broker_priority_honored` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<snapshot_option> ::=**      
Calcule le niveau d’isolation de la transaction.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ACTIVÉ     
Active l'option Instantané au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions peuvent spécifier le niveau d'isolement des transactions SNAPSHOT. Quand une transaction s'exécute au niveau d'isolement SNAPSHOT, toutes les instructions voient un instantané des données tel qu'il existe au début de la transaction. Si une transaction exécutée au niveau d'isolement SNAPSHOT accède à des données dans plusieurs bases de données, l'option ALLOW_SNAPSHOT_ISOLATION doit avoir la valeur ON dans toutes les bases de données ou chaque instruction de la transaction doit utiliser des indicateurs de verrouillage sur toute référence d'une clause FROM à une table de la base de données dont l'option ALLOW_SNAPSHOT_ISOLATION a la valeur OFF.

OFF     
Désactive l'option Instantané au niveau de la base de données. Les transactions peuvent spécifier le niveau d’isolation de la transaction SNAPSHOT.

Lorsque vous modifiez l'état de ALLOW_SNAPSHOT_ISOLATION (de ON à OFF ou inversement), ALTER DATABASE ne retourne pas le contrôle à l'appelant tant que toutes les transactions existantes dans la base de données n'ont pas été validées. Si la base de données présente déjà l'état spécifié dans l'instruction ALTER DATABASE, le contrôle est immédiatement retourné à l'appelant. Si l’instruction ALTER DATABASE n’est pas retournée rapidement, utilisez [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) pour déterminer si certaines transactions sont de longue durée. Si l'instruction ALTER DATABASE est annulée, la base de données conserve l'état qu'elle présentait au démarrage de ALTER DATABASE. la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indique l’état des transactions d’isolement d’instantané dans la base de données. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF marque une pause de six secondes puis réessaie d’exécuter l’opération.

Vous ne pouvez pas modifier l’état de ALLOW_SNAPSHOT_ISOLATION si la base de données est hors connexion (OFFLINE).

Si vous configurez ALLOW_SNAPSHOT_ISOLATION dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé si la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Vous pouvez modifier les paramètres de ALLOW_SNAPSHOT_ISOLATION pour les bases de données master, model, msdb et tempdb. Le paramètre est conservé à chaque arrêt et redémarrage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] si vous changez le paramètre pour la base de données tempdb. Si vous modifiez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées à l'exception de tempdb.

Cette option est ON par défaut pour les bases de données master et msdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `snapshot_isolation_state` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ACTIVÉ     
Active l'option d'instantané de lecture validée au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions qui définissent le niveau d'isolement de lecture validée utilisent le contrôle de version de ligne au lieu du verrouillage. Toutes les instructions voient un instantané des données telles qu’elles existent au début de l’instruction quand une transaction est exécutée au niveau d’isolation READ COMMITTED.

OFF     
Désactive l'option d'instantané de lecture validée au niveau de la base de données. Les transactions spécifiant le niveau d'isolation READ COMMITTED utilisent le verrouillage.

Pour définir READ_COMMITTED_SNAPSHOT sur ON ou sur OFF, il ne doit exister aucune connexion active à la base de données, à l’exception de la connexion exécutant la commande ALTER DATABASE. Toutefois, il n’est pas nécessaire que la base de données soit en mode mono-utilisateur. Vous ne pouvez pas modifier l’état de cette option si la base de données est hors connexion (OFFLINE).

Si vous configurez READ_COMMITTED_SNAPSHOT dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé lorsque la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Il n’est pas possible d’affecter la valeur ON à READ_COMMITTED_SNAPSHOT pour les bases de données système master, tempdb ou msdb. Si vous changez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées, à l’exception de tempdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_read_committed_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quand une table est créée avec **DURABILITY = SCHEMA_ONLY** et que **READ_COMMITTED_SNAPSHOT** est changé par la suite à l’aide d’**ALTER DATABASE**, les données de la table sont perdues.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

ACTIVÉ     
Quand le niveau d’isolation de la transaction est défini sur un niveau inférieur à SNAPSHOT, toutes les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée sont exécutées avec l’isolation SNAPSHOT. Des niveaux d’isolation inférieurs à snapshot sont, par exemple, READ COMMITTED ou READ UNCOMMITTED. Ces opérations sont exécutées si le niveau d’isolation de la transaction est défini explicitement sur le niveau de la session, ou si la valeur par défaut est utilisée implicitement.

OFF     
N’élève pas le niveau d’isolation pour les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée.

Vous ne pouvez pas modifier l’état de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de données est hors connexion (OFFLINE).

L’option par défaut est OFF.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_memory_optimized_elevate_to_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Contrôle les options de conformité ANSI au niveau de la base de données.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Détermine la valeur par défaut, NULL ou NOT NULL, d’une colonne, d’un [type CLR défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) dont le paramètre d’acceptation des valeurs NULL n’est pas défini de façon explicite dans les instructions CREATE TABLE ou ALTER TABLE. Les colonnes définies avec des contraintes respectent les règles de contrainte, quel que soit ce paramètre.

ACTIVÉ     
La valeur par défaut d’une colonne non définie est NULL.

OFF     
La valeur par défaut d’une colonne non définie est NOT NULL.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre défini pour ANSI_NULL_DEFAULT au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULL_DEFAULT pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Pour garantir la compatibilité ANSI, l'activation (ON) de l'option de base de données ANSI_NULL_DEFAULT entraîne la définition de NULL comme valeur par défaut de la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_null_default_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullDefault` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ACTIVÉ     
Toutes les comparaisons avec une valeur Null produisent le résultat UNKNOWN (inconnu).

OFF     
Les comparaisons de valeurs non-UNICODE avec une valeur Null génèrent la valeur TRUE si les deux valeurs sont Null.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut défini pour ANSI_NULLS au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULLS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> SET ANSI_NULLS doit également avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_nulls_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVÉ     
Les chaînes sont complétées pour avoir la même longueur avant leur conversion. Elles sont également complétées pour avoir la même longueur avant leur insertion dans un type de données **varchar** ou **nvarchar**.

OFF     
Cette option insère des espaces à droite dans les valeurs de type character dans des colonnes **varchar** ou **nvarchar**. Cette option laisse également les zéros à droite dans les valeurs de type binary insérées dans des colonnes **varbinary**. Les valeurs ne sont pas complétées à concurrence de la longueur de la colonne.

Lorsque cette option a la valeur OFF, elle affecte uniquement la définition des nouvelles colonnes.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Il est recommandé de toujours affecter la valeur ON à l'option ANSI_PADDING. Par ailleurs, ANSI_PADDING doit avoir la valeur ON lorsque vous créez ou manipulez des index dans des colonnes calculées ou des vues indexées.

Les colonnes **char(_n_)** et **binary(_n_)** qui acceptent des valeurs NULL sont complétées à concurrence de la longueur de la colonne lorsque ANSI_PADDING a la valeur ON. Les espaces à droite et les zéros sont tronqués lorsque ANSI_PADDING a la valeur OFF. Les colonnes **char(_n_)** et **binary(_n_)** qui n’acceptent pas les valeurs NULL sont toujours complétées à concurrence de la longueur de la colonne.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre de ANSI_PADDING au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_PADDING pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_padding_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiPaddingEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVÉ     
Des erreurs ou avertissements sont émis si des conditions telles que « division par zéro » sont vérifiées. Des erreurs et avertissements sont également générés lorsque des valeurs Null apparaissent dans des fonctions d’agrégation.

OFF     
Aucun avertissement n'est généré et des valeurs Null sont retournées lorsque des conditions telles qu'une division par zéro se manifestent.

> [!IMPORTANT]
> SET ANSI_WARNINGS doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de ANSI_NULLS défini au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_WARNINGS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_warnings_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiWarningsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVÉ     
Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.

OFF     
Un message d’avertissement s’affiche quand l’une de ces erreurs se produit. Le traitement de la requête, du lot ou de la transaction se poursuit comme s’il n’y avait pas d’erreur, même si un message d’avertissement s’affiche.

> [!IMPORTANT]
> SET ARITHABORT doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_arithabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsArithmeticAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }     

Pour plus d’informations, voir [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVÉ     
Le résultat d'une concaténation est NULL lorsque l'un des deux opérandes est NULL. Par exemple, la concaténation de la chaîne de caractères « Ceci est » et de NULL retourne la valeur NULL au lieu de la valeur « Ceci est ».

OFF     
La valeur Null est considérée comme une chaîne de caractères vide.

> [IMPORTANT] CONCAT_NULL_YIELDS_NULL doit être défini sur ON quand vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.
>
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL sera toujours ON et toute application qui définira explicitement l’option à OFF déclenchera une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de CONCAT_NULL_YIELDS_NULL défini au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à CONCAT_NULL_YIELDS_NULL pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_concat_null_yields_null_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNullConcat` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVÉ     
Des guillemets doubles peuvent être utilisés pour entourer des identificateurs délimités.

Toutes les chaînes délimitées par des guillemets doubles sont considérées comme des identificateurs d'objet. Les identificateurs entre guillemets n’ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] propres aux identificateurs. Ils peuvent être des mots clés et contenir des caractères non autorisés dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si un guillemet simple (') fait partie de la chaîne littérale, il pourra être représenté par un guillemet double ('').

OFF     
Les identificateurs ne peuvent pas être mis entre guillemets et doivent respecter toutes les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet également de délimiter les identificateurs par des crochets ([ ]). Les identificateurs entre crochets peuvent toujours être utilisés, quel que soit le paramètre QUOTED_IDENTIFIER. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

Lors de la création d’une table, l’option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table. L’option est stockée même si elle a la valeur OFF au moment de la création de la table.

Les paramètres définis au niveau de la connexion à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de QUOTED_IDENTIFIER. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à QUOTED_IDENTIFIER par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_quoted_identifier_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsQuotedIdentifiersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVÉ     
Une erreur est générée lors d'une perte de précision dans une expression.

OFF     
La perte de précision ne génère pas de message d’erreur et le résultat est arrondi en fonction de la précision de la colonne ou de la variable stockant le résultat.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT doit avoir la valeur OFF lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_numeric_roundabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNumericRoundAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVÉ     
L'activation récursive des déclencheurs AFTER est autorisée.

OFF     
Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Seule la récursivité directe est désactivée lorsque RECURSIVE_TRIGGERS a la valeur OFF. Pour désactiver la récursivité indirecte, vous devez aussi affecter la valeur 0 à l'option serveur nested triggers.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Spécifie la fréquence des points de contrôle indirects en fonction de chaque base de données. À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la valeur par défaut pour les nouvelles bases de données est **1 minute**, ce qui signifie que la base de données utilise des points de contrôle indirects. Pour les versions antérieures, la valeur par défaut est 0, ce qui indique que la base de données utilise les points de contrôle automatiques, dont la fréquence dépend du paramètre d’intervalle de récupération de l’instance de serveur. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande une valeur d’une minute pour la plupart des systèmes.

TARGET_RECOVERY_TIME **=** *target_recovery_time* { SECONDS | MINUTES }     
*target_recovery_time*     
Spécifie la limite maximale de durée de récupération de la base de données spécifiée en cas de sinistre. *target_recovery_time* est de type **int**.

SECONDS     
Indique que *target_recovery_time* correspond au nombre de secondes.

MINUTES     
Indique que *target_recovery_time* correspond au nombre de minutes.

Pour plus d’informations sur les points de contrôle indirects, voir [Points de contrôle de base de données](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Spécifie le(s) cas où une transaction incomplète doit être restaurée lors d'un changement d'état de la base de données. Lorsque la clause de fin est omise, l’instruction ALTER DATABASE attend indéfiniment s’il existe un verrou quelconque sur la base de données. Une seule clause de fin peut être spécifiée, à la suite des clauses SET.

> [!NOTE]
> Toutes les options de base de données n’utilisent pas la clause WITH \<termination>. Pour plus d’informations, consultez le tableau sous « [Options de configuration](#SettingOptions) » dans la section « Remarques » de cet article.

ROLLBACK AFTER *number* [SECONDS] | ROLLBACK IMMEDIATE     
Indique si la restauration intervient après le nombre de secondes spécifié ou immédiatement. *number* est de type **int**.

NO_WAIT     
Spécifie que la requête échoue si le changement d’état ou d’option de base de données demandé ne peut pas être effectué immédiatement. Une exécution immédiate signifie ne pas attendre la validation ou la restauration des transactions.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuration des options
Pour récupérer les paramètres actuels des options de base de données, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Quand vous définissez une option de base de données, la nouvelle valeur prend effet immédiatement.

Vous pouvez modifier les valeurs par défaut de l’une des options de base de données afin qu’elles s’appliquent à toutes les nouvelles bases de données créées. Pour ce faire, modifiez l’option de base de données appropriée dans la base de données model.

Toutes les options de base de données n’utilisent pas la clause WITH \<termination> et ne peuvent pas être combinées avec d’autres options. Le tableau suivant répertorie ces options ainsi que l'état de l'option et d'arrêt.

|Catégorie d'options|Peut être spécifiée avec d'autres options|Peut utiliser la clause WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<db_state_option>|Oui|Oui|
|\<db_user_access_option>|Oui|Oui|
|\<db_update_option>|Oui|Oui|
|\<delayed_durability_option>|Oui|Oui|
|\<external_access_option>|Oui|Non|
|\<cursor_option>|Oui|Non|
|\<auto_option>|Oui|Non|
|\<sql_option>|Oui|Non|
|\<recovery_option>|Oui|Non|
|\<target_recovery_time_option>|Non|Oui|
|\<database_mirroring_option>|Non|Non|
|ALLOW_SNAPSHOT_ISOLATION|Non|Non|
|READ_COMMITTED_SNAPSHOT|Non|Oui|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Oui|Oui|
|\<service_broker_option>|Oui|Non|
|DATE_CORRELATION_OPTIMIZATION|Oui|Oui|
|\<parameterization_option>|Oui|Oui|
|\<change_tracking_option>|Oui|Oui|
|\<db_encryption_option>|Oui|Non|

Le cache de plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est effacé par la configuration d'une des options suivantes :

:::row:::
    :::column:::
        OFFLINE
    :::column-end:::
    :::column:::
        ONLINE
    :::column-end:::
    :::column:::
        MODIFY_NAME
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        COLLATE
    :::column-end:::
    :::column:::
        READ_ONLY
    :::column-end:::
    :::column:::
        READ_WRITE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        MODIFY FILEGROUP DEFAULT
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_WRITE
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_ONLY
    :::column-end:::
:::row-end:::

Le cache du plan est également vidé dans les scénarios suivants.

- L'option de base de données AUTO_CLOSE est activée (ON). Lorsqu'aucune connexion utilisateur ne fait référence ou n'utilise la base de données, la tâche en arrière-plan essaie de fermer et d'arrêter la base de données automatiquement.
- Vous exécutez plusieurs requêtes sur une base de données dont les options par défaut sont activées. Puis, la base de données est supprimée.
- Un instantané de base de données pour une base de données source est supprimé.
- Vous reconstruisez avec succès le journal des transactions d'une base de données.
- Vous restaurez une sauvegarde de base de données.
- Vous détachez une base de données.

Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée du cache du plan, le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.

## <a name="examples"></a>Exemples

### <a name="a-setting-options-on-a-database"></a>R. Configuration des options d'une base de données

L'exemple suivant définit les options de mode de récupération et de vérification de pages de données pour l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;
GO

```

### <a name="b-setting-the-database-to-read_only"></a>B. Paramétrage de la base de données avec READ_ONLY

Pour modifier l’état d’une base de données ou d’un groupe de fichiers en READ_ONLY ou en READ_WRITE, vous avez besoin d’un accès exclusif à la base de données. L'exemple ci-dessous illustre le basculement de la base de données en mode `SINGLE_USER` pour obtenir l'accès exclusif. L'exemple affecte ensuite à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] l'état `READ_ONLY` et rend à tous les utilisateurs l'accès à la base de données.

> [!NOTE]
> Cet exemple utilise l'option de fin `WITH ROLLBACK IMMEDIATE` dans la première instruction `ALTER DATABASE` . Toutes les transactions incomplètes seront restaurées et les autres connexions à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] immédiatement déconnectées.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. Activation de l'isolement d'instantané sur une base de données

L'exemple ci-dessous illustre l'activation de l'option d'infrastructure d'isolement d'instantané pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO

```

Le jeu de résultats montre que l'infrastructure d'isolement d'instantané est activée.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[nom_base_de_données] |1| ACTIVÉ |

### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D. Activation, modification et désactivation du suivi des modifications

L'exemple ci-dessous illustre l'activation du suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et la définition d'une période de rétention de `2` jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

L'exemple suivant illustre comment modifier la période de rétention en spécifiant `3` jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

L'exemple ci-dessous illustre comment désactiver le suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="e-enabling-the-query-store"></a>E. Activation du magasin de requêtes

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])

L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="f-enabling-the-query-store-with-wait-statistics"></a>F. Activation du magasin des requêtes avec les statistiques d’attente

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="g-enabling-the-query-store-with-custom-capture-policy-options"></a>G. Activation du magasin des requêtes avec les options de stratégie de capture personnalisée

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Voir aussi

- [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)
- [Statistiques](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Activer et désactiver le suivi des modifications](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bonnes pratiques relatives au Magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

Bien que les niveaux de compatibilité soient des options `SET`, ils sont décrits dans [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> La plupart des options SET de base de données sont configurables pour la session en cours avec les [Instructions SET](../../t-sql/statements/set-statements-transact-sql.md), souvent par des applications au moment de la connexion. Les options SET de niveau session remplacent les valeurs **ALTER DATABASE SET**. Les options de base de données décrites dans les sections suivantes sont des valeurs que vous pouvez définir pour les sessions qui ne fournissent pas explicitement d’autres valeurs pour les options SET.

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}
;

<option_spec> ::=
{
    <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }
  | AUTOMATIC_TUNING ( CREATE_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( DROP_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
        = OFF
      | = ON [ ( <change_tracking_option_list > [,...n] ) ]
      | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<db_update_option> ::=
  { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
  { RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::= DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
      = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<termination>::=
{
    ROLLBACK AFTER integer [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention>::=TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Arguments

*database_name*     
Nom de la base de données à modifier.

CURRENT     
`CURRENT` exécute l’action dans la base de données active. `CURRENT` n’est pas pris en charge pour toutes les options dans tous les contextes. Si `CURRENT` échoue, fournissez le nom de la base de données.

**\<auto_option> ::=**      
Contrôle les options automatiques.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ACTIVÉ     
L’optimiseur de requête crée si nécessaire des statistiques sur les colonnes uniques des prédicats de requête, afin d’améliorer les plans de requête et les performances des requêtes. Ces statistiques de colonnes uniques sont créées quand l’optimiseur de requête compile les requêtes. Les statistiques de colonnes uniques sont créées uniquement sur les colonnes qui ne constituent pas déjà la première colonne d'un objet de statistiques existant.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

OFF     
L’optimiseur de requête ne crée pas de statistiques sur les colonnes uniques des prédicats de requête quand il compile les requêtes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_create_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoCreateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Options des statistiques » dans [Statistiques](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Définissez AUTO_CREATE_STATISTICS sur la valeur ON, et INCREMENTAL sur la valeur ON. Ce paramètre crée automatiquement des statistiques incrémentielles sur les statistiques incrémentielles sont prises en charge. La valeur par défaut est OFF. Pour plus d’informations, voir [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVÉ     
Les fichiers de base de données peuvent faire l'objet d'une réduction périodique.

Les fichiers de données et les fichiers journaux peuvent être automatiquement réduits. AUTO_SHRINK ne réduit la taille du journal des transactions que si vous définissez la base de données sur le mode de récupération SIMPLE ou si vous sauvegardez le journal. Si la valeur spécifiée est OFF, les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l’espace inutilisé.

L'option AUTO_SHRINK provoque un compactage dès qu'un fichier comprend plus de 25 % d'espace inutilisé. L’option provoque une réduction du fichier à l’une de deux tailles. Il est réduit à la taille plus grande retenue :

- La taille où 25 pour cent du fichier correspond à l’espace inutilisé
- La taille du fichier quand il a été créé

Vous ne pouvez pas réduire une base de données en lecture seule.

OFF     
Les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l'espace inutilisé.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_shrink_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoShrink` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L’option AUTO_SHRINK n’est pas disponible dans une base de données autonome.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVÉ     
Spécifie que l’optimiseur de requête met à jour les statistiques quand elles sont utilisées par une requête et quand elles sont susceptibles d’être obsolètes. Les statistiques deviennent obsolètes après que des opérations d'insertion, de mise à jour, de suppression ou de fusion ont modifié la distribution des données dans la table ou la vue indexée. L’optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.

L’optimiseur de requête vérifie s’il existe des statistiques obsolètes avant de compiler une requête et d’exécuter un plan de requête mis en cache. L’optimiseur de requête utilise les colonnes, les tables et les vues indexées du prédicat de requête pour identifier les statistiques susceptibles d’être obsolètes. L’optimiseur de requête détermine ces informations avant de compiler une requête. Avant d’exécuter un plan de requête mis en cache, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que le plan de requête référence des statistiques à jour.

L'option AUTO_UPDATE_STATISTICS s'applique aux statistiques créées pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l'aide de l'instruction CREATE STATISTICS. Cette option s'applique également aux statistiques filtrées.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

Utilisez l'option AUTO_UPDATE_STATISTICS_ASYNC pour spécifier si les statistiques doivent être mises à jour en mode synchrone ou asynchrone.

OFF     
Spécifie que l’optimiseur de requête ne met pas à jour les statistiques quand elles sont utilisées par une requête. L’optimiseur de requête ne met pas non plus à jour les statistiques quand elles sont susceptibles d’être obsolètes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_update_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoUpdateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Options des statistiques » dans [Statistiques](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVÉ     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont asynchrones. L’optimiseur de requête n’attend pas la fin des mises à jour des statistiques pour compiler les requêtes.

Affecter la valeur ON à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Par défaut, l’option AUTO_UPDATE_STATISTICS_ASYNC est définie sur OFF ; l’optimiseur de requête met à jour les statistiques en mode synchrone.

OFF     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont synchrones. L’optimiseur de requête attend la fin des mises à jour des statistiques pour compiler les requêtes.

Affecter la valeur OFF à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_update_stats_async_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Pour plus d’informations décrivant quand utiliser des mises à jour de statistiques synchrones ou asynchrones, consultez la section « Options des statistiques » dans [Statistiques](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**S’applique à** : [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]

Contrôle les options automatiques pour [Optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }     
AUTO     
La définition de l’optimisation automatique sur AUTO applique les paramètres de configuration Azure par défaut à l’optimisation automatique.

INHERIT     
L’utilisation de la valeur INHERIT permet d’hériter de la configuration par défaut du serveur parent. Ceci est particulièrement pratique si vous voulez personnaliser la configuration de l’optimisation automatique sur un serveur parent et que toutes les bases de données sur ce serveur HÉRITENT de ces paramètres personnalisés. Veuillez noter que pour que l’héritage fonctionne, les trois options de réglage FORCE_LAST_GOOD_PLAN, CREATE_INDEX et DROP_INDEX doivent avoir la valeur DEFAULT dans les bases de données.

CUSTOM     
Avec la valeur CUSTOM, vous devrez personnaliser la configuration de chaque option de l’optimisation automatique disponible sur les bases de données.

Active ou désactive l’option `CREATE_INDEX` de gestion des index automatique de l’[optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

CREATE_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Hérite des paramètres par défaut du serveur. Dans ce cas, les options d’activation ou de désactivation automatique des fonctionnalités du réglage automatique sont définies au niveau du serveur.

ACTIVÉ     
Si activé, les index manquants sont automatiquement générés sur une base de données. Après la création des index, des gains de performances de la charge de travail se vérifient. Lorsque ces index créés ne fournissent plus d’avantages pour les performances de la charge de travail, ils sont automatiquement annulés. Les index créés automatiquement sont marqués comme index générés par le système.

OFF     
ne génère pas automatiquement les index manquants sur la base de données.

Active ou désactive l’option `DROP_INDEX` de gestion des index automatique de l’[optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

DROP_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Hérite des paramètres par défaut du serveur. Dans ce cas, les options d’activation ou de désactivation automatique des fonctionnalités du réglage automatique sont définies au niveau du serveur.

ACTIVÉ     
Supprime automatiquement les index en double ou qui ne sont plus utiles de la charge de travail des performances.

OFF     
ne supprime pas automatiquement les index manquants sur la base de données.

Active ou désactive l’option `FORCE_LAST_GOOD_PLAN` de correction automatique de plan de l’[optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF }     
DEFAULT     
Hérite des paramètres par défaut du serveur. Dans ce cas, les options d’activation ou de désactivation automatique des fonctionnalités du réglage automatique sont définies au niveau du serveur.

ACTIVÉ     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] force automatiquement le dernier plan correct connu sur les requêtes [!INCLUDE[tsql-md](../../includes/tsql-md.md)] là où le nouveau plan de requête provoque des régressions des performances. Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervise en permanence les performances de la requête [!INCLUDE[tsql-md](../../includes/tsql-md.md)] avec le plan forcé. S’il existe des gains de performances, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continue à utiliser le dernier plan correct connu. Si aucun gain de performances n’est détecté, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] génère un nouveau plan de requête. L’instruction échoue si le Magasin des requêtes n’est pas activé ou s’il n’est pas en mode *Lecture-écriture*.

OFF     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] indique les régressions des performances de requêtes potentielles dues à des changements de plan de requête dans la vue [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). Toutefois, ces recommandations ne sont pas appliquées automatiquement. Les utilisateurs peuvent superviser les recommandations actives et résoudre les problèmes identifiés en appliquant les scripts [!INCLUDE[tsql-md](../../includes/tsql-md.md)] qui sont montrés dans la vue. Il s’agit de la valeur par défaut.

**\<change_tracking_option> ::=**      
Contrôle les options de suivi des modifications. Vous pouvez activer le suivi des modifications, définir des options, modifier des options et désactiver le suivi des modifications. Pour des exemples, consultez la section « Exemples » plus loin dans cet article.

ACTIVÉ     
Active le suivi des modifications pour la base de données Lorsque vous activez le suivi des modifications, vous pouvez également définir les options AUTO CLEANUP et CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ACTIVÉ     
Les informations de suivi des modifications sont supprimées automatiquement à l'issue de la période de rétention spécifiée.

OFF     
Les données de suivi des modifications ne sont pas supprimées de la base de données.

CHANGE_RETENTION = *période_conservation* { **DAYS** | HOURS | MINUTES }     
Spécifie la période minimale de conservation des informations de suivi des modifications dans la base de données. Les données sont supprimées uniquement lorsque AUTO_CLEANUP a la valeur ON.

*retention_period* est un entier qui spécifie la composante numérique de la période de rétention.

La période de conservation par défaut est **2 jours**. La période de rétention minimale est 1 minute. Le type de conservation par défaut est **DAYS**.

OFF     
Désactive le suivi des modifications pour la base de données. Désactivez le suivi des modifications sur toutes les tables avant de le désactiver sur la base de données.

**\<cursor_option> ::=**      
Contrôle les options de curseur.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVÉ     
Les curseurs ouverts quand vous validez ou restaurez une transaction sont fermés.

OFF     
Les curseurs restent ouverts lorsqu'une transaction est validée. La restauration d'une transaction ferme tous les curseurs à l'exception de ceux définis avec la valeur INSENSITIVE ou STATIC.

Les paramètres de niveau connexion définis à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de CURSOR_CLOSE_ON_COMMIT. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui désactive l’option CURSOR_CLOSE_ON_COMMIT pour la session (valeur OFF) par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_cursor_close_on_commit_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsCloseCursorsOnCommitEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Le curseur n'est libéré implicitement qu'au moment de la déconnexion. Pour plus d’informations, voir [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Contrôle l'état de chiffrement de la base de données.

ENCRYPTION { ON | OFF }     
Spécifie si la base de données doit être chiffrée (ON) ou non chiffrée (OFF). Pour plus d’informations sur le chiffrement de base de données, voir [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) et [Transparent Data Encryption avec Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quand le chiffrement est activé au niveau de la base de données, tous les groupes de fichiers sont chiffrés. Tous les nouveaux groupes de fichiers héritent de la propriété chiffrée. Si des groupes de fichiers dans la base de données sont définis sur READ ONLY, l’opération de chiffrement de la base de données échoue.

Vous pouvez voir l’état de chiffrement de la base de données en utilisant la vue de gestion dynamique [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Contrôle si des mises à jour sont autorisées dans la base de données.

READ_ONLY     
Les utilisateurs peuvent lire des données dans la base de données mais ils n'ont pas le droit de les modifier.

> [!NOTE]
> Pour améliorer les performances des requêtes, mettez à jour les statistiques avant de définir une base de données à READ_ONLY. Si des statistiques supplémentaires sont nécessaires après qu'une base de données est définie à READ_ONLY, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des statistiques dans tempdb. Pour plus d’informations sur les statistiques pour une base de données en lecture seule, consultez [Statistiques](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de données est accessible aux opérations de lecture et d'écriture.

Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.

> [!NOTE]
> `SET { READ_ONLY | READ_WRITE }` est désactivé sur les bases de données fédérées [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)].

**\<db_user_access_option> ::=**      
Contrôle l'accès utilisateur à la base de données.

RESTRICTED_USER     
Autorise seulement les membres du rôle de base de données fixe `db_owner` et des rôles serveur fixes `dbcreator` et `sysadmin` à se connecter à la base de données, mais ne limite pas leur nombre. Toutes les connexions à la base de données sont déconnectées dans la plage de temps spécifiée par la clause d'arrêt de l'instruction ALTER DATABASE. Après que la base est passée à l'état RESTRICTED_USER, toute tentative de connexion par des utilisateurs non qualifiés est refusée. **RESTRICTED_USER** ne peut pas être modifié avec SQL Managed Instance.

MULTI_USER     
Tous les utilisateurs qui bénéficient des autorisations appropriées peuvent se connecter à la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `user_access` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `UserAccess` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Contrôle si les transactions sont validées de manière entièrement durable ou durable différée.

DISABLED     
Toutes les transactions suivant `SET DISABLED` sont entièrement durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

ALLOWED     
Toutes les transactions suivant `SET ALLOWED` sont soit entièrement durables, soit retardées durables, en fonction de l’option de durabilité définie dans l’instruction de validation ou de bloc atomique.

FORCED     
Toutes les transactions suivant `SET FORCED` sont retardées durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

**\<PARAMETERIZATION_option> ::=**      
Contrôle l'option de paramétrage.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Les requêtes sont paramétrables en fonction du comportement par défaut de la base de données.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paramètre toutes les requêtes de la base de données.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_parameterization_forced` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Contrôle si le Magasin des requêtes est activé dans cette base de données et contrôle la suppression du contenu du Magasin des requêtes.

ACTIVÉ     
Active le magasin des requêtes. ON est la valeur par défaut.

OFF     
Désactive le magasin des requêtes. 

> [!NOTE]  
> Le Magasin des requêtes ne peut pas être désactivé dans la base de données unique [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et le pool élastique. L’exécution de `ALTER DATABASE [database] SET QUERY_STORE = OFF` renverra l’avertissement `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR     
Supprime le contenu du magasin des requêtes.

OPERATION_MODE     
Décrit le mode de fonctionnement du magasin des requêtes. Les valeurs valides sont READ_ONLY et READ_WRITE. En mode READ_WRITE, le magasin des requêtes collecte et conserve les informations sur le plan de requête et les statistiques d’exécution. En mode READ_ONLY, les informations peuvent être lues à partir du Magasin des requêtes, mais les nouvelles informations ne sont pas ajoutées. Si l’espace alloué maximal du magasin des requêtes est épuisé, le mode d’opération du magasin des requêtes passe à READ_ONLY.

CLEANUP_POLICY     
Décrit la stratégie de conservation des données du magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS détermine le nombre de jours pendant lesquels les informations d’une requête sont conservées dans le magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS est de type **bigint**. La valeur par défaut est 30. Pour l’édition [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] De base, la valeur par défaut est **7** jours.

DATA_FLUSH_INTERVAL_SECONDS     
Détermine la fréquence à laquelle les données écrites dans le magasin des requêtes sont stockées sur le disque. Pour optimiser les performances, les données collectées par le magasin des requêtes sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée à l'aide de l'argument DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS est de type **bigint**. La valeur par défaut est **900** (15 minutes).

MAX_STORAGE_SIZE_MB     
Détermine l’espace alloué au magasin des requêtes. MAX_STORAGE_SIZE_MB est de type **bigint**. Pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition Premium, la valeur par défaut est de **1 Go** et pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition De base, elle est de **10 Mo**.

> [!NOTE]
> La limite du paramètre `MAX_STORAGE_SIZE_MB` est de 10 240 Mo sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

> [!NOTE]
> La limite `MAX_STORAGE_SIZE_MB` n’est pas strictement appliquée. La taille de stockage est vérifiée seulement quand le Magasin des requêtes écrit des données sur le disque. Cet intervalle est défini par l’option `DATA_FLUSH_INTERVAL_SECONDS` ou l’option de la boîte de dialogue Magasin des requêtes [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] **Intervalle de vidage des données**. La valeur par défaut de l’intervalle est 900 secondes (ou 15 minutes).
> Si le Magasin des requêtes a enfreint la limite `MAX_STORAGE_SIZE_MB` entre des vérifications de la taille de stockage, il passe en mode lecture seule. Si l’option `SIZE_BASED_CLEANUP_MODE` est activée, le mécanisme de nettoyage permettant d’appliquer la limite `MAX_STORAGE_SIZE_MB` est également déclenché.
> Une fois que suffisamment d’espace a été libéré, le mode Magasin des requêtes revient automatiquement en lecture-écriture.

> [!IMPORTANT]
> Si vous pensez que la capture de votre charge de travail nécessite plus de 10 Go d’espace disque, vous devez probablement repenser et optimiser votre charge de travail pour réutiliser les plans de requête (par exemple, en utilisant le [paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) ou ajuster les configurations du Magasin des requêtes.    
> À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez définir `QUERY_CAPTURE_MODE` sur CUSTOM pour disposer d’un contrôle supplémentaire sur la stratégie de capture des requêtes.

INTERVAL_LENGTH_MINUTES     
Détermine l’intervalle de temps auquel les données des statistiques d’exécution du runtime sont agrégées dans le magasin des requêtes. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Cette fenêtre de temps fixe est configurée à l'aide de l'argument INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES est de type **bigint**. La valeur par défaut est **60**.

SIZE_BASED_CLEANUP_MODE     
Contrôle si le nettoyage est activé automatiquement quand la quantité totale de données approche de la taille maximale.

OFF     
Le nettoyage basé sur la taille n’est pas activé automatiquement.

AUTO     
Le nettoyage basé sur la taille est activé automatiquement quand la taille sur le disque atteint 90 % de **max_storage_size_mb**. Le nettoyage basé sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête à environ 80 % de **max_storage_size_mb**. Il s’agit de la valeur de configuration par défaut.

SIZE_BASED_CLEANUP_MODE est de type **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Désigne le mode de capture de requête actif actuellement. Chaque mode définit des stratégies de capture de requête spécifiques.   

> [!NOTE]
> Les curseurs, les requêtes dans les procédures stockées et les requêtes compilées en mode natif sont toujours capturés lorsque le mode de capture de requête est défini sur All (Tous), Auto ou Custom (Personnalisé).

ALL     
Capture toutes les requêtes.

AUTO     
Capturer les requêtes pertinentes en fonction du nombre d’exécutions et de la consommation de ressources. Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Aucune     
Cesser la capture des nouvelles requêtes. Le Magasin des requêtes continue de collecter des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer la capture de requêtes importantes.

CUSTOM     
Permet de contrôler les options QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE est de type **nvarchar**.

max_plans_per_query     
Définit le nombre maximal de plans gérés pour chaque requête. MAX_PLANS_PER_QUERY est de type **int**. La valeur par défaut est **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Contrôle si les statistiques d’attente seront capturées par requête.

ACTIVÉ    
Les informations des statistiques d’attente par requête sont capturées. Il s’agit de la valeur de configuration par défaut.

OFF    
Les informations des statistiques d’attente par requête ne seront pas capturées.

**\<query_capture_policy_option_list> :: =**      
Contrôle les options de stratégie de capture du magasin des requêtes. Sauf pour STALE_CAPTURE_POLICY_THRESHOLD, ces options définissent dans la valeur de la durée de validité de la stratégie de capture les conditions OR qui doivent intervenir pour que les requêtes soient capturées.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Définit la période d’évaluation pour déterminer si une requête doit être capturée. La valeur par défaut est de 1 jour, les valeurs possibles oscillant entre 1 heure et sept jours. *number* est de type **int**.

EXECUTION_COUNT     
Définit le nombre d’exécutions d’une requête pendant la période d’évaluation. La valeur par défaut est 30, ce qui signifie que pour la durée de validité par défaut de la stratégie de capture, une requête doit s’exécuter au moins 30 fois dans la même journée pour devenir persistante dans le Magasin des requêtes. EXECUTION_COUNT est de type **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Définit le temps UC de compilation écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 1000 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins une seconde de temps processeur écoulé pendant la compilation de la requête dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_COMPILE_CPU_TIME_MS est de type **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Définit le temps UC d’exécution écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 100 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins 100 ms de temps processeur écoulé pendant l’exécution dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_EXECUTION_CPU_TIME_MS est de type **int**.

**\<snapshot_option> ::=**      
Détermine le niveau d'isolation de la transaction.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ACTIVÉ     
Active l'option Instantané au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions peuvent spécifier le niveau d'isolement des transactions SNAPSHOT. Quand une transaction s'exécute au niveau d'isolement SNAPSHOT, toutes les instructions voient un instantané des données tel qu'il existe au début de la transaction. Si une transaction exécutée au niveau d'isolement SNAPSHOT accède à des données dans plusieurs bases de données, l'option ALLOW_SNAPSHOT_ISOLATION doit avoir la valeur ON dans toutes les bases de données ou chaque instruction de la transaction doit utiliser des indicateurs de verrouillage sur toute référence d'une clause FROM à une table de la base de données dont l'option ALLOW_SNAPSHOT_ISOLATION a la valeur OFF.

OFF     
Désactive l'option Instantané au niveau de la base de données. Les transactions peuvent spécifier le niveau d’isolation de la transaction SNAPSHOT.

Lorsque vous modifiez l'état de ALLOW_SNAPSHOT_ISOLATION (de ON à OFF ou inversement), ALTER DATABASE ne retourne pas le contrôle à l'appelant tant que toutes les transactions existantes dans la base de données n'ont pas été validées. Si la base de données présente déjà l'état spécifié dans l'instruction ALTER DATABASE, le contrôle est immédiatement retourné à l'appelant. Si l’instruction ALTER DATABASE n’est pas retournée rapidement, utilisez [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) pour déterminer si certaines transactions sont de longue durée. Si l'instruction ALTER DATABASE est annulée, la base de données conserve l'état qu'elle présentait au démarrage de ALTER DATABASE. la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indique l’état des transactions d’isolement d’instantané dans la base de données. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF marque une pause de six secondes puis réessaie d’exécuter l’opération.

Vous ne pouvez pas modifier l’état de ALLOW_SNAPSHOT_ISOLATION si la base de données est hors connexion (OFFLINE).

Si vous configurez ALLOW_SNAPSHOT_ISOLATION dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé si la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Vous pouvez modifier les paramètres de ALLOW_SNAPSHOT_ISOLATION pour les bases de données master, model, msdb et tempdb. Le paramètre est conservé à chaque arrêt et redémarrage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] si vous changez le paramètre pour la base de données tempdb. Si vous modifiez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées à l'exception de tempdb.

Cette option a la valeur ON par défaut pour les bases de données master et msdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `snapshot_isolation_state` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ACTIVÉ     
Active l'option d'instantané de lecture validée au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions qui définissent le niveau d’isolation READ COMMITTED utilisent le versioning de ligne au lieu du verrouillage. Toutes les instructions voient un instantané des données telles qu’elles existent au début de l’instruction quand une transaction est exécutée au niveau d’isolation READ COMMITTED.

OFF     
Désactive l'option d'instantané de lecture validée au niveau de la base de données. Les transactions spécifiant le niveau d'isolation READ COMMITTED utilisent le verrouillage.

Pour définir READ_COMMITTED_SNAPSHOT sur ON ou sur OFF, il ne doit exister aucune connexion active à la base de données, à l’exception de la connexion exécutant la commande ALTER DATABASE. Toutefois, il n’est pas nécessaire que la base de données soit en mode mono-utilisateur. Vous ne pouvez pas modifier l’état de cette option si la base de données est hors connexion (OFFLINE).

Si vous configurez READ_COMMITTED_SNAPSHOT dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé lorsque la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Il n’est pas possible d’affecter la valeur ON à READ_COMMITTED_SNAPSHOT pour les bases de données système master, tempdb ou msdb. Si vous changez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées, à l’exception de tempdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_read_committed_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quand une table est créée avec `DURABILITY = SCHEMA_ONLY`, et **READ_COMMITTED_SNAPSHOT** est ensuite modifié à l’aide de `ALTER DATABASE`, les données de la table seront perdues.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ACTIVÉ     
Quand le niveau d’isolation de la transaction est défini sur un niveau inférieur à SNAPSHOT, toutes les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée sont exécutées avec l’isolation SNAPSHOT. Des niveaux d’isolation inférieurs à snapshot sont, par exemple, READ COMMITTED ou READ UNCOMMITTED. Ces opérations sont exécutées si le niveau d’isolation de la transaction est défini explicitement sur le niveau de la session, ou si la valeur par défaut est utilisée implicitement.

OFF     
N’élève pas le niveau d’isolation pour les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée.

Vous ne pouvez pas modifier l’état de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de données est hors connexion (OFFLINE).

La valeur par défaut est OFF.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_memory_optimized_elevate_to_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Contrôle les options de conformité ANSI au niveau de la base de données.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Détermine la valeur par défaut, NULL ou NOT NULL, d’une colonne, d’un [type CLR défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) dont le paramètre d’acceptation des valeurs NULL n’est pas défini de façon explicite dans les instructions CREATE TABLE ou ALTER TABLE. Les colonnes définies avec des contraintes respectent les règles de contrainte, quel que soit ce paramètre.

ACTIVÉ     
La valeur par défaut est NULL.

OFF     
La valeur par défaut est NOT NULL.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre défini pour ANSI_NULL_DEFAULT au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULL_DEFAULT pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Pour garantir la compatibilité ANSI, l'activation (ON) de l'option de base de données ANSI_NULL_DEFAULT entraîne la définition de NULL comme valeur par défaut de la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_null_default_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullDefault` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ACTIVÉ     
Toutes les comparaisons avec une valeur Null produisent le résultat UNKNOWN (inconnu).

OFF     
Les comparaisons de valeurs non-UNICODE avec une valeur Null génèrent la valeur TRUE si les deux valeurs sont Null.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut défini pour ANSI_NULLS au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULLS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!NOTE]
> SET ANSI_NULLS doit également avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_nulls_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVÉ     
Les chaînes sont complétées pour avoir la même longueur avant leur conversion. Elles sont également complétées pour avoir la même longueur avant leur insertion dans un type de données **varchar** ou **nvarchar**.

OFF     
Cette option insère des espaces à droite dans les valeurs de type character dans des colonnes **varchar** ou **nvarchar**. Cette option laisse également les zéros à droite dans les valeurs de type binary insérées dans des colonnes **varbinary**. Les valeurs ne sont pas complétées à concurrence de la longueur de la colonne.

Lorsque cette option a la valeur OFF, elle affecte uniquement la définition des nouvelles colonnes.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Il est recommandé de toujours affecter la valeur ON à l'option ANSI_PADDING. Par ailleurs, ANSI_PADDING doit avoir la valeur ON lorsque vous créez ou manipulez des index dans des colonnes calculées ou des vues indexées.

Les colonnes **char(_n_)** et **binary(_n_)** qui acceptent des valeurs NULL sont complétées à concurrence de la longueur de la colonne lorsque ANSI_PADDING a la valeur ON. Les espaces à droite et les zéros sont tronqués lorsque ANSI_PADDING a la valeur OFF. Les colonnes **char(_n_)** et **binary(_n_)** qui n’acceptent pas les valeurs NULL sont toujours complétées à concurrence de la longueur de la colonne.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre de ANSI_PADDING au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_PADDING pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_padding_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiPaddingEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVÉ     
Des erreurs ou avertissements sont émis si des conditions telles que « division par zéro » sont vérifiées. Des erreurs et avertissements sont également générés lorsque des valeurs Null apparaissent dans des fonctions d’agrégation.

OFF     
Aucun avertissement n'est généré et des valeurs Null sont retournées lorsque des conditions telles qu'une division par zéro se manifestent.

> [!NOTE]
> SET ANSI_WARNINGS doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de ANSI_NULLS défini au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_WARNINGS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_warnings_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiWarningsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVÉ     
Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.

OFF     
Un message d’avertissement s’affiche quand l’une de ces erreurs se produit. Le traitement de la requête, du lot ou de la transaction se poursuit comme s’il n’y avait pas d’erreur, même si un message d’avertissement s’affiche.

> [!NOTE]
> SET ARITHABORT doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_arithabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsArithmeticAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Pour plus d’informations, voir [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVÉ     
Le résultat d'une concaténation est NULL lorsque l'un des deux opérandes est NULL. Par exemple, la concaténation de la chaîne de caractères « Ceci est » et NULL donne la valeur NULL et non la valeur « Ceci est ».

OFF     
La valeur Null est considérée comme une chaîne de caractères vide.

> [!NOTE]
> CONCAT_NULL_YIELDS_NULL doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.
>
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de CONCAT_NULL_YIELDS_NULL défini au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à CONCAT_NULL_YIELDS_NULL pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_concat_null_yields_null_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNullConcat` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVÉ     
Des guillemets doubles peuvent être utilisés pour entourer des identificateurs délimités.

Toutes les chaînes délimitées par des guillemets doubles sont considérées comme des identificateurs d'objet. Les identificateurs entre guillemets n’ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] propres aux identificateurs. Ils peuvent être des mots clés et contenir des caractères interdits dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si un guillemet simple (') fait partie de la chaîne littérale, il pourra être représenté par un guillemet double ('').

OFF     
Les identificateurs ne peuvent pas être mis entre guillemets et doivent respecter toutes les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet également de délimiter les identificateurs par des crochets ([ ]). Les identificateurs entre crochets peuvent toujours être utilisés, quel que soit le paramètre QUOTED_IDENTIFIER. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

Lors de la création d’une table, l’option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table. L’option est stockée même si elle a la valeur OFF au moment de la création de la table.

Les paramètres définis au niveau de la connexion à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de QUOTED_IDENTIFIER. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à QUOTED_IDENTIFIER par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_quoted_identifier_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsQuotedIdentifiersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVÉ     
Une erreur est générée lors d'une perte de précision dans une expression.

OFF     
La perte de précision ne génère pas de message d’erreur et le résultat est arrondi en fonction de la précision de la colonne ou de la variable stockant le résultat.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT doit avoir la valeur OFF lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_numeric_roundabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNumericRoundAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVÉ     
L'activation récursive des déclencheurs AFTER est autorisée.

OFF     
Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Seule la récursivité directe est désactivée lorsque RECURSIVE_TRIGGERS a la valeur OFF. Pour désactiver la récursivité indirecte, vous devez aussi affecter la valeur 0 à l'option serveur nested triggers.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Spécifie la fréquence des points de contrôle indirects en fonction de chaque base de données. À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la valeur par défaut pour les nouvelles bases de données est une minute, ce qui signifie que la base de données utilise les points de contrôle indirects. Pour les versions antérieures, la valeur par défaut est 0, ce qui indique que la base de données utilise les points de contrôle automatiques, dont la fréquence dépend du paramètre d’intervalle de récupération de l’instance de serveur. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande une valeur d’une minute pour la plupart des systèmes.

TARGET_RECOVERY_TIME **=** objectif_temps_récupération { SECONDS | MINUTES }     
*target_recovery_time*     
Spécifie la limite maximale de durée de récupération de la base de données spécifiée en cas de sinistre.

SECONDS     
Indique que *target_recovery_time* correspond au nombre de secondes.

MINUTES     
Indique que *target_recovery_time* correspond au nombre de minutes.

Pour plus d’informations sur les points de contrôle indirects, voir [Points de contrôle de base de données](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Spécifie le(s) cas où une transaction incomplète doit être restaurée lors d'un changement d'état de la base de données. Lorsque la clause de fin est omise, l’instruction ALTER DATABASE attend indéfiniment s’il existe un verrou quelconque sur la base de données. Une seule clause de fin peut être spécifiée, à la suite des clauses SET.

> [!NOTE]
> Toutes les options de base de données n’utilisent pas la clause WITH \<termination>. Pour plus d’informations, consultez le tableau sous « [Options de configuration](#SettingOptions) » dans la section « Remarques » de cet article.

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Indique si la restauration intervient après le nombre de secondes spécifié ou immédiatement.

NO_WAIT     
Spécifie que la requête échoue si le changement d’état ou d’option de base de données demandé ne peut pas être effectué immédiatement. Une exécution immédiate signifie ne pas attendre la validation ou la restauration des transactions.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuration des options

Pour récupérer les paramètres actuels des options de base de données, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Quand vous définissez une option de base de données, la nouvelle valeur prend effet immédiatement.

Vous pouvez modifier les valeurs par défaut de l’une des options de base de données afin qu’elles s’appliquent à toutes les nouvelles bases de données créées. Pour ce faire, modifiez l’option de base de données appropriée dans la base de données model.

Toutes les options de base de données n’utilisent pas la clause WITH \<termination> et ne peuvent pas être combinées avec d’autres options. Le tableau suivant répertorie ces options ainsi que l'état de l'option et d'arrêt.

|Catégorie d'options|Peut être spécifiée avec d'autres options|Peut utiliser la clause WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<auto_option>|Oui|Non|
|\<change_tracking_option>|Oui|Oui|
|\<cursor_option>|Oui|Non|
|\<db_encryption_option>|Oui|Non|
|\<db_update_option>|Oui|Oui|
|\<db_user_access_option>|Oui|Oui|
|\<delayed_durability_option>|Oui|Oui|
|\<parameterization_option>|Oui|Oui|
|ALLOW_SNAPSHOT_ISOLATION|Non|Non|
|READ_COMMITTED_SNAPSHOT|Non|Oui|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Oui|Oui|
|DATE_CORRELATION_OPTIMIZATION|Oui|Oui|
|\<sql_option>|Oui|Non|
|\<target_recovery_time_option>|Non|Oui|

## <a name="examples"></a>Exemples

### <a name="a-setting-the-database-to-read_only"></a>R. Paramétrage de la base de données avec READ_ONLY
Pour modifier l’état d’une base de données ou d’un groupe de fichiers en READ_ONLY ou en READ_WRITE, vous avez besoin d’un accès exclusif à la base de données. L’exemple suivant illustre le basculement de la base de données en mode `RESTRICTED_USER` pour limiter l’accès. L'exemple affecte ensuite à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] l'état `READ_ONLY` et rend à tous les utilisateurs l'accès à la base de données.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO

```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Activation de l'isolement d'instantané sur une base de données
L'exemple ci-dessous illustre l'activation de l'option d'infrastructure d'isolement d'instantané pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Le jeu de résultats montre que l'infrastructure d'isolement d'instantané est activée.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[nom_base_de_données] |1| ACTIVÉ |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Activation, modification et désactivation du suivi des modifications
L'exemple ci-dessous illustre l'activation du suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et la définition d'une période de rétention de `2` jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

L’exemple suivant montre comment changer la période de conservation en 3 jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

L'exemple ci-dessous illustre comment désactiver le suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Activation du magasin de requêtes
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Activation du magasin des requêtes avec les statistiques d’attente
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Activation du magasin des requêtes avec les options de stratégie de capture personnalisée
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Voir aussi

- [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistiques](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Activer et désactiver le suivi des modifications](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bonnes pratiques relatives au Magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Bien que les niveaux de compatibilité soient des options `SET`, ils sont décrits dans [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> La plupart des options SET de base de données sont configurables pour la session en cours avec les [Instructions SET](../../t-sql/statements/set-statements-transact-sql.md), souvent par des applications au moment de la connexion. Les options SET de niveau session remplacent les valeurs **ALTER DATABASE SET**. Les options de base de données décrites dans les sections suivantes sont des valeurs que vous pouvez définir pour les sessions qui ne fournissent pas explicitement d’autres valeurs pour les options SET.

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <optionspec> [ ,...n ]
}
;

<optionspec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;
<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<delayed_durability_option> ::=DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
    = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<temporal_history_retention>::= TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Arguments

*database_name*     
Nom de la base de données à modifier.

CURRENT     
`CURRENT` exécute l’action dans la base de données active. `CURRENT` n’est pas pris en charge pour toutes les options dans tous les contextes. Si `CURRENT` échoue, fournissez le nom de la base de données.

**\<auto_option> ::=**      
Contrôle les options automatiques.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ACTIVÉ     
L’optimiseur de requête crée si nécessaire des statistiques sur les colonnes uniques des prédicats de requête, afin d’améliorer les plans de requête et les performances des requêtes. Ces statistiques de colonnes uniques sont créées quand l’optimiseur de requête compile les requêtes. Les statistiques de colonnes uniques sont créées uniquement sur les colonnes qui ne constituent pas déjà la première colonne d'un objet de statistiques existant.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

OFF     
L’optimiseur de requête ne crée pas de statistiques sur les colonnes uniques des prédicats de requête quand il compile les requêtes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_create_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoCreateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Options des statistiques » dans [Statistiques](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Définissez AUTO_CREATE_STATISTICS sur la valeur ON, et INCREMENTAL sur la valeur ON. Ce paramètre crée automatiquement des statistiques incrémentielles sur les statistiques incrémentielles sont prises en charge. La valeur par défaut est OFF. Pour plus d’informations, voir [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVÉ     
Les fichiers de base de données peuvent faire l'objet d'une réduction périodique.

Les fichiers de données et les fichiers journaux peuvent être automatiquement réduits. AUTO_SHRINK ne réduit la taille du journal des transactions que si vous définissez la base de données sur le mode de récupération SIMPLE ou si vous sauvegardez le journal. Si la valeur spécifiée est OFF, les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l’espace inutilisé.

L'option AUTO_SHRINK provoque un compactage dès qu'un fichier comprend plus de 25 % d'espace inutilisé. L’option provoque une réduction du fichier à l’une de deux tailles. Il est réduit à la taille plus grande retenue :

- La taille où 25 pour cent du fichier correspond à l’espace inutilisé
- La taille du fichier quand il a été créé

Vous ne pouvez pas réduire une base de données en lecture seule.

OFF     
Les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l'espace inutilisé.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_auto_shrink_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoShrink` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> L’option AUTO_SHRINK n’est pas disponible dans une base de données autonome.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVÉ     
Spécifie que l’optimiseur de requête met à jour les statistiques quand elles sont utilisées par une requête et quand elles sont susceptibles d’être obsolètes. Les statistiques deviennent obsolètes après que des opérations d'insertion, de mise à jour, de suppression ou de fusion ont modifié la distribution des données dans la table ou la vue indexée. L’optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.

L’optimiseur de requête vérifie s’il existe des statistiques obsolètes avant de compiler une requête et d’exécuter un plan de requête mis en cache. L’optimiseur de requête utilise les colonnes, les tables et les vues indexées du prédicat de requête pour identifier les statistiques susceptibles d’être obsolètes. L’optimiseur de requête détermine ces informations avant de compiler une requête. Avant d’exécuter un plan de requête mis en cache, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que le plan de requête référence des statistiques à jour.

L'option AUTO_UPDATE_STATISTICS s'applique aux statistiques créées pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l'aide de l'instruction CREATE STATISTICS. Cette option s'applique également aux statistiques filtrées.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

Utilisez l'option AUTO_UPDATE_STATISTICS_ASYNC pour spécifier si les statistiques doivent être mises à jour en mode synchrone ou asynchrone.

OFF     
Spécifie que l’optimiseur de requête ne met pas à jour les statistiques quand elles sont utilisées par une requête. L’optimiseur de requête ne met pas non plus à jour les statistiques quand elles sont susceptibles d’être obsolètes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

Vous pouvez déterminer l’état de cette option en consultant la colonne is_auto_update_stats_on de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoUpdateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVÉ     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont asynchrones. L’optimiseur de requête n’attend pas la fin des mises à jour des statistiques pour compiler les requêtes.

Affecter la valeur ON à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Par défaut, l’option AUTO_UPDATE_STATISTICS_ASYNC est définie sur OFF ; l’optimiseur de requête met à jour les statistiques en mode synchrone.

OFF     
Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont synchrones. L’optimiseur de requête attend la fin des mises à jour des statistiques pour compiler les requêtes.

Affecter la valeur OFF à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.

Vous pouvez déterminer l’état de cette option en consultant la colonne is_auto_update_stats_async_on de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Pour plus d’informations sur l’utilisation des mises à jour de statistiques synchrones ou asynchrones, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**S’applique à** : [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]

Active ou désactive l’option `FORCE_LAST_GOOD_PLAN` [Optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ACTIVÉ     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] force automatiquement le dernier plan correct connu sur les requêtes [!INCLUDE[tsql-md](../../includes/tsql-md.md)] là où le nouveau plan de requête provoque des régressions des performances. Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervise en permanence les performances de la requête [!INCLUDE[tsql-md](../../includes/tsql-md.md)] avec le plan forcé. S’il existe des gains de performances, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continue à utiliser le dernier plan correct connu. Si aucun gain de performances n’est détecté, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] génère un nouveau plan de requête. L’instruction échoue si le Magasin des requêtes n’est pas activé ou s’il n’est pas en mode *Lecture-écriture*.

OFF     
Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] indique les régressions des performances de requêtes potentielles dues à des changements de plan de requête dans la vue [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). Toutefois, ces recommandations ne sont pas appliquées automatiquement. Les utilisateurs peuvent superviser les recommandations actives et résoudre les problèmes identifiés en appliquant les scripts [!INCLUDE[tsql-md](../../includes/tsql-md.md)] qui sont montrés dans la vue. Il s’agit de la valeur par défaut.

**\<change_tracking_option> ::=**      
Contrôle les options de suivi des modifications. Vous pouvez activer le suivi des modifications, définir des options, modifier des options et désactiver le suivi des modifications. Pour des exemples, consultez la section « Exemples » plus loin dans cet article.

ON active le suivi des modifications pour la base de données. Lorsque vous activez le suivi des modifications, vous pouvez également définir les options AUTO CLEANUP et CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ACTIVÉ     
Les informations de suivi des modifications sont supprimées automatiquement à l'issue de la période de rétention spécifiée.

OFF     
Les données de suivi des modifications ne sont pas supprimées de la base de données.

CHANGE_RETENTION = *période_conservation* { **DAYS** | HOURS | MINUTES }     
Spécifie la période minimale de conservation des informations de suivi des modifications dans la base de données. Les données sont supprimées uniquement lorsque AUTO_CLEANUP a la valeur ON.

*retention_period* est un entier qui spécifie la composante numérique de la période de rétention.

La période de conservation par défaut est **2 jours**. La période de rétention minimale est 1 minute. Le type de conservation par défaut est **DAYS**.

OFF     
Désactive le suivi des modifications pour la base de données. Désactivez le suivi des modifications sur toutes les tables avant de le désactiver sur la base de données.

**\<cursor_option> ::=**      
Contrôle les options de curseur.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVÉ     
Les curseurs ouverts quand vous validez ou restaurez une transaction sont fermés.

OFF     
Les curseurs restent ouverts lorsqu'une transaction est validée. La restauration d'une transaction ferme tous les curseurs à l'exception de ceux définis avec la valeur INSENSITIVE ou STATIC.

Les paramètres de niveau connexion définis à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de CURSOR_CLOSE_ON_COMMIT. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui désactive l’option CURSOR_CLOSE_ON_COMMIT pour la session (valeur OFF) par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne is_cursor_close_on_commit_on de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété IsCloseCursorsOnCommitEnabled de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Le curseur n'est libéré implicitement qu'au moment de la déconnexion. Pour plus d’informations, voir [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Contrôle l'état de chiffrement de la base de données.

ENCRYPTION { ON | **OFF** }     
Spécifie si la base de données doit être chiffrée (ON) ou non chiffrée (OFF). Pour plus d’informations sur le chiffrement de base de données, voir [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) et [Transparent Data Encryption avec Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Quand le chiffrement est activé au niveau de la base de données, tous les groupes de fichiers sont chiffrés. Tous les nouveaux groupes de fichiers héritent de la propriété chiffrée. Si des groupes de fichiers dans la base de données sont définis sur READ ONLY, l’opération de chiffrement de la base de données échoue.

Vous pouvez voir l’état de chiffrement de la base de données en utilisant la vue de gestion dynamique [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Contrôle si des mises à jour sont autorisées dans la base de données.

READ_ONLY     
Les utilisateurs peuvent lire des données dans la base de données mais ils n'ont pas le droit de les modifier.

> [!NOTE]
> Pour améliorer les performances des requêtes, mettez à jour les statistiques avant de définir une base de données à READ_ONLY. Si des statistiques supplémentaires sont nécessaires après qu'une base de données est définie à READ_ONLY, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des statistiques dans tempdb. Pour plus d’informations sur les statistiques pour une base de données en lecture seule, consultez [Statistiques](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de données est accessible aux opérations de lecture et d'écriture.

Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données.

**\<db_user_access_option> ::=**      
Contrôle l'accès utilisateur à la base de données.

RESTRICTED_USER     
Autorise seulement les membres du rôle de base de données fixe `db_owner` et des rôles serveur fixes `dbcreator` et `sysadmin` à se connecter à la base de données, mais ne limite pas leur nombre. Toutes les connexions à la base de données sont déconnectées dans la plage de temps spécifiée par la clause d'arrêt de l'instruction ALTER DATABASE. Après que la base est passée à l'état RESTRICTED_USER, toute tentative de connexion par des utilisateurs non qualifiés est refusée. **RESTRICTED_USER** ne peut pas être modifié avec SQL Managed Instance.

MULTI_USER     
Tous les utilisateurs qui bénéficient des autorisations appropriées peuvent se connecter à la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne user_access de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `UserAccess` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Contrôle si les transactions sont validées de manière entièrement durable ou durable différée.

DISABLED     
Toutes les transactions suivant `SET DISABLED` sont entièrement durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

ALLOWED     
Toutes les transactions suivant `SET ALLOWED` sont soit entièrement durables, soit retardées durables, en fonction de l’option de durabilité définie dans l’instruction de validation ou de bloc atomique.

FORCED     
Toutes les transactions suivant `SET FORCED` sont retardées durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.

**\<PARAMETERIZATION_option> ::=**      
Contrôle l'option de paramétrage.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Les requêtes sont paramétrables en fonction du comportement par défaut de la base de données.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paramètre toutes les requêtes de la base de données.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_parameterization_forced` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Contrôle si le Magasin des requêtes est activé dans cette base de données et contrôle la suppression du contenu du Magasin des requêtes.

ACTIVÉ     
Active le magasin des requêtes.

OFF     
Désactive le magasin des requêtes. Il s’agit de la valeur par défaut.

CLEAR     
Supprime le contenu du magasin des requêtes.

OPERATION_MODE     
Décrit le mode de fonctionnement du magasin des requêtes. Les valeurs valides sont READ_ONLY et READ_WRITE. En mode READ_WRITE, le magasin des requêtes collecte et conserve les informations sur le plan de requête et les statistiques d’exécution. En mode READ_ONLY, les informations peuvent être lues à partir du Magasin des requêtes, mais les nouvelles informations ne sont pas ajoutées. Si l’espace alloué maximal du magasin des requêtes est épuisé, le mode d’opération du magasin des requêtes passe à READ_ONLY.

CLEANUP_POLICY     
Décrit la stratégie de conservation des données du magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS détermine le nombre de jours pendant lesquels les informations d’une requête sont conservées dans le magasin des requêtes. STALE_QUERY_THRESHOLD_DAYS est de type **bigint**. La valeur par défaut est 30. Pour l’édition [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] De base, la valeur par défaut est **7** jours.

DATA_FLUSH_INTERVAL_SECONDS     
Détermine la fréquence à laquelle les données écrites dans le magasin des requêtes sont stockées sur le disque. Pour optimiser les performances, les données collectées par le magasin des requêtes sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée à l'aide de l'argument DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS est de type **bigint**. La valeur par défaut est **900** (15 minutes).

MAX_STORAGE_SIZE_MB     
Détermine l’espace alloué au magasin des requêtes. MAX_STORAGE_SIZE_MB est de type **bigint**. La valeur par défaut est **100 Mo**.

> [!NOTE]
> La limite `MAX_STORAGE_SIZE_MB` n’est pas strictement appliquée. La taille de stockage est vérifiée seulement quand le Magasin des requêtes écrit des données sur le disque. Cet intervalle est défini par l’option `DATA_FLUSH_INTERVAL_SECONDS` ou l’option de la boîte de dialogue Magasin des requêtes [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] **Intervalle de vidage des données**. La valeur par défaut de l’intervalle est 900 secondes (ou 15 minutes).
> Si le Magasin des requêtes a enfreint la limite `MAX_STORAGE_SIZE_MB` entre des vérifications de la taille de stockage, il passe en mode lecture seule. Si l’option `SIZE_BASED_CLEANUP_MODE` est activée, le mécanisme de nettoyage permettant d’appliquer la limite `MAX_STORAGE_SIZE_MB` est également déclenché.
> Une fois que suffisamment d’espace a été libéré, le mode Magasin des requêtes revient automatiquement en lecture-écriture.

> [!IMPORTANT]
> Si vous pensez que la capture de votre charge de travail nécessite plus de 10 Go d’espace disque, vous devez probablement repenser et optimiser votre charge de travail pour réutiliser les plans de requête (par exemple, en utilisant le [paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) ou ajuster les configurations du Magasin des requêtes.    
> À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez définir `QUERY_CAPTURE_MODE` sur CUSTOM pour disposer d’un contrôle supplémentaire sur la stratégie de capture des requêtes.

INTERVAL_LENGTH_MINUTES     
Détermine l’intervalle de temps auquel les données des statistiques d’exécution du runtime sont agrégées dans le magasin des requêtes. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Cette fenêtre de temps fixe est configurée à l'aide de l'argument INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES est de type **bigint**. La valeur par défaut est **60**.

SIZE_BASED_CLEANUP_MODE     
Contrôle si le nettoyage est activé automatiquement quand la quantité totale de données approche de la taille maximale.

OFF     
Le nettoyage basé sur la taille n’est pas activé automatiquement.

AUTO     
Le nettoyage basé sur la taille est activé automatiquement quand la taille sur le disque atteint 90 % de **max_storage_size_mb**. Le nettoyage basé sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête à environ 80 % de **max_storage_size_mb**. Il s’agit de la valeur de configuration par défaut.

SIZE_BASED_CLEANUP_MODE est de type **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Désigne le mode de capture de requête actif actuellement.

ALL     
Toutes les requêtes sont capturées.

AUTO     
Capturer les requêtes pertinentes en fonction du nombre d’exécutions et de la consommation de ressources. Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Aucune     
Cesser la capture des nouvelles requêtes. Le Magasin des requêtes continue de collecter des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer la capture de requêtes importantes.

QUERY_CAPTURE_MODE est de type **nvarchar**.

max_plans_per_query     
Entier représentant le nombre maximal de plans gérés pour chaque requête. MAX_PLANS_PER_QUERY est de type **int**. La valeur par défaut est **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Contrôle si les statistiques d’attente seront capturées par requête.

ACTIVÉ    
Les informations des statistiques d’attente par requête sont capturées. Il s’agit de la valeur de configuration par défaut.

OFF    
Les informations des statistiques d’attente par requête ne seront pas capturées.

**\<query_capture_policy_option_list> :: =**      
Contrôle les options de stratégie de capture du magasin des requêtes. Sauf pour STALE_CAPTURE_POLICY_THRESHOLD, ces options définissent dans la valeur de la durée de validité de la stratégie de capture les conditions OR qui doivent intervenir pour que les requêtes soient capturées.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Définit la période d’évaluation pour déterminer si une requête doit être capturée. La valeur par défaut est de 1 jour, les valeurs possibles oscillant entre 1 heure et sept jours. *number* est de type **int**.

EXECUTION_COUNT     
Définit le nombre d’exécutions d’une requête pendant la période d’évaluation. La valeur par défaut est 30, ce qui signifie que pour la durée de validité par défaut de la stratégie de capture, une requête doit s’exécuter au moins 30 fois dans la même journée pour devenir persistante dans le Magasin des requêtes. EXECUTION_COUNT est de type **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Définit le temps UC de compilation écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 1000 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins une seconde de temps processeur écoulé pendant la compilation de la requête dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_COMPILE_CPU_TIME_MS est de type **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Définit le temps UC d’exécution écoulé total utilisé par une requête pendant la période d’évaluation. La valeur par défaut est 100 ; ainsi, pour la durée de validité par défaut de la stratégie de capture, une requête doit avoir un total d’au moins 100 ms de temps processeur écoulé pendant l’exécution dans une même journée pour devenir persistante dans le Magasin des requêtes. TOTAL_EXECUTION_CPU_TIME_MS est de type **int**.

**\<snapshot_option> ::=**

Détermine le niveau d'isolation de la transaction.

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ACTIVÉ     
Active l’option Instantané au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions peuvent spécifier le niveau d’isolement des transactions SNAPSHOT. Quand une transaction s'exécute au niveau d'isolement SNAPSHOT, toutes les instructions voient un instantané des données tel qu'il existe au début de la transaction. Si une transaction exécutée au niveau d'isolement SNAPSHOT accède à des données dans plusieurs bases de données, l'option ALLOW_SNAPSHOT_ISOLATION doit avoir la valeur ON dans toutes les bases de données ou chaque instruction de la transaction doit utiliser des indicateurs de verrouillage sur toute référence d'une clause FROM à une table de la base de données dont l'option ALLOW_SNAPSHOT_ISOLATION a la valeur OFF.

OFF     
Désactive l'option Instantané au niveau de la base de données. Les transactions peuvent spécifier le niveau d’isolation de la transaction SNAPSHOT.

Lorsque vous modifiez l'état de ALLOW_SNAPSHOT_ISOLATION (de ON à OFF ou inversement), ALTER DATABASE ne retourne pas le contrôle à l'appelant tant que toutes les transactions existantes dans la base de données n'ont pas été validées. Si la base de données présente déjà l'état spécifié dans l'instruction ALTER DATABASE, le contrôle est immédiatement retourné à l'appelant. Si l’instruction ALTER DATABASE n’est pas retournée rapidement, utilisez [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) pour déterminer si certaines transactions sont de longue durée. Si l'instruction ALTER DATABASE est annulée, la base de données conserve l'état qu'elle présentait au démarrage de ALTER DATABASE. la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indique l’état des transactions d’isolement d’instantané dans la base de données. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF marque une pause de six secondes puis réessaie d’exécuter l’opération.

Vous ne pouvez pas modifier l’état de ALLOW_SNAPSHOT_ISOLATION si la base de données est hors connexion (OFFLINE).

Si vous configurez ALLOW_SNAPSHOT_ISOLATION dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé si la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Vous pouvez modifier les paramètres de ALLOW_SNAPSHOT_ISOLATION pour les bases de données master, model, msdb et tempdb. Le paramètre est conservé à chaque arrêt et redémarrage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] si vous changez le paramètre pour la base de données tempdb. Si vous modifiez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées à l'exception de tempdb.

Cette option a la valeur ON par défaut pour les bases de données master et msdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `snapshot_isolation_state` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | **OFF** }     
ACTIVÉ     
Active l’option Instantané Read-Committed au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n’utilise l’isolement d’instantané. Une fois que cette option est activée, les transactions spécifiant le niveau d’isolement READ COMMITTED utilisent le versioning de ligne au lieu du verrouillage. Toutes les instructions voient un instantané des données telles qu’elles existent au début de l’instruction quand une transaction est exécutée au niveau d’isolation READ COMMITTED.

OFF     
Désactive l’option Instantané Read-Committed au niveau de la base de données. Les transactions spécifiant le niveau d'isolation READ COMMITTED utilisent le verrouillage.

Pour définir READ_COMMITTED_SNAPSHOT sur ON ou sur OFF, il ne doit exister aucune connexion active à la base de données, à l’exception de la connexion exécutant la commande ALTER DATABASE. Toutefois, il n’est pas nécessaire que la base de données soit en mode mono-utilisateur. Vous ne pouvez pas modifier l’état de cette option si la base de données est hors connexion (OFFLINE).

Si vous configurez READ_COMMITTED_SNAPSHOT dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé lorsque la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).

Il n’est pas possible d’affecter la valeur ON à READ_COMMITTED_SNAPSHOT pour les bases de données système master, tempdb ou msdb. Si vous changez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées, à l’exception de tempdb.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_read_committed_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Quand une table est créée avec **DURABILITY = SCHEMA_ONLY** et que **READ_COMMITTED_SNAPSHOT** est changé par la suite à l’aide d’**ALTER DATABASE**, les données de la table sont perdues.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ACTIVÉ     
Quand le niveau d’isolation de la transaction est défini sur un niveau inférieur à SNAPSHOT, toutes les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée sont exécutées avec l’isolation SNAPSHOT. Des niveaux d’isolation inférieurs à snapshot sont, par exemple, READ COMMITTED ou READ UNCOMMITTED. Ces opérations sont exécutées si le niveau d’isolation de la transaction est défini explicitement sur le niveau de la session, ou si la valeur par défaut est utilisée implicitement.

OFF     
N’élève pas le niveau d’isolation pour les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables à mémoire optimisée.

Vous ne pouvez pas modifier l’état de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de données est hors connexion (OFFLINE).

La valeur par défaut est OFF.

La valeur actuelle de cette option peut être déterminée en examinant la colonne `is_memory_optimized_elevate_to_snapshot_on` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Contrôle les options de conformité ANSI au niveau de la base de données.

ANSI_NULL_DEFAULT { ON | OFF }     
Détermine la valeur par défaut, NULL ou NOT NULL, d’une colonne, d’un [type CLR défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) dont le paramètre d’acceptation des valeurs NULL n’est pas défini de façon explicite dans les instructions CREATE TABLE ou ALTER TABLE. Les colonnes définies avec des contraintes respectent les règles de contrainte, quel que soit ce paramètre.

ACTIVÉ     
La valeur par défaut est NULL.

OFF     
La valeur par défaut est NOT NULL.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre défini pour ANSI_NULL_DEFAULT au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULL_DEFAULT pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Pour garantir la compatibilité ANSI, l'activation (ON) de l'option de base de données ANSI_NULL_DEFAULT entraîne la définition de NULL comme valeur par défaut de la base de données.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_null_default_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullDefault` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | OFF }     
ACTIVÉ     
Toutes les comparaisons avec une valeur Null produisent le résultat UNKNOWN (inconnu).

OFF     
Les comparaisons de valeurs non-UNICODE avec une valeur Null génèrent la valeur TRUE si les deux valeurs sont Null.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

  Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut défini pour ANSI_NULLS au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULLS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> SET ANSI_NULLS doit également avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_nulls_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiNullsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVÉ     
Les chaînes sont complétées pour avoir la même longueur avant leur conversion. Elles sont également complétées pour avoir la même longueur avant leur insertion dans un type de données **varchar** ou **nvarchar**.

OFF     
Cette option insère des espaces à droite dans les valeurs de type character dans des colonnes **varchar** ou **nvarchar**. Cette option laisse également les zéros à droite dans les valeurs de type binary insérées dans des colonnes **varbinary**. Les valeurs ne sont pas complétées à concurrence de la longueur de la colonne.

Lorsque cette option a la valeur OFF, elle affecte uniquement la définition des nouvelles colonnes.

> [!IMPORTANT]
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Il est recommandé de toujours affecter la valeur ON à l'option ANSI_PADDING. Par ailleurs, ANSI_PADDING doit avoir la valeur ON lorsque vous créez ou manipulez des index dans des colonnes calculées ou des vues indexées.

Les colonnes **char(_n_)** et **binary(_n_)** qui acceptent des valeurs NULL sont complétées à concurrence de la longueur de la colonne lorsque ANSI_PADDING a la valeur ON. Les espaces à droite et les zéros sont tronqués lorsque ANSI_PADDING a la valeur OFF. Les colonnes **char(_n_)** et **binary(_n_)** qui n’acceptent pas les valeurs NULL sont toujours complétées à concurrence de la longueur de la colonne.

  Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre de ANSI_PADDING au niveau de la base de données par défaut. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_PADDING pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_padding_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiPaddingEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVÉ     
Des erreurs ou avertissements sont émis si des conditions telles que « division par zéro » sont vérifiées. Des erreurs et avertissements sont également générés lorsque des valeurs Null apparaissent dans des fonctions d’agrégation.

OFF     
Aucun avertissement n'est généré et des valeurs Null sont retournées lorsque des conditions telles qu'une division par zéro se manifestent.

> [!IMPORTANT]
> SET ANSI_WARNINGS doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

  Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de ANSI_NULLS défini au niveau de la base de données. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_WARNINGS pour la session par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_ansi_warnings_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAnsiWarningsEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVÉ     
Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.

OFF     
Un message d’avertissement s’affiche quand l’une de ces erreurs se produit. Le traitement de la requête, du lot ou de la transaction se poursuit comme s’il n’y avait pas d’erreur, même si un message d’avertissement s’affiche.

> [!IMPORTANT]
> SET ARITHABORT doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

  Vous pouvez déterminer l’état de cette option en consultant la colonne `is_arithabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsArithmeticAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Pour plus d’informations, voir [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVÉ     
Le résultat d'une concaténation est NULL lorsque l'un des deux opérandes est NULL. Par exemple, la concaténation de la chaîne de caractères « Ceci est » et NULL donne la valeur NULL et non la valeur « Ceci est ».

OFF     
La valeur Null est considérée comme une chaîne de caractères vide.

> [!IMPORTANT]
> CONCAT_NULL_YIELDS_NULL doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.
>
> Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de CONCAT_NULL_YIELDS_NULL défini au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à CONCAT_NULL_YIELDS_NULL pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_concat_null_yields_null_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNullConcat` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVÉ     
Des guillemets doubles peuvent être utilisés pour entourer des identificateurs délimités.

Toutes les chaînes délimitées par des guillemets doubles sont considérées comme des identificateurs d'objet. Les identificateurs entre guillemets n’ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] propres aux identificateurs. Ils peuvent être des mots clés et contenir des caractères interdits dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si un guillemet simple (') fait partie de la chaîne littérale, il pourra être représenté par un guillemet double ('').

OFF     
Les identificateurs ne peuvent pas être mis entre guillemets et doivent respecter toutes les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles.

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet également de délimiter les identificateurs par des crochets ([ ]). Les identificateurs entre crochets peuvent toujours être utilisés, quel que soit le paramètre QUOTED_IDENTIFIER. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

  Lors de la création d’une table, l’option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table. L’option est stockée même si elle a la valeur OFF au moment de la création de la table.

Les paramètres définis au niveau de la connexion à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de QUOTED_IDENTIFIER. Les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à QUOTED_IDENTIFIER par défaut. Les clients exécutent l’instruction lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

  Vous pouvez déterminer l’état de cette option en consultant la colonne `is_quoted_identifier_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsQuotedIdentifiersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVÉ     
Une erreur est générée lors d'une perte de précision dans une expression.

OFF     
La perte de précision ne génère pas de message d’erreur et le résultat est arrondi en fonction de la précision de la colonne ou de la variable stockant le résultat.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT doit avoir la valeur OFF lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_numeric_roundabort_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsNumericRoundAbortEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVÉ     
L'activation récursive des déclencheurs AFTER est autorisée.

OFF     
Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> Seule la récursivité directe est désactivée lorsque RECURSIVE_TRIGGERS a la valeur OFF. Pour désactiver la récursivité indirecte, vous devez aussi affecter la valeur 0 à l'option serveur nested triggers.

Vous pouvez déterminer l’état de cette option en consultant la colonne `is_recursive_triggers_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété `IsRecursiveTriggersEnabled` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Spécifie la fréquence des points de contrôle indirects en fonction de chaque base de données. À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la valeur par défaut pour les nouvelles bases de données est **1 minute**, ce qui signifie que la base de données utilise des points de contrôle indirects. Pour les versions antérieures, la valeur par défaut est 0, ce qui indique que la base de données utilise les points de contrôle automatiques, dont la fréquence dépend du paramètre d’intervalle de récupération de l’instance de serveur. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande une valeur d’une minute pour la plupart des systèmes.

TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }     
*target_recovery_time*     
Spécifie la limite maximale de durée de récupération de la base de données spécifiée en cas de sinistre.

SECONDS     
Indique que *target_recovery_time* correspond au nombre de secondes.

MINUTES     
Indique que *target_recovery_time* correspond au nombre de minutes.

Pour plus d’informations sur les points de contrôle indirects, voir [Points de contrôle de base de données](../../relational-databases/logs/database-checkpoints-sql-server.md).

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Indique si la restauration intervient après le nombre de secondes spécifié ou immédiatement.

NO_WAIT     
Spécifie que la requête échoue si le changement d’état ou d’option de base de données demandé ne peut pas être effectué immédiatement. Une exécution immédiate signifie ne pas attendre la validation ou la restauration des transactions.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuration des options
Pour récupérer les paramètres actuels des options de base de données, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Quand vous définissez une option de base de données, la nouvelle valeur prend effet immédiatement.

Vous pouvez modifier les valeurs par défaut de l’une des options de base de données afin qu’elles s’appliquent à toutes les nouvelles bases de données créées. Pour ce faire, modifiez l’option de base de données appropriée dans la base de données model.

## <a name="examples"></a>Exemples

### <a name="a-setting-the-database-to-read_only"></a>R. Paramétrage de la base de données avec READ_ONLY
Pour modifier l’état d’une base de données ou d’un groupe de fichiers en READ_ONLY ou en READ_WRITE, vous avez besoin d’un accès exclusif à la base de données. L’exemple suivant illustre le basculement de la base de données en mode `RESTRICTED_USER` pour d’accès restreint. L'exemple affecte ensuite à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] l'état `READ_ONLY` et rend à tous les utilisateurs l'accès à la base de données.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Activation de l'isolement d'instantané sur une base de données
L'exemple ci-dessous illustre l'activation de l'option d'infrastructure d'isolement d'instantané pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Le jeu de résultats montre que l'infrastructure d'isolement d'instantané est activée.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[nom_base_de_données] |1| ACTIVÉ |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Activation, modification et désactivation du suivi des modifications
L'exemple ci-dessous illustre l'activation du suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et la définition d'une période de rétention de `2` jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

L'exemple suivant illustre comment modifier la période de rétention en spécifiant `3` jours.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

L'exemple ci-dessous illustre comment désactiver le suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Activation du magasin de requêtes
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Activation du magasin des requêtes avec les statistiques d’attente
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Activation du magasin des requêtes avec les options de stratégie de capture personnalisée
L’exemple suivant active le magasin des requêtes et configure ses paramètres.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Voir aussi

- [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistiques](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [Activer et désactiver le suivi des modifications](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bonnes pratiques relatives au Magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE { database_name }
SET
{
    <optionspec> [ ,...n ]
}
;

<option_spec>::=
{
    <auto_option>
  | <db_encryption_option>
  | <query_store_options>
  | <result_set_caching>
  | <snapshot_option>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON }
}

<db_encryption_option> ::=
{
    ENCRYPTION { ON | OFF }
}

<query_store_option> ::=
{
    QUERY_STORE { OFF |  ON }
}

<result_set_caching_option> ::=
{
    RESULT_SET_CACHING {ON | OFF}
}

<snapshot_option> ::=
{
    READ_COMMITTED_SNAPSHOT {ON | OFF }
}

```

## <a name="arguments"></a>Arguments

*database_name*     
Nom de la base de données à modifier.

**<auto_option> ::=**      
Contrôle les options automatiques.

AUTO_CREATE_STATISTICS { **ON** | OFF }     

ACTIVÉ     
L’optimiseur de requête crée si nécessaire des statistiques sur les colonnes uniques des prédicats de requête, afin d’améliorer les plans de requête et les performances des requêtes. Ces statistiques de colonnes uniques sont créées quand l’optimiseur de requête compile les requêtes. Les statistiques de colonnes uniques sont créées uniquement sur les colonnes qui ne constituent pas déjà la première colonne d'un objet de statistiques existant.

La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.

OFF     
L’optimiseur de requête ne crée pas de statistiques sur les colonnes uniques des prédicats de requête quand il compile les requêtes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.

### <a name="remarks"></a>Notes
Cette commande doit être exécutée lorsque vous êtes connecté à la base de données utilisateur.

Vous pouvez déterminer l’état de cette option en consultant la colonne `s_auto_create_stats_on` de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Vous pouvez également déterminer l’état en consultant la propriété `IsAutoCreateStatistics` de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans Statistiques.

**<db_encryption_option> ::=**      
Contrôle l'état de chiffrement de la base de données.

ENCRYPTION { ON | OFF }     

ACTIVÉ     
Indique que la base de données doit être chiffrée.

OFF     
Indique que la base de données ne doit pas être chiffrée.

Pour plus d’informations sur le chiffrement de base de données, consultez Transparent Data Encryption et Transparent Data Encryption avec Azure SQL Database.

Quand le chiffrement est activé au niveau de la base de données, tous les groupes de fichiers sont chiffrés. Tous les nouveaux groupes de fichiers héritent de la propriété chiffrée. Si des groupes de fichiers dans la base de données sont définis sur READ ONLY, l’opération de chiffrement de la base de données échoue.

Vous pouvez voir l’état du chiffrement de la base de données et l’état de l’analyse du chiffrement en utilisant la vue de gestion dynamique sys.dm_database_encryption_keys.

**<query_store_option> ::=**      
Contrôle si le Magasin des requêtes est activé dans cet entrepôt de données.

QUERY_STORE { ON |  **OFF**  }     

ACTIVÉ     
Active le magasin des requêtes.

OFF     
Désactive le magasin des requêtes. OFF est la valeur par défaut.

> [!NOTE]
> Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], vous devez exécuter `ALTER DATABASE SET QUERY_STORE` à partir de la base de données utilisateur. L’exécution de l’instruction à partir d’une autre instance d’entrepôt de données n’est pas prise en charge.

**<result_set_caching_option> ::=**      
**S’applique à** : Azure Synapse Analytics  

Contrôle si le résultat de la requête est mis en cache dans la base de données.

RESULT_SET_CACHING { ON | OFF}     

ACTIVÉ     
Spécifie que les jeux de résultats de requête retournés à partir de cette base de données seront mis en cache dans la base de données.

OFF     
Spécifie que les jeux de résultats de requête retournés à partir de cette base de données ne seront pas mis en cache dans la base de données.

### <a name="remarks"></a>Notes

Cette commande doit être exécutée quand vous êtes connecté à la base de données `master`.  La modification de ce paramètre de base de données prend effet immédiatement.  Des coûts de stockage sont facturés en mettant en cache des jeux de résultats de requête. Après avoir désactivé la mise en cache de résultats pour une base de données, le cache de résultats rendu persistant auparavant sera immédiatement supprimé depuis le stockage Azure Synapse.

Exécutez cette commande pour vérifier la configuration de la mise en cache de l’ensemble des résultats d’une base de données.  Si la mise en cache des résultats est activée, is_result_set_caching_on retourne 1.

```sql
SELECT name, is_result_set_caching_on FROM sys.databases
WHERE name = <'Your_Database_Name'>
```

Exécutez cette commande pour vérifier si une requête a été exécutée avec un résultat mis en cache.  La colonne result_cache_hit retourne 1 pour la correspondance dans le cache, 0 pour l’absence dans le cache et des valeurs négatives reflétant les raisons pour lesquelles la mise en cache du jeu de résultats n’a pas été utilisée.  Pour plus d’informations, consultez [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true).  

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

> [!IMPORTANT]
> Les opérations de création du cache des jeux de résultats et de récupération des données à partir du cache ont lieu sur le nœud de contrôle d’une instance Data Warehouse. Quand la mise en cache des jeux de résultats est activée (ON), l’exécution de requêtes qui retournent un jeu de résultats volumineux (par exemple, > 1 million de lignes) peut entraîner une utilisation intensive du processeur sur le nœud de contrôle et allonger ainsi le temps de réponse de toutes les requêtes sur l’instance. Ces requêtes sont couramment utilisées lors de l’exploration de données et des opérations ETL. Pour éviter une utilisation trop intensive du nœud de contrôle et les problèmes de performances qui en découlent, les utilisateurs doivent désactiver (OFF) la mise en cache des jeux de résultats sur la base de données avant d’exécuter ces types de requêtes.  

Pour plus d’informations sur le réglage des performances avec la mise en cache des jeux de résultats, consultez [Conseils de réglage des performances](/azure/sql-data-warehouse/performance-tuning-result-set-caching).

### <a name="permissions"></a>Autorisations

Pour définir l’option RESULT_SET_CACHING, un utilisateur a besoin d’une connexion du principal au niveau du serveur (celle créée par le processus de provisionnement) ou doit être membre du rôle de base de données `dbmanager`.  

**<snapshot_option> ::=**      
**S’applique à** : Azure Synapse Analytics

Contrôle le niveau d’isolation des transactions d’une base de données.

READ_COMMITTED_SNAPSHOT  { ON | **OFF** }     

ACTIVÉ     
Active l’option READ_COMMITTED_SNAPSHOT au niveau de la base de données.

OFF     
Désactive l’option READ_COMMITTED_SNAPSHOT au niveau de la base de données.

### <a name="remarks"></a>Notes

Cette commande doit être exécutée quand vous êtes connecté à la base de données `master`. La définition de READ_COMMITTED_SNAPSHOT sur ON ou sur OFF pour une base de données utilisateur entraîne la fermeture de toutes les connexions ouvertes à cette base de données. Vous pouvez effectuer cette modification pendant la fenêtre de maintenance de la base de données ou attendre qu’il n’existe plus de connexion active à la base de données, à l’exception de la connexion exécutant la commande ALTER DATABASE.  Il n'est pas nécessaire que la base de données soit en mode mono-utilisateur. La modification du paramètre READ_COMMITTED_SNAPSHOT au niveau de la session n’est pas prise en charge.  Pour vérifier ce paramètre pour une base de données, vérifiez la colonne is_read_committed_snapshot_on dans sys. databases.

Dans une base de données avec la fonction READ_COMMITTED_SNAPSHOT activée, les requêtes peuvent avoir des performances plus lentes en raison de l’analyse des versions si plusieurs versions de données sont présentes. Les transactions longues peuvent également entraîner une augmentation de la taille de la base de données. Ce problème se produit s’il existe des modifications de données effectuées par ces transactions qui bloquent le nettoyage de la version.  

### <a name="permissions"></a>Autorisations

Pour définir l’option READ_COMMITTED_SNAPSHOT, un utilisateur doit disposer de l’autorisation ALTER sur la base de données.

## <a name="examples"></a>Exemples

### <a name="check-statistics-setting-for-a-database"></a>Vérifier le paramètre des statistiques pour une base de données

```sql
SELECT name, is_auto_create_stats_on FROM sys.databases
```

### <a name="enable-query-store-for-a-database"></a>Activer le Magasin des requêtes sur une base de données

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON;
```

### <a name="enable-result-set-caching-for-a-database"></a>Activer la mise en cache d’un jeu de résultats pour une base de données

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE [database_name]
SET RESULT_SET_CACHING ON;
```

### <a name="check-result-set-caching-setting-for-a-database"></a>Vérifier le paramètre de mise en cache d’un jeu de résultats pour une base de données

```sql
SELECT name, is_result_set_caching_on
FROM sys.databases;
```

### <a name="enable-the-read_committed_snapshot-option-for-a-database"></a>Activer l’option Read_Committed_Snapshot pour une base de données

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE MyDatabase
SET READ_COMMITTED_SNAPSHOT ON
```

## <a name="see-also"></a>Voir aussi

- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [Meilleures pratiques pour Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-best-practices#maintain-statistics)
- [Concevoir de tables dans Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview#statistics)
- [Éléments du langage d’Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end

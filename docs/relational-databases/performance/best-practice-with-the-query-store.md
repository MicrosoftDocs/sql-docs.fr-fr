---
title: Bonnes pratiques relatives au Magasin des requêtes | Microsoft Docs
description: Découvrez les bonnes pratiques relatives à l’utilisation du Magasin des requêtes SQL Server avec votre charge de travail, telles que l’utilisation des versions les plus récentes de SQL Server Management Studio et de Query Performance Insight.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, best practices
ms.assetid: 5b13b5ac-1e4c-45e7-bda7-ebebe2784551
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85c9a992660b63f70d2193acd114e7b07d3c1092
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186494"
---
# <a name="best-practices-with-query-store"></a>Bonnes pratiques relatives au Magasin des requêtes

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Cet article décrit les bonnes pratiques concernant l’utilisation du Magasin des requêtes SQL Server avec votre charge de travail.

## <a name="use-the-latest-ssmanstudiofull"></a><a name="SSMS"></a>Utiliser la dernière version de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] propose un ensemble d’interfaces utilisateur conçu pour configurer le Magasin des requêtes et consommer les données collectées relatives à votre charge de travail. Téléchargez la dernière version de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] [ici](../../ssms/download-sql-server-management-studio-ssms.md).

Pour obtenir une description rapide de la manière d’utiliser le Magasin des requêtes dans des scénarios de résolution de problèmes, consultez les blogs relatifs au [\@Azure Magasin des requêtes](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

## <a name="use-query-performance-insight-in-azure-sql-database"></a><a name="Insight"></a> Utiliser Query Performance Insight dans Azure SQL Database

Si vous exécutez le Magasin des requêtes dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez utiliser [Query Performance Insight](/azure/sql-database/sql-database-query-performance) pour analyser la consommation de ressources dans le temps. Même si vous pouvez utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) pour obtenir des détails sur la consommation en ressources de toutes vos requêtes (par exemple, processeur, mémoire et E/S), Query Performance Insight vous offre un moyen rapide et efficace de déterminer leur impact sur la consommation DTU globale de votre base de données. Pour plus d’informations, consultez [Query Performance Insight pour base de données SQL Azure](/azure/azure-sql/database/query-performance-insight-use).

Cette section décrit les paramètres optimaux de configuration par défaut, conçus pour garantir un fonctionnement fiable du Magasin des requêtes et des fonctionnalités dépendantes. La configuration par défaut est optimisée pour la collecte des données en continu, c’est-à-dire pour passer le moins de temps possible aux états OFF/READ_ONLY. Pour plus d’informations sur toutes les options de Magasin des requêtes disponibles, consultez [Options ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store).

| Configuration | Description | Default | Commentaire |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Spécifie la limite d’espace de données que le magasin de requêtes peut inclure dans la base de données client |100 |Appliqué aux nouvelles bases de données |
| INTERVAL_LENGTH_MINUTES |Définit la durée pendant laquelle les statistiques d’exécution collectées pour les plans de requête sont agrégées et rendues persistantes. Chaque plan de requête actif dispose au maximum d’une ligne pour une période définie avec cette configuration |60 |Appliqué aux nouvelles bases de données |
| STALE_QUERY_THRESHOLD_DAYS |Stratégie de nettoyage basée sur la durée, et qui contrôle la période de rétention des statistiques d’exécution persistantes et des requêtes inactives |30 |Appliqué aux nouvelles bases de données et aux bases de données ayant la valeur par défaut précédente (367) |
| SIZE_BASED_CLEANUP_MODE |Indique si un nettoyage automatique des données a lieu lorsque la taille des données du magasin de requêtes approche de la limite |AUTO |Appliqué à toutes les bases de données |
| QUERY_CAPTURE_MODE |Indique si toutes les requêtes sont suivies, ou seulement un sous-ensemble de requêtes |AUTO |Appliqué à toutes les bases de données |
| FLUSH_INTERVAL_SECONDS |Indique la durée maximale pendant laquelle les statistiques d’exécution capturées sont conservées dans la mémoire, avant le vidage sur disque |900 |Appliqué aux nouvelles bases de données |
| | | | |

> [!IMPORTANT]
> Ces paramètres par défaut sont automatiquement appliqués à l’étape finale de l’activation du Magasin des requêtes dans toutes les [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Après cela, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne modifiera pas les valeurs de configuration définies par les clients, à moins qu’elles aient un impact négatif sur les charges de travail principales ou sur la fiabilité des opérations du Magasin des requêtes.

> [!NOTE]  
> Le Magasin des requêtes ne peut pas être désactivé dans la base de données unique [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et le pool élastique. L’exécution de `ALTER DATABASE [database] SET QUERY_STORE = OFF` renverra l’avertissement `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

Si vous souhaitez conserver vos paramètres personnalisés, utilisez [ALTER DATABASE avec les options du magasin de requêtes](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store) pour rétablir la configuration à l’état précédent. Découvrez les [meilleures pratiques liées au Magasin de données des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md) pour savoir comment choisir des paramètres de configuration optimaux.

## <a name="use-query-store-with-elastic-pool-databases"></a>Utiliser le Magasin des requêtes avec des bases de données de pool élastique

Vous pouvez utiliser le magasin de requêtes dans toutes les bases de données sans le moindre problème, même dans les pools très denses. Tous les problèmes liés à l’utilisation excessive de ressources qui peuvent s’être produits lors de l’activation du Magasin des requêtes pour le grand nombre de bases de données des pools élastiques ont été résolus.

## <a name="keep-query-store-adjusted-to-your-workload"></a><a name="Configure"></a> Veiller à l’adéquation entre le magasin de requêtes et votre charge de travail

Configurez le magasin de requêtes en fonction de votre charge de travail et selon vos besoins en matière de résolution des problèmes de performances.
Les paramètres par défaut sont assez bons pour démarrer, mais vous devez surveiller le comportement du Magasin des requêtes au fil du temps et ajuster sa configuration en conséquence.

 ![Propriétés du Magasin des requêtes](../../relational-databases/performance/media/query-store-properties.png "query-store-properties")

 Voici les recommandations à suivre pour définir les valeurs des paramètres :

**Taille maximale (Mo)**  : indique l’espace maximal qu’occupera le Magasin des requêtes au sein de votre base de données. Il s’agit du paramètre le plus important qui affecte directement le mode d’opération du Magasin des requêtes.

Pendant que le Magasin des requêtes collecte des requêtes, des plans d’exécution et des statistiques, sa taille dans la base de données croît jusqu’à atteindre cette limite. Quand cela se produit, le magasin de requêtes change automatiquement de mode d’opération pour passer en lecture seule et cesse de collecter les nouvelles données, ce qui signifie que l’analyse de vos performances n’est plus précise.

La valeur par défaut dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] est de 100 Mo. Cette taille peut ne pas suffire si votre charge de travail génère un grand nombre de requêtes et de plans différents, ou si vous souhaitez conserver plus longtemps l’historique de requêtes. À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], la valeur par défaut est de 1 Go. Effectuez le suivi de l’utilisation de l’espace actuelle et augmentez la valeur **Taille maximale (Mo)** pour empêcher le Magasin des requêtes de passer en mode lecture seule.

> [!IMPORTANT]
> La limite **Taille maximale (Mo)** n’est pas strictement appliquée. La taille de stockage est vérifiée seulement quand le Magasin des requêtes écrit des données sur le disque. Cet intervalle est défini par l’option **Intervalle de vidage des données (minutes)** . Si le Magasin des requêtes a enfreint la limite de taille maximale entre les vérifications de taille de stockage, il passe en mode lecture seule. Si l’option **Mode de nettoyage basé sur la taille** est activée, le mécanisme de nettoyage permettant d’appliquer la limite de taille maximale est également déclenché.

Utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou exécutez le script suivant pour obtenir les dernières informations concernant la taille du magasin de requêtes :

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
 max_storage_size_mb, readonly_reason
FROM sys.database_query_store_options;
```

Le script suivant définit une nouvelle valeur pour **Taille maximale (Mo)**  :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

 **Intervalle de vidange des données (minutes)**  : définit à quelle fréquence les statistiques d’exécution doivent être collectées sur le disque. La fréquence est exprimée en minutes dans l’interface graphique utilisateur (GUI), mais dans [!INCLUDE[tsql](../../includes/tsql-md.md)], elle est exprimée en secondes. La valeur par défaut est de 900 secondes, ce qui correspond à 15 minutes dans l’interface utilisateur graphique. Utilisez une valeur plus élevée si votre charge de travail ne génère pas de grandes quantités de requêtes et de plans différents, ou si vous pouvez supporter une durée de conservation des données plus élevée avant un arrêt de la base de données.

> [!NOTE]
> L’indicateur de trace 7745 empêche l’écriture sur le disque des données du Magasin des requêtes en cas de commande d’arrêt ou de basculement. Pour plus d’informations, consultez la section [Utiliser des indicateurs de trace sur des serveurs critiques](#Recovery).

Utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] pour définir une valeur différente pour **Intervalle de vidage des données** :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS = 900);
```

**Intervalle de collecte des statistiques** : définit le niveau de précision des statistiques d’exécution collectées, exprimé en minutes. La valeur par défaut est de 60 minutes. Envisagez d’utiliser une valeur inférieure si vous avez besoin d’une précision plus fine ou de moins de temps pour détecter et atténuer les problèmes. Gardez à l’esprit que la valeur affecte directement la taille des données du Magasin des requêtes. Utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] pour définir une autre valeur pour **Intervalle de collecte des statistiques** :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 60);
```

**Seuil de requêtes périmées (jours)**  : stratégie de nettoyage basée sur la durée qui permet de contrôler la période de conservation des statistiques d’exécution persistantes et des requêtes inactives, exprimée en jours. Par défaut, le Magasin des requêtes est configuré pour conserver les données pendant 30 jours, ce qui est peut-être inutilement long pour votre scénario.

Évitez de conserver les données d’historique que vous ne prévoyez pas d’utiliser. Cette pratique réduit les passages à l’état lecture seule. La taille des données du Magasin des requêtes et le temps de détection et d’atténuation des problèmes seront plus prévisibles. Utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou le script suivant pour configurer la stratégie de nettoyage basée sur la durée :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 90));
```

**Mode de nettoyage basé sur la taille** : indique si le nettoyage automatique des données a lieu lorsque la taille des données du Magasin des requêtes s’approche de la limite. Activez le nettoyage basée sur la taille pour vous assurer que le Magasin des requêtes s’exécutera toujours en mode lecture-écriture et collectera les données les plus récentes.  Notez qu’il n’y a aucune garantie sous des charges de travail lourdes que le nettoyage du magasin des requêtes maintiendra constamment la taille des données dans la limite. Il est possible que le nettoyage automatique des données passe en arrière-plan et bascule (temporairement) en mode lecture seule.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
```

**Mode de capture du Magasin des requêtes** : spécifie la stratégie de capture des requêtes du Magasin des requêtes.

- **Tout** : Capture toutes les requêtes. Cette option est l’option par défaut dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].
- **Auto** : les requêtes peu fréquentes et les requêtes dont la durée de compilation et d’exécution n’est pas significative sont ignorées. Les seuils concernant le nombre d’exécutions, la durée de compilation et la durée d’exécution sont déterminés en interne. À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il s’agit de l’option par défaut.
- **Aucun** : le Magasin des requêtes cesse de capturer les nouvelles requêtes.
- **Personnalisé** : permet un contrôle supplémentaire et permet d’ajuster la stratégie de collecte des données. Les nouveaux paramètres personnalisés définissent ce qui se passe pendant la durée seuil de la stratégie de capture interne. Il s’agit d’une durée limite pendant laquelle les conditions configurables sont évaluées et, si elles ont la valeur true, la requête peut être capturée par le Magasin des requêtes.

> [!IMPORTANT]
> Les curseurs, les requêtes dans les procédures stockées et les requêtes compilées en mode natif sont toujours capturés quand le mode de capture du Magasin des requêtes est défini sur **Tous**, **Auto** ou **Personnalisé**. Pour capturer des requêtes compilées en mode natif, activez la collecte des statistiques par requête avec [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

 Le script suivant définit QUERY_CAPTURE_MODE sur AUTO :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

### <a name="examples"></a>Exemples

L’exemple suivant définit QUERY_CAPTURE_MODE sur AUTO et définit d’autres options recommandées dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

L’exemple suivant définit QUERY_CAPTURE_MODE sur AUTO et définit d’autres options recommandées dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] pour inclure des statistiques d’attente :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON
    );
```

L’exemple suivant définit QUERY_CAPTURE_MODE sur AUTO et définit d’autres options recommandées dans [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], et il définit *éventuellement* la stratégie de capture CUSTOM (personnalisée) avec ses valeurs par défaut, au lieu du mode de capture AUTO :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1000,
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

## <a name="start-with-query-performance-troubleshooting"></a>Démarrer la résolution des problèmes de performances des requêtes

La résolution des problèmes liés au Magasin des requêtes est un flux de travail simple, comme l’illustre le diagramme suivant :

![Résolution des problèmes relatifs au magasin des requêtes](../../relational-databases/performance/media/query-store-troubleshooting.png "query-store-troubleshooting")

Activez le Magasin des requêtes à l’aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], comme cela est décrit dans la section précédente ou exécutez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :

```sql
ALTER DATABASE [DatabaseOne] SET QUERY_STORE = ON;
```

Le Magasin des requêtes a besoin d’un certain temps avant de collecter le jeu de données qui représente avec précision votre charge de travail. En générale, un jour suffit, même pour les charges de travail très complexes. Néanmoins, vous pouvez commencer à explorer les données et à identifier les requêtes qui demandent votre attention immédiate après avoir activé la fonctionnalité. Accédez au sous-dossier Magasin des requêtes sous le nœud de base de données dans l’Explorateur d’objets de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour ouvrir les vues de résolution de problèmes de scénarios spécifiques.

Les vues du magasin de requêtes de[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fonctionnent avec l’ensemble de métriques d’exécution, chacune exprimée comme étant l’une des fonctions statistiques suivantes :

|Version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Métrique d’exécution|Fonction statistique|
|----------------------|----------------------|------------------------|
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Temps processeur, Durée, Nombre d’exécutions, Lectures logiques, Écritures logiques, Consommation de mémoire, Lectures physiques, Durée du CLR, Degré de parallélisme et Nombre de lignes|Moyenne, Maximum, Minimum, Écart type, Total|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|Temps processeur, Durée, Nombre d’exécutions, Lectures logiques, Écritures logiques, Consommation de mémoire, Lectures physiques, Durée du CLR, Degré de parallélisme, Nombre de lignes, Mémoire utilisée par la journalisation, Mémoire tempdb et Délai d’attente|Moyenne, Maximum, Minimum, Écart type, Total|

Le graphique suivant montre comment trouver les vues du magasin de requêtes :

![Vues du magasin des requêtes](../../relational-databases/performance/media/objectexplorerquerystore_sql17.png "Vues du magasin des requêtes")

Le tableau suivant explique quand utiliser chaque vue du magasin de requêtes :

|Vue SQL Server Management Studio|Scénario|
|---------------|--------------|
|**Requêtes régressées**|Identifie les requêtes dont les métriques d’exécution ont récemment régressé (par exemple, dont l’état s’est aggravé). <br />Utilisez cette vue pour mettre en corrélation les problèmes de performances observés dans votre application avec les requêtes réelles qui ont besoin d’être corrigées ou améliorées.|
|**Consommation globale des ressources**|Analyse la consommation totale de ressources pour la base de données par rapport à l’une des métriques d’exécution.<br />Utilisez cette vue pour identifier des modèles de ressources (charges de travail diurnes/nocturnes) et optimiser la consommation globale pour votre base de données.|
|**Principales requêtes consommatrices de ressources**|Choisissez une mesure d’exécution présentant un intérêt et identifiez les requêtes qui ont enregistré les valeurs les plus extrêmes au cours d’un intervalle de temps donné. <br />Utilisez cette vue pour concentrer votre attention sur les requêtes les plus pertinentes, qui ont le plus fort impact sur la consommation en ressources de base de données.|
|**Requêtes avec des plans forcés**|Liste les plans forcés à l’aide du Magasin des requêtes. <br />Utilisez cette vue pour accéder rapidement à tous les plans forcés.|
|**Requêtes avec variation forte**|Analysez les requêtes ayant une forte variation d’exécution en lien avec les dimensions disponibles, notamment la durée, le temps processeur, les E/S et l’utilisation de la mémoire dans l’intervalle de temps souhaité.<br />Utilisez cette vue pour identifier les requêtes avec des performances extrêmement variables qui peuvent affecter l’expérience utilisateur dans vos applications.|
|**Statistiques d’attente des requêtes**|Analysez les catégories d’attente qui sont les plus actives dans une base de données, et les requêtes qui contribuent le plus à la catégorie d’attente sélectionnée.<br />Utilisez cette vue pour analyser les statistiques d’attente et identifier les requêtes susceptibles d’affecter l’expérience utilisateur dans vos applications.<br /><br />S’applique à : à compter de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18.0 et de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Requêtes suivies**|Suivez l’exécution des requêtes les plus importantes en temps réel. En règle générale, vous utilisez cette vue quand certaines de vos requêtes sont soumises à des plans forcés et que vous voulez vérifier que les performances des requêtes sont stables.|

> [!TIP]
> Pour savoir comment identifier les principales requêtes consommatrices de ressources et corriger celles qui ont régressé en raison d’un changement de plan à l’aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], consultez les blogs \@Azure relatifs au [Magasin des requêtes](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

Quand vous identifiez une requête dont les performances ne sont pas optimales, votre action dépend de la nature du problème.

- Si la requête a été exécutée avec plusieurs plans et que le dernier est nettement plus mauvais que le précédent, vous pouvez utiliser le mécanisme permettant de forcer le plan. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de forcer le plan de l’optimiseur. Si le forçage de plan échoue, un XEvent est déclenché et l’optimiseur est tenu d’optimiser de façon normale.

  ![Magasin des requêtes - Forcer le plan](../../relational-databases/performance/media/query-store-force-plan.png "query-store-force-plan")

  > [!NOTE]
  > Le graphique précédent peut présenter des formes différentes pour des plans de requête spécifiques, avec les significations suivantes pour chaque état possible :<br />
  >
  > |Graphique à base de formes|Signification|
  > |-------------------|-------------|
  > |Circle|Requête terminée, ce qui signifie qu’une exécution normale s’est achevée correctement.|
  > |Carré|Annulée, ce qui signifie qu’un client est à l’origine de l’abandon de l’exécution.|
  > |Triangle|Échec, ce qui signifie qu’une exception est à l’origine de l’abandon de l’exécution.|
  >
  > De plus, la taille de la forme reflète le nombre d’exécutions des requêtes dans l’intervalle de temps spécifié. La taille augmente en fonction du nombre d’exécutions.

- Vous pouvez en déduire qu’il manque un index à votre requête pour qu’elle s’exécute de façon optimale. Ces informations apparaissent dans le plan d’exécution de requête. Créez l’index manquant et vérifiez le niveau de performance des requêtes en utilisant le Magasin des requêtes.

   ![Magasin des requêtes - Afficher le plan](../../relational-databases/performance/media/query-store-show-plan.png "query-store-show-plan")

Si vous exécutez votre charge de travail sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)], inscrivez-vous à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Index Advisor pour recevoir automatiquement des recommandations d’index.

- Dans certains cas, vous pouvez imposer une recompilation des statistiques si vous constatez une grande différence entre le nombre estimé et le nombre réel de lignes dans le plan d’exécution.
- Réécrivez les requêtes problématiques, par exemple, pour profiter du paramétrage des requêtes ou pour implémenter une logique plus optimale.

## <a name="verify-that-query-store-collects-query-data-continuously"></a><a name="Verify"></a> Vérifier que le Magasin des requêtes collecte les données des requêtes en continu

Le Magasin des requêtes peut modifier discrètement le mode d’opération. Surveillez régulièrement l’état du Magasin des requêtes pour vérifier qu’il fonctionne et pour prendre des mesures afin d’éviter des défaillances dont les causes étaient évitables. Exécutez la requête suivante pour déterminer le mode d’opération et afficher les paramètres les plus pertinents :

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

La différence entre `actual_state_desc` et `desired_state_desc` indique que le mode d’opération a changé automatiquement. Le changement le plus courant correspond au basculement discret du Magasin des requêtes en mode lecture seule. Dans certaines circonstances très rares, le Magasin des requêtes peut finir à l’état ERREUR en raison d’erreurs internes.

Quand l’état effectif est Lecture seule, consultez la colonne **readonly_reason** pour déterminer la cause première. En règle générale, vous déterminez que le Magasin des requêtes est passé en mode lecture seule quand le quota de taille a été dépassé. Dans ce cas, **readonly_reason** a la valeur 65536. Pour d’autres raisons, consultez [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).

Pour faire basculer le magasin de requêtes en mode lecture-écriture et activer la collecte de données, envisagez les mesures suivantes :

- Augmentez la taille maximale de stockage en utilisant l’option **MAX_STORAGE_SIZE_MB** de la commande **ALTER DATABASE**.
- Nettoyez les données du magasin de requêtes à l’aide de l’instruction suivante :

  ```sql
  ALTER DATABASE [QueryStoreDB] SET QUERY_STORE CLEAR;
  ```

Vous pouvez appliquer une ou deux de ces mesures en exécutant l’instruction suivante qui remet explicitement le mode d’opération en lecture-écriture :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
```

À titre préventif, prenez les mesures suivantes :

- Vous pouvez prévenir les changements discrets du mode d’opération en appliquant les bonnes pratiques. Vérifiez que la taille du Magasin des requêtes est toujours inférieure à la valeur maximale autorisée pour réduire considérablement les chances de passer en mode lecture seule. Activez la stratégie basée sur la taille comme indiqué dans la section [Configurer le Magasin des requêtes](#Configure) pour que le Magasin des requêtes nettoie automatiquement les données quand sa taille approche de la limite.
- Pour faire en sorte que les données les plus récentes soient conservées, configurez une stratégie basée sur la durée pour supprimer régulièrement les informations obsolètes.
- Enfin, envisagez de définir l’option **Mode de capture du Magasin des requêtes** sur **Auto**, car cela permet d’éliminer les requêtes moins pertinentes pour votre charge de travail.

### <a name="error-state"></a>État ERREUR

Pour récupérer le Magasin des requêtes, essayez de définir explicitement le mode lecture-écriture et revérifiez l’état réel.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

Si le problème persiste, cela indique que l’endommagement des données du Magasin des requêtes est rendu persistant sur le disque.

Depuis [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], le Magasin des requêtes peut être récupéré via l’exécution de la procédure stockée **sp_query_store_consistency_check** dans la base de données affectée. Il faut désactiver le Magasin des requêtes avant de tenter l’opération de récupération. Pour [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous devez effacer les données du Magasin des requêtes comme cela est indiqué.

Si la récupération a échoué, vous pouvez essayer d’effacer le contenu du Magasin des requêtes avant de définir le mode lecture-écriture.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE CLEAR;
GO

ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

## <a name="set-the-optimal-query-store-capture-mode"></a>Définir le mode de capture optimal du Magasin des requêtes

Conservez les données les plus pertinentes dans le magasin de requêtes. Le tableau suivant décrit des scénarios standard pour chaque mode de capture du Magasin des requêtes :

|Mode de capture du magasin des requêtes|Scénario|
|------------------------|--------------|
|**Tout**|Analysez minutieusement votre charge de travail, c’est-à-dire toutes les formes de requêtes, leur fréquence d’exécution et les autres statistiques.<br /><br /> Identifiez les nouvelles requêtes dans votre charge de travail.<br /><br /> Détectez si des requêtes ad hoc sont utilisées pour identifier les opportunités de paramétrage défini par l’utilisateur ou automatique.<br /><br />Remarque : Il s’agit du mode de capture par défaut dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Automatique**|Concentrez-vous sur les requêtes pertinentes et actionnables. Les requêtes qui s’exécutent régulièrement ou qui consomment beaucoup de ressources en sont un exemple.<br /><br />Remarque : à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], il s’agit du mode de capture par défaut.|
|**Aucun**|Vous avez déjà capturé le jeu de requêtes que vous vouliez surveiller dans le runtime et souhaitez éliminer les confusions que pourraient provoquer les autres requêtes.<br /><br /> L’option Aucun est adaptée aux environnements de test et d’évaluation.<br /><br /> Elle est aussi appropriée pour les éditeurs de logiciels qui proposent le magasin de requêtes avec une configuration destinée à surveiller la charge de travail de leur application.<br /><br /> Cette option doit être utilisée avec précaution, car vous risquez de ne pas pouvoir suivre et optimiser de nouvelles requêtes importantes. Évitez d’utiliser l’option Aucun(e) sauf si l’un de vos scénarios l’exige.|
|**Personnalisée**|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduit un mode de capture personnalisé sous la commande `ALTER DATABASE SET QUERY_STORE`. Quand ce mode est activé, vous pouvez affiner la collecte de données sur un serveur spécifique au moyen de configurations supplémentaires du Magasin des requêtes, disponibles sous un nouveau paramètre de stratégie de capture du Magasin des requêtes.<br /><br />Les nouveaux paramètres personnalisés définissent ce qui se passe pendant la durée seuil de la stratégie de capture interne. Il s’agit d’une durée limite pendant laquelle les conditions configurables sont évaluées et, si elles ont la valeur true, la requête peut être capturée par le Magasin des requêtes. Pour plus d’informations, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).|

> [!NOTE]
> Les curseurs, les requêtes dans les procédures stockées et les requêtes compilées en mode natif sont toujours capturés quand le mode de capture du Magasin des requêtes est défini sur **Tous**, **Auto** ou **Personnalisé**. Pour capturer des requêtes compilées en mode natif, activez la collecte des statistiques par requête avec [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

## <a name="keep-the-most-relevant-data-in-query-store"></a>Conserver les données les plus pertinentes dans le magasin des requêtes

Configurez le Magasin des requêtes afin qu’il contienne uniquement les données pertinentes. Ainsi, il s’exécutera toujours en offrant une excellente expérience de résolution des problèmes tout en ayant un impact minimal sur votre charge de travail normale.
Le tableau suivant décrit les bonnes pratiques :

|Bonne pratique|Paramètre|
|-------------------|-------------|
|Limiter la conservation des données d’historique.|Configurez la stratégie basée sur la durée pour activer le nettoyage automatique.|
|Filtrer les requêtes non pertinentes.|Configurez l’option **Mode de capture du Magasin des requêtes** sur **Auto**.|
|Supprimer les requêtes les moins pertinentes quand la taille maximale est atteinte.|Activez la stratégie de nettoyage basée sur la taille.|

## <a name="avoid-using-non-parameterized-queries"></a><a name="Parameterize"></a> Éviter l’utilisation de requêtes non paramétrées

Il n’est pas recommandé d’utiliser des requêtes non paramétrées quand cela n’est pas nécessaire. Le cas d’une analyse ad hoc en est un exemple. Les plans mis en cache ne peuvent pas être réutilisés, ce qui force l’optimiseur de requête à compiler les requêtes pour chaque texte de requête unique. Pour plus d’informations, consultez [Principes d’utilisation du paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

De plus, le Magasin des requêtes peut rapidement dépasser le quota de taille en raison du nombre potentiellement élevé de textes de requête différents et ainsi du nombre élevé de plans d’exécution différents de forme similaire. Par conséquent, votre charge de travail ne fonctionne pas de façon optimale et le Magasin des requêtes risque de basculer en mode lecture seule ou de supprimer constamment des données pour essayer de suivre le rythme des requêtes entrantes.

Considérez les options suivantes :

- Paramétrez les requêtes lorsque cela est possible. Par exemple, incluez les requêtes dans un wrapper au sein d’une procédure stockée ou de sp_executesql. Pour plus d’informations, consultez [Réutilisation des paramètres et des plans d’exécution](../../relational-databases/query-processing-architecture-guide.md#PlanReuse).
- Utilisez l’option [Optimiser pour les charges de travail ad hoc](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) si votre charge de travail contient de nombreux lots ad hoc à usage unique avec des plans de requête différents.
  - Comparez le nombre de valeurs query_hash distinctes au nombre total d’entrées dans sys.query_store_query. Si le rapport est proche de 1, votre charge de travail ad hoc génère des requêtes différentes.
- Appliquez un [paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) pour la base de données ou un sous-ensemble de requêtes si le nombre de plans de requête différents est modéré.
  - Utilisez un [repère de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md) pour forcer le paramétrage uniquement pour la requête sélectionnée.
  - Configurez le paramétrage forcé en utilisant la commande [parameterization database option](../../relational-databases/databases/database-properties-options-page.md#miscellaneous) si votre charge de travail ne contient qu’un petit nombre de plans de requête différents. C’est le cas, par exemple, lorsque le rapport entre le nombre de valeurs query_hash distinctes et le nombre total d’entrées dans sys.query_store_query est nettement inférieur à 1.
- Définissez QUERY_CAPTURE_MODE sur AUTO pour éliminer automatiquement les requêtes ad hoc qui consomment peu de ressources.

## <a name="avoid-a-drop-and-create-pattern-for-containing-objects"></a><a name="Drop"></a> Éviter un modèle DROP et CREATE pour les objets conteneurs

Le Magasin des requêtes associe une entrée de requête à un objet conteneur, tel qu’une procédure stockée, une fonction ou un déclencheur. Quand vous recréez un objet conteneur, une nouvelle entrée de requête est générée pour le même texte de requête. Cela vous empêche de suivre les statistiques de performances de cette requête dans le temps et d’utiliser un mécanisme permettant de forcer le plan. Pour éviter une telle situation, utilisez le processus `ALTER <object>` pour modifier la définition d’un objet conteneur chaque fois que cela est possible.

## <a name="check-the-status-of-forced-plans-regularly"></a><a name="CheckForced"></a> Vérifier régulièrement l’état des plans forcés

Le forçage de plan est un mécanisme pratique qui permet de corriger les problèmes de performances des requêtes importantes et de les rendre plus prévisibles. Comme pour les indicateurs de plan et les repères de plan, forcer un plan n’est pas la garantie qu’il sera utilisé dans les exécutions futures. En règle générale, quand le schéma de base de données change de manière à ce que les objets référencés par le plan d’exécution sont modifiés ou supprimés, le forçage de plan échoue. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a recours à la recompilation des requêtes et la raison réelle de l’échec du forçage apparaît dans [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). La requête suivante retourne des informations sur les plans forcés :

```sql
USE [QueryStoreDB];
GO

SELECT p.plan_id, p.query_id, q.object_id as containing_object_id,
    force_failure_count, last_force_failure_reason_desc
FROM sys.query_store_plan AS p
JOIN sys.query_store_query AS q on p.query_id = q.query_id
WHERE is_forced_plan = 1;
```

Pour obtenir la liste complète des raisons, consultez [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). Vous pouvez aussi utiliser le XEvent **query_store_plan_forcing_failed** pour suivre les échecs de l’utilisation forcée d’un plan et y remédier.

## <a name="avoid-renaming-databases-for-queries-with-forced-plans"></a><a name="Renaming"></a> Éviter de renommer les bases de données pour des requêtes associées à des plans forcés

Les plans d’exécution référencent les objets avec des noms en trois parties, tels que `database.schema.object`.

Si vous renommez une base de données, le forçage de plan échoue, ce qui entraîne une recompilation dans toutes les exécutions de requête suivantes.

## <a name="using-query-store-in-mission-critical-servers"></a><a name="Recovery"></a> Utilisation du Magasin des requêtes dans des serveurs stratégiques

Les indicateurs de trace globaux 7745 et 7752 peuvent être utilisés pour améliorer la disponibilité des bases de données à l’aide du Magasin des requêtes. Pour plus d’informations, consultez [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

- L’indicateur de trace 7745 empêche le comportement par défaut selon lequel le Magasin des requêtes écrit des données sur le disque avant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse être arrêté. Cela signifie que les données du Magasin des requêtes qui ont été collectées mais pas encore enregistrées de façon permanente sur le disque seront perdues, jusqu’à la fenêtre de temps définie avec `DATA_FLUSH_INTERVAL_SECONDS`.
- L’indicateur de trace 7752 permet le chargement asynchrone du Magasin des requêtes. Cela permet de mettre en ligne une base de données et d’exécuter des requêtes avant la récupération complète du Magasin des requêtes. Le comportement par défaut consiste à charger de façon synchrone le Magasin de données des requêtes. Le comportement par défaut empêche l’exécution des requêtes avant la récupération du Magasin des requêtes, mais il évite également qu’une requête soit manquée lors de la collecte des données.

> [!NOTE]
> À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], ce comportement est contrôlé par le moteur, et l’indicateur de trace 7752 n’a pas d’effet.

> [!IMPORTANT]
> Si vous utilisez le Magasin des requêtes pour avoir des aperçus juste-à-temps de la charge de travail dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], prévoyez d’installer dès que possible les correctifs de scalabilité des performances figurant dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 ([KB 4340759](https://support.microsoft.com/help/4340759)). Sans ces améliorations, lorsque la base de données est sous des charges de travail lourdes, une contention de verrouillages spinlock peut se produire et les performances du serveur peuvent devenir lentes. En particulier, vous pouvez constater une contention importante sur le verrouillage spinlock `QUERY_STORE_ASYNC_PERSIST` ou `SPL_QUERY_STORE_STATS_COOKIE_CACHE`. Une fois cette amélioration appliquée, le Magasin des requêtes n’entraîne plus de contention de verrouillages spinlock.

> [!IMPORTANT]
> Si vous utilisez le Magasin des requêtes pour obtenir des insights de la charge de travail juste-à-temps dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), prévoyez d’installer dès que possible l’amélioration de la scalabilité des performances de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU15, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU23 et [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU9. Sans cette amélioration, lorsque la base de données est sous des charges de travail ad hoc lourdes, le Magasin des requêtes peut utiliser une grande quantité de mémoire et les performances du serveur peuvent devenir lentes. Une fois cette amélioration appliquée, le Magasin des requêtes impose des limites internes à la quantité de mémoire que ses différents composants peuvent utiliser et peut automatiquement changer le mode d’opération en lecture seule jusqu’à ce que la mémoire soit suffisamment retournée au [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Notez que les limites de mémoire interne du Magasin des requêtes ne sont pas documentées, car elles sont sujettes à modification.  


## <a name="using-query-store-in-azure-sql-database-active-geo-replication"></a><a name="geosyncreplicas"></a> Utilisation du Magasin des requêtes dans la géoréplication active d’Azure SQL Database

Le Magasin des requêtes sur une géoréplication active secondaire d’Azure SQL Database est une copie en lecture seule de l’activité sur le réplica principal. 

Évitez les niveaux incompatibles entre les bases de données Azure SQL qui participent à la géoréplication. La taille de calcul de la base de données secondaire doit être égale à celle de la base de données primaire, ou proche de celle-ci, et se trouver dans le même niveau de service que celui de la base de données primaire. Recherchez le type d’attente HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO dans [sys.dm_db_wait_stats](../system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md), qui indique une limitation du taux de journalisation des transactions sur le réplica principal en raison d’un décalage au niveau secondaire.

Pour plus d’informations sur l’estimation et la configuration de la taille de la base de données Azure SQL secondaire de la géoréplication active, consultez [Configuration d’une base de données secondaire](/azure/azure-sql/database/active-geo-replication-overview#configuring-secondary-database).


## <a name="see-also"></a>Voir aussi

- [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)
- [Vues de catalogue du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Utilisation du Magasin des requêtes avec l’OLTP en mémoire](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Surveillance des performances à l’aide du Magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)

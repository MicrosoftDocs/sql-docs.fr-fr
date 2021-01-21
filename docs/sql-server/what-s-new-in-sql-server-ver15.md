---
title: Nouveautés de SQL Server 2019 | Microsoft Docs
description: Découvrez les nouvelles fonctionnalités de SQL Server 2019 (15.x) qui vous permettent de choisir les langages de développement, les types de données, les environnements et les systèmes d’exploitation.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 749e13242c81e5a566ae13dfd66385194639db37
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596224"
---
# <a name="whats-new-in-sql-server-2019"></a>Nouveautés de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur les versions précédentes pour faire de SQL Server une plateforme compatible avec de nombreux langages de développement, types de données, systèmes d’exploitation et environnement locaux ou cloud.

Cet article récapitule les nouvelles fonctionnalités et améliorations de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].

Pour obtenir plus d’informations et découvrir les problèmes connus, consultez les [notes de publication de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](sql-server-version-15-release-notes.md).

Pour bénéficier d’une expérience optimale avec [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], utilisez les [derniers outils](../azure-data-studio/download-azure-data-studio.md).

[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] pour [!INCLUDE[sql-server](../includes/ssnoversion-md.md)]. Il fournit également des capacités supplémentaires et des améliorations pour le moteur de base de données SQL Server, SQL Server Analysis Services, SQL Server Machine Learning Services, SQL Server sur Linux et SQL Server Master Data Services.

La vidéo suivante fournit une présentation de 13 minutes de SQL Server 2019 :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Les sections suivantes fournissent une vue d’ensemble de ces fonctionnalités.

## <a name="data-virtualization-and-big-data-clusters-2019"></a>Virtualisation des données et [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]

Aujourd’hui, les entreprises règnent souvent sur de vastes patrimoines de données constitués de jeux de données très divers et toujours plus grands, qui sont hébergés dans des sources de données compartimentées. Obtenez des insights en quasi temps réel à partir de toutes vos données avec des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Ceux-ci fournissent un environnement complet permettant d’utiliser des jeux de données volumineux, notamment des fonctionnalités de machine learning et d’IA.

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
| Solution de Big Data scalable | [Déployer des clusters scalables](../big-data-cluster/deploy-get-started.md) de conteneurs SQL Server, Spark et HDFS exécutés sur Kubernetes. <br/><br/> Lire, écrire et traiter les données du Big Data à partir de Transact-SQL ou de Spark.<br/><br/> Combiner et analyser facilement des données relationnelles à valeur élevée et un volume important de données du Big Data.<br/><br/>Interroger des sources de données externes.<br/><br/>Stocker les données du Big Data dans un système HDFS géré par SQL Server.<br/><br/>Interroger les données de plusieurs sources de données externes via le cluster.<br/><br/> Utiliser les données pour l’IA, le machine learning et d’autres tâches d’analyse.<br/><br/> [Déployer et exécuter des applications](../big-data-cluster/concept-application-deployment.md) dans des [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]. <br/><br/> L’instance maître SQL Server fournit la haute disponibilité et la reprise d’activité après sinistre pour toutes les bases de données à l’aide de la technologie des groupes de disponibilité Always On.<br/>|
|Virtualisation de données avec Polybase | Interrogez des données à partir de sources de données SQL Server, Oracle, Teradata, MongoDB et ODBC externes avec des tables externes, désormais à l’aide de la [prise en charge de l’encodage UTF-8](../relational-databases/collations/collation-and-unicode-support.md). Pour plus d’informations, consultez [Qu’est-ce que PolyBase ?](../relational-databases/polybase/polybase-guide.md)|
| &nbsp; | &nbsp; |

Pour plus d’informations, consultez [Que sont les [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] SQL Server ?](../big-data-cluster/big-data-cluster-overview.md)

## <a name="intelligent-database"></a>Base de données intelligente
[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur les innovations des versions précédentes pour fournir des performances de pointe dès sa première utilisation. Du [traitement intelligent des requêtes](../relational-databases/performance/intelligent-query-processing.md) à la prise en charge des appareils à mémoire persistante, les fonctionnalités dans le domaine des bases de données intelligentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] améliorent les performances et la scalabilité de l’ensemble de vos charges de travail de base de données sans aucune modification de la conception de votre application ou de votre base de données.

### <a name="intelligent-query-processing"></a>Traitement de requêtes intelligent
Le [traitement intelligent des requêtes](../relational-databases/performance/intelligent-query-processing.md) optimise les charges de travail parallèles critiques exécutées à grande échelle. En même temps, ces charges sont capables de s’adapter au monde des données en constante évolution. Le traitement intelligent des requêtes est disponible par défaut sur le dernier paramètre du [niveau de compatibilité de la base de données](../t-sql/statements/alter-database-transact-sql-compatibility-level.md#differences-between-compatibility-level-140-and-level-150), offrant ainsi un impact important qui améliore les performances des charges de travail existantes avec un effort d’implémentation minimal.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Rétroaction d’allocation de mémoire en mode ligne |Développe la fonctionnalité de commentaires d’allocation de mémoire en mode batch en ajustant les tailles d’allocation de mémoire pour les opérateurs du mode batch et du mode ligne. Cet ajustement peut automatiquement corriger les allocations excessives qui entraînent une perte de mémoire et une concurrence réduite. Il peut également remédier aux allocations de mémoire insuffisantes qui entraînent des dépassements de capacité coûteux sur le disque. Consultez [Commentaires d’allocation de mémoire en mode ligne](../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback). |
|Mode Batch sur rowstore | Active l’exécution en mode batch sans nécessiter d’index columnstore. L’exécution en mode batch utilise le processeur plus efficacement pendant les charges de travail analytiques. Avant [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], elle était uniquement utilisée quand une requête incluait des opérations avec des index columnstore. Toutefois, certaines applications peuvent utiliser des fonctionnalités qui ne sont pas prises en charge avec les index columnstore. Elles ne peuvent donc pas tirer parti du mode batch. À compter de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], le mode batch est activé sur les charges de travail analytiques éligibles dont les requêtes incluent des opérations avec n’importe quel type d’index (rowstore ou columnstore). Consultez [Mode Batch sur rowstore](../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore). |
|Incorporation des fonctions UDF scalaires|Transforme automatiquement les fonctions scalaires définies par l’utilisateur en expressions relationnelles, et les incorpore à la requête SQL d’appel. Cette transformation améliore les performances des charges de travail qui tirent parti des fonctions UDF scalaires. Voir [Incorporation (inlining) des fonctions UDF scalaires](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Compilation différée de variable de table|Améliore la qualité du plan et le niveau de performance global pour les requêtes faisant référence à des variables de tables. Pendant l’optimisation et la compilation initiale, cette fonctionnalité propage les estimations de cardinalité basées sur le nombre réel de lignes de la variable de table. Ces informations précises sur le nombre de lignes optimisent les opérations de plan en aval. Consultez [Compilation différée de variables de tables](../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation). |
|Traitement approximatif des requêtes avec `APPROX_COUNT_DISTINCT` |Pour les scénarios dans lesquels la précision absolue n’est pas importante, mais où la réactivité est essentielle, `APPROX_COUNT_DISTINCT` agrège des jeux de données volumineux tout en utilisant moins de ressources que `COUNT(DISTINCT())` pour une concurrence supérieure. Consultez [Traitement des requêtes approximatif](../relational-databases/performance/intelligent-query-processing.md#approximate-query-processing).|
| &nbsp; | &nbsp; |


### <a name="in-memory-database"></a>Base de données en mémoire
Les technologies [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de [base de données en mémoire](../relational-databases/in-memory-database.md) tirent parti des innovations matérielles modernes pour offrir des performances et une mise à l’échelle inégalées. [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur des innovations antérieures dans ce domaine, notamment le traitement transactionnel en ligne (OLTP) en mémoire, pour apporter un niveau de scalabilité sans précédent à l’ensemble de vos charges de travail de base de données.  

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Pool de tampons hybride| Une nouvelle fonctionnalité de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] avec laquelle les pages de base de données qui se trouvent sur des fichiers de base de données placés sur un appareil à mémoire persistante (PMEM) sont directement accessibles si nécessaire. Consultez [Pool de mémoires tampons hybride](../database-engine/configure-windows/hybrid-buffer-pool.md).|
|Métadonnées TempDB à mémoire optimisée| [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit dans la famille de fonctionnalités [Base de données en mémoire](../relational-databases/in-memory-database.md) une nouvelle fonctionnalité, les métadonnées TempDB à mémoire optimisée, qui supprime efficacement ce goulot d’étranglement et déverrouille un nouveau niveau de scalabilité pour les charges de travail de base de données TempDB lourdes. Dans [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], les tables système impliquées dans la gestion des métadonnées de table temporaire peuvent être déplacées dans des tables à mémoire optimisée non durables dépourvues de verrous. Consultez [Métadonnées tempdb à mémoire optimisée](../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).|
| Prise en charge de l’OLTP en mémoire pour les instantanés de base de données | [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit la prise en charge de la création d’[instantanés de bases de données](../relational-databases/databases/database-snapshots-sql-server.md) qui incluent des groupes de fichiers à mémoire optimisée. |
| &nbsp; | &nbsp; |

### <a name="intelligent-performance"></a>Performances intelligentes
[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur les innovations dans le domaine des bases de données intelligentes des versions précédentes pour garantir [une exécution plus rapide](/archive/blogs/bobsql/). Ces améliorations permettent de surmonter les goulots d’étranglement connus au niveau des ressources et offrent des options à l’aide desquelles vous pouvez configurer votre serveur de base de données pour fournir des performances prévisibles sur toutes vos charges de travail.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|`OPTIMIZE_FOR_SEQUENTIAL_KEY`|Active une optimisation dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] qui vous aide à améliorer le débit pour les insertions de haute concurrence dans l’index. Cette option vise les index sujets à contention d’insertion de la dernière page, ce qui se produit souvent avec des index comportant une clé séquentielle comme une colonne d’identité, une séquence ou une colonne de date/heure. Consultez [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys).|
|Forcer les curseurs statiques et les curseurs avec avance rapide | Prend en charge la possibilité de forcer le plan du Magasin des requêtes pour l’avance rapide et les curseurs statiques. Consultez [Planifier la prise en charge du forçage des curseurs statiques et des curseurs avec avance rapide](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#ctp23).|
|Gouvernance des ressources| La valeur configurable pour l’option `REQUEST_MAX_MEMORY_GRANT_PERCENT` de `CREATE WORKLOAD GROUP` et `ALTER WORKLOAD GROUP` est passée d’un entier à un type de données float, pour permettre un contrôle plus granulaire des limites de la mémoire. Consultez [ALTER WORKLOAD GROUP](../t-sql/statements/alter-workload-group-transact-sql.md) et [CREATE WORKLOAD GROUP](../t-sql/statements/create-workload-group-transact-sql.md).|
|Recompilations réduites pour les charges de travail| Améliore les performances lors de l’utilisation de tables temporaires sur plusieurs étendues en réduisant les recompilations inutiles. Consultez [Recompilations réduites pour les charges de travail](../relational-databases/tables/tables.md#ctp23). |
|Scalabilité des points de contrôle indirect |Consultez [Amélioration de la scalabilité des points de contrôle indirect](../relational-databases/logs/database-checkpoints-sql-server.md#ctp23).|
|Mises à jour PFS simultanées|Les [pages PFS (Page Free Space)](https://techcommunity.microsoft.com/t5/SQL-Server/Under-the-covers-GAM-SGAM-and-PFS-pages/ba-p/383125) sont des pages spéciales dans un fichier de base de données que SQL Server utilise pour localiser l’espace libre quand il alloue de l’espace pour un objet. La contention de verrous de page sur les pages PFS est couramment associée à [TempDB](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d), mais elle peut également se produire sur des bases de données utilisateur s’il existe de nombreux threads d’allocation d’objets simultanés. Cette amélioration modifie la façon dont la concurrence est managée avec les mises à jour PFS afin qu’elles puissent être mises à jour sous un verrou partagé, plutôt qu’avec un verrou exclusif. Ce comportement est activé par défaut dans toutes les bases de données (notamment TempDB) à compter de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].|
|Migration de workers d’un planificateur |La migration de workers permet à un planificateur inactif de migrer un worker à partir de la file d’attente exécutable d’un autre planificateur sur le même nœud NUMA et de reprendre immédiatement la tâche du worker migré. Cette amélioration permet de mieux équilibrer l’utilisation du processeur dans les situations où des tâches de longue durée sont affectées au même planificateur. Pour plus d’informations, consultez [Performances intelligentes dans SQL Server 2019 - Migration de workers](https://techcommunity.microsoft.com/t5/SQL-Server/SQL-Server-2019-Intelligent-Performance-Worker-Migration/ba-p/939610). |
| &nbsp; | &nbsp; |

### <a name="monitoring"></a>Surveillance
Les améliorations apportées à la supervision vous permettent d’obtenir, au moment opportun, des insights sur les performances de n’importe quelle charge de travail de base de données.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|`WAIT_ON_SYNC_STATISTICS_REFRESH` | Nouveau type d’attente dans la vue de gestion dynamique `sys.dm_os_wait_stats`. Il montre le temps, cumulé par instance, consacré aux opérations d’actualisation synchrone des statistiques. Voir [`sys.dm_os_wait_stats`](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|
|Stratégie de capture personnalisée pour le Magasin des requêtes | Quand cette stratégie est activée, vous pouvez affiner la collecte de données sur un serveur spécifique au moyen de configurations supplémentaires du Magasin des requêtes, disponibles sous un nouveau paramètre de stratégie de capture du Magasin des requêtes. Consultez [Options ALTER DATABASE SET](../t-sql/statements/alter-database-transact-sql-set-options.md).|
|`LIGHTWEIGHT_QUERY_PROFILING`| Nouvelle configuration délimitée à la base de données. Voir [`LIGHTWEIGHT_QUERY_PROFILING`](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#lqp). |
|`command` de la colonne `sys.dm_exec_requests` | Affiche `SELECT (STATMAN)` si `SELECT` attend la fin d’une opération synchrone de mise à jour des statistiques pour poursuivre l’exécution de la requête. Voir [`sys.dm_exec_requests`](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|
|`sys.dm_exec_query_plan_stats` | Nouvelle fonction de gestion dynamique (DMF) qui retourne l’équivalent du dernier plan d’exécution réel connu pour toutes les requêtes. Voir [sys.dm_exec_query_plan_stats](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).|
|`LAST_QUERY_PLAN_STATS` | Nouvelle configuration délimitée à la base de données qui active `sys.dm_exec_query_plan_stats`. Consultez la page [ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|
|`query_post_execution_plan_profile` | Événement étendu qui collecte l’équivalent d’un plan d’exécution réel basé sur le profilage léger, contrairement à `query_post_execution_showplan` qui utilise le profilage standard. Voir [Infrastructure du profilage de requête](../relational-databases/performance/query-profiling-infrastructure.md).|
|`sys.dm_db_page_info(database_id, file_id, page_id, mode)` | Nouvelle fonction DMF qui retourne des informations sur une page d’une base de données. Consultez [sys.dm_db_page_info (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="developer-experience"></a>Expérience développeur
[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] continue de fournir une expérience de développement de premier ordre grâce à des améliorations apportées aux types de données (graphe et spatial), à la prise en charge d’UTF-8 et à un nouveau framework d’extensibilité qui permet aux développeurs d’utiliser le langage de leur choix pour obtenir des insights sur toutes leurs données.

### <a name="graph"></a>Graph

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Actions de suppression en cascade des contraintes d’arête | Vous pouvez maintenant définir des actions de suppression en cascade au niveau d’une contrainte d’arête dans une base de données de graphe. Consultez [Contraintes d’arête](../relational-databases/tables/graph-edge-constraints.md). |
|Nouvelle fonction de graphique - `SHORTEST_PATH` | Vous pouvez maintenant utiliser `SHORTEST_PATH` à l’intérieur de `MATCH` pour trouver le chemin le plus court entre deux nœuds dans un graphe ou pour effectuer des traversées de longueur arbitraires.|
|Tables et index de partition| Les tables de graphe prennent désormais en charge le partitionnement des tables et des index. |
|Utilisation d’alias de tables dérivées ou de vues dans les requêtes de correspondance de graphe |Consultez [Requête de correspondance de graphique](../t-sql/queries/match-sql-graph.md). |
| &nbsp; | &nbsp; |

### <a name="unicode-support"></a>Prise en charge d’Unicode
Prend en charge des entreprises dans différents pays et régions, où l’exigence de fournir des applications et des services de base de données multilingues globaux est essentielle pour répondre aux demandes des clients et se conformer aux réglementations spécifiques du marché. 

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Prise en charge du codage de caractères UTF-8 |Prend en charge UTF-8 pour l’encodage d’importation et d’exportation, ainsi que le classement au niveau de la base de données ou au niveau des colonnes pour les données de chaîne. La prise en charge inclut les tables externes Polybase et Always Encrypted (si les enclaves ne sont pas utilisées). Voir [Prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md).|
| &nbsp; | &nbsp; |

### <a name="language-extensions"></a>Extensions de langage

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Nouveau SDK pour le langage Java | Simplifie le développement de programmes Java qui peuvent être exécutés à partir de SQL Server. Consultez [Kit de développement logiciel (SDK) d’extensibilité Microsoft pour Java pour SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md). |
|Le Kit de développement logiciel (SDK) du langage Java est open source |Le [SDK d’extensibilité Microsoft pour Java Microsoft SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md) est désormais open source et [disponible sur GitHub](https://github.com/microsoft/sql-server-language-extensions).|
|Prise en charge des types de données Java|Consultez [Types de données Java](../language-extensions/how-to/java-to-sql-data-types.md).|
|Nouveau runtime Java par défaut | SQL Server intègre désormais Azul Systems Zulu Embedded pour la prise en charge de Java du produit. Consultez [Free supported Java in SQL Server 2019 is now available](https://cloudblogs.microsoft.com/sqlserver/2019/07/24/free-supported-java-in-sql-server-2019-is-now-available/). |
|Extensions de langage SQL Server| Exécutez le code externe avec l’infrastructure d’extensibilité. Consultez [Extensions de langage SQL Server](../language-extensions/language-extensions-overview.md).
|Inscrire les langages externes|Un nouveau langage de définition de données (DDL), `CREATE EXTERNAL LANGUAGE`, enregistre les langages externes comme Java dans SQL Server. Consultez [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). |
| &nbsp; | &nbsp; |

### <a name="spatial"></a>spatial

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
| Nouveaux identificateurs de référence spatiale (SRID) |Le [GDA2020 australien](http://www.ga.gov.au/scientific-topics/positioning-navigation/geodesy/datums-projections/gda2020) fournit des données plus robustes et plus précises qui sont mieux alignées avec les systèmes GPS. Les nouveaux SRID sont :<ul><li>7843 pour la 2D géographique</li><li>7844pour la 3D géographique</li></ul> Pour obtenir les définitions des nouveaux SRID, consultez la vue [sys.spatial_reference_systems](../relational-databases/system-catalog-views/sys-spatial-reference-systems-transact-sql.md). |
| &nbsp; | &nbsp; |

### <a name="error-messages"></a>Messages d’erreur
Quand un processus ETL (extraction, transformation et chargement) échouait en raison d’une incompatibilité des types et/ou de la longueur des données entre la source et la destination, la résolution du problème prenait beaucoup de temps, surtout dans les jeux de données volumineux. [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] permet d’obtenir plus rapidement des insights sur les erreurs de troncation de données.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Avertissements détaillés sur la troncation | Le message d’erreur de troncation des données inclut par défaut les noms de tables et de colonnes, ainsi que la valeur tronquée. Voir [VERBOSE_TRUNCATION_WARNINGS](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#verbose-truncation).|
| &nbsp; | &nbsp; |

## <a name="mission-critical-security"></a>Sécurité stratégique
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fournit une architecture de sécurité conçue pour permettre aux développeurs et aux administrateurs de base de données de créer des applications de base de données sécurisées et de contrer les menaces. Chaque version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’appuie sur les améliorations des versions précédentes et introduit de nouvelles fonctionnalités. [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’inscrit dans cette lignée.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Always Encrypted avec enclaves sécurisées|S’étend sur Always Encrypted avec un chiffrement sur place et des calculs enrichis en activant des calculs sur les données de texte en clair à l’intérieur d’une enclave sécurisée côté serveur. Le chiffrement sur place améliore les performances et la fiabilité des opérations de chiffrement (chiffrement des colonnes, rotation des clés de chiffrement des colonnes, etc.), car il évite de déplacer des données en dehors de la base de données.<br><br> La prise en charge des calculs enrichis (correspondance à des modèles et opérations de comparaison) ouvre Always Encrypted à un ensemble beaucoup plus large de scénarios et d’applications qui demandent une protection des données sensibles tout en nécessitant des fonctionnalités plus riches dans les requêtes Transact-SQL. Consultez [Always Encrypted avec enclaves sécurisées](../relational-databases/security/encryption/always-encrypted-enclaves.md).|
|Gestion des certificats dans le Gestionnaire de configuration SQL Server|Les tâches de gestion des certificats, telles que l’affichage et le déploiement des certificats, sont désormais possibles à l’aide du Gestionnaire de configuration SQL Server. Consultez [Gestion des certificats (Gestionnaire de configuration SQL Server)](../database-engine/configure-windows/manage-certificates.md).|
|Découverte et classification des données|La découverte et classification des données fournit des fonctionnalités permettant de classer et d’étiqueter des colonnes dans des tables utilisateur. La classification de vos données sensibles (professionnelles, financières, médicales, PII, etc.) peuvent jouer un rôle essentiel dans la protection des informations d’une organisation. Elles peuvent servir d’infrastructure pour :<ul><li>Aider à répondre aux standards de confidentialité des données et aux exigences de conformité réglementaires</li><li>Différents scénarios de sécurité, comme la supervision (audit) et les alertes en cas d’accès anormal à des données sensibles</li><li>Faciliter l’identification des emplacements des données sensibles dans l’entreprise, afin que les administrateurs puissent prendre les bonnes mesures pour sécuriser la base de données</li></ul>|
|SQL Server Audit|[L’audit](../relational-databases/security/auditing/sql-server-audit-database-engine.md) a également été amélioré pour inclure un nouveau champ `data_sensitivity_information` dans l’enregistrement du journal d’audit, qui contient les classifications de sensibilité (étiquettes) des données réelles retournées par la requête. Pour obtenir des informations et des exemples, consultez [`ADD SENSITIVITY CLASSIFICATION`](../t-sql/statements/add-sensitivity-classification-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="high-availability"></a>Haute disponibilité
Quand vous déployez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous devez toujours vérifier que toutes les instances [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] critiques et les bases de données qu’elles contiennent sont disponibles chaque fois que l’entreprise et les utilisateurs finaux en ont besoin. La disponibilité est un pilier clé de la plateforme [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], et [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit de nombreuses nouveautés et améliorations qui permettent aux entreprises de s’assurer que leurs environnements de base de données sont hautement disponibles.

### <a name="availability-groups"></a>Groupes de disponibilité

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Jusqu’à cinq réplicas synchrones|[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] augmente le nombre maximal de réplicas synchrones à 5, contre 3 dans [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Vous pouvez configurer ce groupe de cinq réplicas de manière à instaurer le basculement automatique en son sein. Il existe un seul réplica principal, plus quatre réplicas secondaires synchrones.|
|Redirection de la connexion entre un réplica secondaire et un réplica principal| Elle permet de rediriger les connexions d’applications clientes vers le réplica principal, quel que soit le serveur cible spécifié dans la chaîne de connexion. Pour plus d’informations, consultez [Redirection de connexion en lecture/écriture depuis un réplica secondaire vers le réplica principal (groupes de disponibilité Always On)](../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).|
|Avantages de HADR| Chaque client Software Assurance de SQL Server pourra utiliser trois avantages améliorés pour toute version de SQL Server toujours prise en charge par Microsoft. Pour plus d’informations, lisez [notre annonce ici](https://cloudblogs.microsoft.com/sqlserver/2019/10/30/new-high-availability-and-disaster-recovery-benefits-for-sql-server/).
| &nbsp; | &nbsp; |

### <a name="recovery"></a>Récupération

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Récupération de base de données accélérée | Réduisez le temps de récupération après un redémarrage ou une restauration de transaction de longue durée grâce à la récupération accélérée des bases de données. Consultez [Récupération accélérée des bases de données](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).|
| &nbsp; | &nbsp; |

### <a name="resumable-operations"></a>Opérations pouvant être reprises

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Générer et régénérer des index columnstore en cluster en ligne | Voir [Exécuter des opérations d’index en ligne](../relational-databases/indexes/perform-index-operations-online.md). |
|Génération d’index rowstore en ligne pouvant être repris | Voir [Exécuter des opérations d’index en ligne](../relational-databases/indexes/perform-index-operations-online.md). |
|Suspendre et reprendre l’analyse initiale du chiffrement transparent des données (TDE)|Consultez [Analyse Transparent Data Encryption(TDE) - suspension et reprise](../relational-databases/security/encryption/transparent-data-encryption.md#scan-suspend-resume).|
| &nbsp; | &nbsp; |

## <a name="platform-choice"></a>Choix de la plateforme
[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur les innovations introduites dans [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] pour vous permettre d’exécuter [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur la plateforme de votre choix avec plus de fonctionnalités et de mesures de sécurité que jamais.

### <a name="linux"></a><a id="sql-server-on-linux"></a>Linux

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:-----|:-----|
|Prise en charge de la réplication | Consultez [Réplication SQL Server sur Linux](../linux/sql-server-linux-replication.md). |
|Prise en charge de MSDTC (Microsoft Distributed Transaction Coordinator) | Consultez [Comment configurer MSDTC sur Linux](../linux/sql-server-linux-configure-msdtc.md). |
|Prise en charge d’OpenLDAP pour les fournisseurs AD tiers | Voir le [tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](../linux/sql-server-linux-active-directory-authentication.md). |
|Machine Learning Services sur Linux | Consultez [Installer SQL Server Machine Learning Services (Python et R) sur Linux](../linux/sql-server-linux-setup-machine-learning.md). |
|Améliorations apportées à TempDB | Par défaut, une nouvelle installation de SQL Server sur Linux crée plusieurs fichiers de données TempDB en fonction du nombre de cœurs logiques (avec jusqu’à huit fichiers de données). Cela ne s’applique pas aux mises à niveau de versions mineures ou majeures sur place. Chaque fichier de TempDB fait 8 Mo avec une croissance automatique de 64 Mo. Ce comportement est similaire à l’installation de SQL Server par défaut sur Windows. |
|PolyBase sur Linux | Consultez [Installer PolyBase](../relational-databases/polybase/polybase-linux-setup.md) sur Linux pour les connecteurs non-Hadoop.<br/><br/>Consultez [Mappage de type PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Prise en charge de la capture des changements de données (CDC) | La capture des changements de données (CDC) est désormais prise en charge sur Linux pour [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]. |
| &nbsp; | &nbsp; |

### <a name="containers"></a>Containers
Pour commencer à travailler avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le moyen le plus simple consiste à utiliser des conteneurs. [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] s’appuie sur les innovations introduites dans les versions antérieures pour vous permettre de déployer des conteneurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur de nouvelles plateformes, de manière plus sécurisée et avec davantage de fonctionnalités.

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
| Registre de conteneurs Microsoft | Le [registre de conteneurs Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) remplace désormais Docker Hub pour les nouvelles images conteneur Microsoft officielles, notamment [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]. |
| Conteneurs non racines | [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] introduit la possibilité de créer des conteneurs plus sûrs en démarrant le processus [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] en tant qu’utilisateur non racine par défaut. Consultez [Générer et exécuter des conteneurs SQL Server en tant qu’utilisateur non-root](../linux/sql-server-linux-docker-container-deployment.md). |
| Images conteneur certifiées Red Hat | À compter de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)], vous pouvez exécuter des conteneurs SQL Server sur Red Hat Enterprise Linux. |
| Prise en charge de Polybase et de Machine Learning| [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] propose de nouvelles façons d’utiliser des conteneurs SQL Server comme Machine Learning Services et Polybase. Vous trouverez quelques exemples dans le [dépôt GitHub de conteneurs SQL Server](https://github.com/microsoft/mssql-docker/tree/master/linux/preview/examples). |
| &nbsp; | &nbsp; |

## <a name="setup-options"></a>Options d’installation

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---| 
|Nouvelles options de configuration de la mémoire | Définit les configurations de serveur *min server memory (Mo)* et *max server memory (Mo)* au cours de l’installation. Consultez la [page de configuration du moteur de base de données - Mémoire](./install/instance-configuration.md#memory) ainsi que les paramètres `USESQLRECOMMENDEDMEMORYLIMITS`, `SQLMINMEMORY` et `SQLMAXMEMORY` dans [Installer SQL Server à partir de l’invite de commandes](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). La valeur proposée s’aligne sur les lignes directrices de configuration de la mémoire dans les [options de configuration de la mémoire du serveur](../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).| 
|Nouvelles options d’installation du parallélisme | Définit la configuration du serveur de *degré maximal de parallélisme* pendant l’installation. Consultez la [page de configuration du moteur de base de données - MaxDOP](./install/instance-configuration.md#maxdop) ainsi que le paramètre `SQLMAXDOP` dans [Installer SQL Server à partir de l’invite de commandes](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). La valeur par défaut s’aligne sur les lignes directrices du degré maximal de parallélisme dans [Configurer l’option de configuration du serveur de degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).| 
|Avertissement d’installation sur la clé de produit de licence serveur/CAL|Si une clé de produit de licence Enterprise Server/CAL est entrée et que l’ordinateur a plus de 20 cœurs physiques, ou 40 cœurs logiques lorsque l’hyper-threading est activé, un avertissement s’affiche lors de l’installation. Les utilisateurs peuvent toujours prendre note de la limitation et continuer l’installation, ou entrer une clé de licence qui prend en charge le nombre maximal de processeurs du système d’exploitation.|
| &nbsp; | &nbsp; |

## <a name="sql-server-machine-learning-services"></a><a id="ml"></a> SQL Server Machine Learning Services

|Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Modélisation basée sur une partition | Vous pouvez traiter les scripts externes par partition des données en utilisant les nouveaux paramètres ajoutés à `sp_execute_external_script`. Cette fonctionnalité prend en charge l’entraînement de nombreux petits modèles (un modèle par partition de données) au lieu d’un grand modèle. Consultez [Créer des modèles basés sur une partition](../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).|
|Cluster de basculement Windows Server| Vous pouvez configurer la haute disponibilité pour Machine Learning Services sur un cluster de basculement Windows Server.|
| &nbsp; | &nbsp; |

## <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

Cette version introduit de nouvelles fonctionnalités et améliorations en matière de performances, de gouvernance des ressources et de prise en charge des clients.

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Groupes de calcul dans les modèles tabulaires| Les groupes de calcul peuvent réduire de manière significative le nombre de mesures redondantes en regroupant les expressions de mesure courantes en tant qu’*éléments de calcul*. Pour en savoir plus, consultez [Groupes de calcul dans le modèle tabulaire](/analysis-services/tabular-models/calculation-groups). |
|Entrelacement de requêtes| L’entrelacement de requêtes est une configuration système en mode tabulaire qui peut améliorer les temps de réponse des requêtes utilisateur dans des scénarios à concurrence élevée. Pour en savoir plus, consultez [Entrelacement de requêtes](/analysis-services/tabular-models/query-interleaving). |
|Relations plusieurs-à-plusieurs dans les modèles tabulaires| Autorise les relations plusieurs-à-plusieurs entre les tables où les deux colonnes ne sont pas uniques. Pour en savoir plus, consultez [Relations dans les modèles tabulaires](/analysis-services/tabular-models/relationships-ssas-tabular).|
|Paramètres de propriété pour la gouvernance des ressources| Cette version comprend de nouveaux paramètres de mémoire : Memory\QueryMemoryLimit, DbpropMsmdRequestMemoryLimit, et OLAP\Query\RowsetSerializationLimit pour la gouvernance des ressources. Pour plus d’informations, consultez [Paramètres de mémoire](/analysis-services/server-properties/memory-properties).|
|Paramètre de gouvernance pour les actualisations du cache Power BI | Cette version introduit la propriété ClientCacheRefreshPolicy qui remplace la mise en cache des données de vignette de tableau de bord et des données de rapport pour la charge initiale des rapports Live Connect par le service Power BI. Pour plus d’informations, consultez [Propriétés générales](/analysis-services/server-properties/general-properties). |
| Attachement en ligne  | L’attachement en ligne peut être utilisé pour la synchronisation des réplicas en lecture seule dans les environnements locaux de scale-out des requêtes. Pour en savoir plus, consultez [Attachement en ligne](/analysis-services/what-s-new-in-sql-server-analysis-services#online-attach). |
| &nbsp; | &nbsp; |

## <a name="sql-server-integration-services"></a>SQL Server Integration Services

Cette version introduit de nouvelles fonctionnalités pour améliorer les opérations sur les fichiers.

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Tâche flexible sur les fichiers |Effectuez des opérations sur des fichiers dans le système de fichiers local, Stockage Blob Azure et Azure Data Lake Storage Gen2. Consultez [Tâche flexible sur les fichiers](../integration-services/control-flow/flexible-file-task.md).|
|Source et destination des fichiers flexibles |Lisez et écrivez des données pour Stockage Blob Azure et Azure Data Lake Storage Gen2. Consultez [Source de fichier flexible](../integration-services/data-flow/flexible-file-source.md) et [Destination de fichier flexible](../integration-services/data-flow/flexible-file-destination.md). |

## <a name="sql-server-master-data-services"></a>SQL Server [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]

| Nouvelle fonctionnalité ou mise à jour | Détails |
|:---|:---|
|Prise en charge des bases de données Azure SQL Managed Instance| Hébergez [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)] sur Azure SQL Managed Instance. Consultez [Installation et configuration de [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]](../master-data-services/master-data-services-installation-and-configuration.md#SetUpWeb).|
|Nouveaux contrôles HTML| Les contrôles HTML remplacent tous les anciens composants Silverlight. La dépendance à Silverlight a été supprimée.|
| &nbsp; | &nbsp; |

## <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

Cette version de SQL Server Reporting Services prend en charge les instances managées SQL Azure, les jeux de données Power BI Premium, l’accessibilité améliorée, le proxy d’application Azure Active Directory et Transparent Database Encryption. Elle met également à jour le Générateur de rapports Microsoft. Pour obtenir des détails, consultez [Nouveautés de SQL Server Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="see-also"></a>Voir aussi

- [`SqlServer` Module PowerShell](https://www.powershellgallery.com/packages/Sqlserver)
- [Documentation SQL Server PowerShell](../powershell/sql-server-powershell.md)

## <a name="next-steps"></a>Étapes suivantes

- [Ateliers SQL Server](https://aka.ms/sqlworkshops)
- [Notes de publication de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](sql-server-version-15-release-notes.md)
- [Microsoft [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] : Livre blanc technique](https://aka.ms/sql2019whitepaper)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

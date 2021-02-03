---
title: Vue d’ensemble et scénarios d’utilisation | Microsoft Docs
description: Découvrez l’OLTP en mémoire, une technologie dans SQL Server et Azure SQL Database pour le traitement transactionnel optimisé. Passez en revue des exemples et des ressources supplémentaires.
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 62c964c5-eae4-4cf1-9024-d5a19adbd652
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8492183591b2069115e8da8d05d89759f5eee46d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235261"
---
# <a name="overview-and-usage-scenarios"></a>Vue d’ensemble et scénarios d’utilisation

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

OLTP en mémoire est la technologie de premier plan disponible dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)] pour optimiser les performances du traitement transactionnel, de l’ingestion et du chargement des données, et des scénarios de données temporaires. Cet article inclut une vue d’ensemble de cette technologie et une présentation des scénarios d’usage de l’OLTP en mémoire. Grâce à ces informations, vous pourrez déterminer si l’OLTP en mémoire est adapté à votre application. À la fin de cet article, vous trouverez un exemple illustrant les objets de l’OLTP en mémoire, ainsi que des liens vers une démonstration des performances de cette technologie et vers des ressources que vous pourrez utiliser pour la suite.

Cet article présente la technologie OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Le billet de blog suivant contient une description détaillée des avantages en matière de performances et d’utilisation des ressources dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] : [OLTP en mémoire dans Azure SQL Database](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

## <a name="in-memory-oltp-overview"></a>Vue d’ensemble de l’OLTP en mémoire

L’OLTP en mémoire peut offrir des gains de performance considérables pour les charges de travail appropriées. Un client, BWIN, a réussi à [atteindre 1,2 million de requêtes par seconde](/archive/blogs/sqlcat/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale) avec une seule machine exécutant [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], en tirant profit de l’OLTP en mémoire. Un autre client, Quorum, est parvenu à doubler sa charge de travail tout en [réduisant son utilisation des ressources de 70 %](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database), grâce à la fonctionnalité OLTP en mémoire disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Si certains clients ont constaté un gain de performances multiplié par 30 dans certains cas, les gains réellement obtenus dépendent de la charge de travail.

Mais d’où proviennent exactement ces gains de performance ? En substance, l’OLTP en mémoire améliore les performances de traitement transactionnel en rendant l’accès aux données et l’exécution des transactions plus efficaces, et en supprimant la contention de verrous et de verrous internes entre les transactions exécutées simultanément. Cette technologie n’est pas rapide parce qu’elle est en mémoire, mais parce qu’elle est optimisée à travers la présence des données en mémoire. Les algorithmes de stockage des données, d’accès et de traitement ont été entièrement repensés pour tirer parti des dernières améliorations en matière de calcul en mémoire et haute simultanéité.

Le fait que les données se trouvent en mémoire ne signifie pas pour autant que vous les perdez en cas de défaillance. Par défaut, toutes les transactions présentent une durabilité complète. Vous bénéficiez donc des mêmes garanties de durabilité que pour toute autre table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : dans le cadre de la validation de transaction, toutes les modifications sont écrites dans le journal des transactions sur le disque. Si une défaillance survient après la validation de la transaction, vos données sont présentes lorsque la base de données est remise en ligne. De plus, l’OLTP en mémoire est compatible avec toutes les fonctionnalités de haute disponibilité et de récupération d’urgence de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme Always On, la sauvegarde et la restauration, etc.

Pour tirer parti de l’OLTP en mémoire dans votre base de données, vous devez utiliser un ou plusieurs des types d’objets suivants :

- Les *tables optimisées en mémoire* servent à stocker les données utilisateur. Vous déclarez qu’une table doit être optimisée en mémoire au moment de sa création.
- Les *tables non durables* sont utilisées pour les données temporaires, soit pour la mise en cache, soit pour le jeu de résultats intermédiaire (à la place des tables temporaires traditionnelles). Une table non durable est une table optimisée en mémoire qui est déclarée avec DURABILITY=SCHEMA_ONLY, ce qui veut dire que les modifications apportées à ces tables n’entraînent aucune E/S. Cela évite la consommation de ressources d’E/S de journal lorsque la durabilité n’est pas un critère important.
- Les *types de tables optimisées en mémoire* sont utilisés pour les paramètres table, ainsi que pour les jeux de résultats intermédiaires dans les procédures stockées. Ils peuvent être utilisés au lieu des types de tables traditionnels. Les variables de table et les paramètres table qui sont déclarés à l’aide d’un type de table optimisée en mémoire héritent des avantages des tables optimisées en mémoire non durables : accès efficace aux données et absence d’E/S.
- Les *modules T-SQL compilés en mode natif* permettent d’accélérer encore plus l’exécution d’une transaction individuelle en réduisant les cycles processeur requis pour traiter les opérations. Vous déclarez qu’un module Transact-SQL doit être compilé en mode natif au moment de sa création. Les modules T-SQL suivants peuvent être compilés en mode natif : procédures stockées, déclencheurs et fonctions scalaires définies par l’utilisateur.

OLTP en mémoire est intégré à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Et comme ces objets se comportent de façon similaire aux objets standard équivalents, vous pouvez généralement améliorer les performances simplement en apportant quelques modifications minimes à la base de données et à l’application. De plus, vous pouvez avoir des tables optimisées en mémoire et des tables sur disque traditionnelles dans la même base de données, et exécuter simultanément des requêtes sur ces deux types de tables. Un script Transact-SQL présentant un exemple de chacun de ces types d’objets est fourni vers la fin de cet article.

## <a name="usage-scenarios-for-in-memory-oltp"></a>Scénarios d’usage de l’OLTP en mémoire

L’OLTP en mémoire n’est pas un bouton d’accélération magique et ne convient pas à toutes les charges de travail. Par exemple, les tables à mémoire optimisée ne réduisent pas l’utilisation du processeur si la plupart des requêtes effectuent des opérations d’agrégation sur de grandes plages de données (dans ce scénario, utilisez plutôt des index columnstore).

Voici une liste de scénarios et de modèles d’application pour lesquels des clients ont pu tirer profit de l’OLTP en mémoire.

### <a name="high-throughput-and-low-latency-transaction-processing"></a>Traitement transactionnel à débit élevé et latence faible

C’est le scénario principal pour lequel nous avons créé l’OLTP en mémoire : prendre en charge de grands volumes de transactions, avec une latence faible homogène pour les transactions individuelles.

Les scénarios de charge de travail les plus fréquents sont les suivants : négoce d’instruments financiers, paris sportifs, jeux mobiles et diffusion publicitaire. Un autre modèle courant observé est un « catalogue » souvent lu et/ou mis à jour. Par exemple, vous avez des fichiers volumineux, qui sont répartis sur plusieurs nœuds de cluster, et vous cataloguez l’emplacement de chaque partition de fichier dans une table à mémoire optimisée.

#### <a name="implementation-considerations"></a>Considérations relatives à l’implémentation

Utilisez des tables optimisées en mémoire pour vos tables de transactions principales, c’est-à-dire pour les tables qui présentent les transactions les plus critiques pour les performances. Utilisez des procédures stockées compilées en mode natif pour optimiser l’exécution de la logique associée à la transaction commerciale. Plus vous pourrez transmettre la logique aux procédures stockées dans la base de données, plus vous tirerez profit de l’OLTP en mémoire.

Pour commencer avec une application existante :

1. Utilisez le [rapport d’analyse des performances de transaction](determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) pour identifier les objets à migrer.
2. Utilisez le [Conseiller d’optimisation de la mémoire](memory-optimization-advisor.md) et le [Conseiller de compilation native](native-compilation-advisor.md) pour faciliter la migration.

#### <a name="customer-case-studies"></a>Études de cas clients

- CMC Markets utilise l’OLTP en mémoire dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] pour obtenir une latence faible homogène : [Because a second is too long to wait, this financial services firm is updating its trading software now.](https://customers.microsoft.com/story/because-a-second-is-too-long-to-wait-this-financial-services-firm-is-updating-its-trading-software) (Une seconde étant un délai d’attente trop long, ce cabinet de services financiers met à jour son logiciel de trading maintenant.)
- Derivco utilise l’OLTP en mémoire dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] pour prendre en charge les hausses de débit et gérer les pics de charge de travail : [When an online gaming company doesn't want to risk its future, it bets on [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].](https://customers.microsoft.com/story/when-an-online-gaming-company-doesnt-want-to-risk-its-future-it-bets-on-sql-server-2016) (Quand une entreprise de jeux en ligne ne veut pas mettre son avenir en danger, elle parie sur SQL Server 2015.)

### <a name="data-ingestion-including-iot-internet-of-things"></a>Intégration de données, IoT (Internet des objets) compris

L’OLTP en mémoire est efficace pour ingérer en même temps d’importants volumes de données provenant de nombreuses sources différentes. Il est également souvent plus intéressant d’ingérer des données dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] plutôt que dans d’autres destinations, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rend l’exécution des requêtes sur les données plus rapide et vous permet d’obtenir des insights en temps réel.

Les modèles d’application courants sont les suivants :

- ingestion de relevés et d’événements de capteurs à des fins de notification et d’analyse d’historique ;
- gestion des mises à jour par lot, même à partir de plusieurs sources, tout en réduisant l’impact sur la charge de travail de lecture simultanée.

#### <a name="implementation-considerations"></a>Considérations relatives à l’implémentation

Utilisez une table optimisée en mémoire pour l’intégration de données. Si l’intégration consiste principalement en des insertions (plutôt que des mises à jour) et l’encombrement de stockage des données dans l’OLTP en mémoire est un critère important :

- Utilisez un travail pour décharger régulièrement les données par lot dans une table sur disque comportant un [index Columnstore cluster](../indexes/columnstore-indexes-overview.md), à l’aide d’un travail qui exécute `INSERT INTO <disk-based table> SELECT FROM <memory-optimized table>`; ou
- Utilisez une [table à mémoire optimisée temporelle](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md) pour gérer les données d’historique ; dans ce mode, les données d’historique se trouvent sur le disque et le déplacement des données est géré par le système.

Le référentiel d’exemples [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient une application de réseau de distribution d’électricité intelligent qui utilise une table temporelle à mémoire optimisée, un type de table à mémoire optimisée et une procédure stockée compilée en mode natif afin d’accélérer l’ingestion des données tout en gérant l’encombrement de stockage des données de capteur dans l’OLTP en mémoire :

- [smart-grid-release](https://github.com/Microsoft/sql-server-samples/releases/tag/iot-smart-grid-v1.0)
- [smart-grid-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/iot-smart-grid)

#### <a name="customer-case-studies"></a>Études de cas clients

- [Quorum doubles key database’s workload while lowering utilization by 70% by leveraging In-Memory OLTP in Azure SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)
- EdgeNet a amélioré les performances de chargement de données par lot et supprimé la nécessité de maintenir un cache de niveau intermédiaire grâce à l’OLTP en mémoire dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] : [Data Services Firm Gains Real-Time Access to Product Data with In-Memory Technology](https://customers.microsoft.com/story/data-services-firm-gains-real-time-access-to-product-d) (Une entreprise de services de données profite d’un accès en temps réel aux données de produits grâce à une technologie en mémoire)
- L’établissement Beth Israel Deaconess Medical Center a pu améliorer considérablement la vitesse d’ingestion des données des contrôleurs de domaine et gère efficacement les pics de charge de travail à l’aide de l’OLTP en mémoire dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] : https://customers.microsoft.com/story/strengthening-data-security-and-creating-more-time-for

### <a name="caching-and-session-state"></a>Mise en cache et état de session

La technologie d’OLTP en mémoire rend SQL vraiment intéressant pour le maintien de l’état de session (par exemple, pour une application ASP.NET) et pour la mise en cache.

L’état de session ASP.NET est un cas d’utilisation très efficace pour l’OLTP en mémoire. Avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un client est parvenu à atteindre 1,2 million de requêtes par seconde. Dans le même temps, il a commencé à utiliser l’OLTP en mémoire pour les besoins de mise en cache de toutes les applications de niveau intermédiaire de l’entreprise. Détails : [How bwin is using [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] In-Memory OLTP to achieve unprecedented performance and scale](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) (Comment bwin utilise la technologie OLTP en mémoire de SQL Server 2015 pour atteindre des performances et un nombre d’utilisateurs sans précédent)

#### <a name="implementation-considerations"></a>Considérations relatives à l’implémentation

Vous pouvez utiliser des tables à mémoire optimisée non durables comme magasin clé-valeur simple en stockant un objet BLOB dans une colonne varbinary(max). Vous pouvez aussi implémenter un cache semi-structuré avec [prise en charge JSON](https://azure.microsoft.com/blog/json-support-is-generally-available-in-azure-sql-database/) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Enfin, vous pouvez créer un cache relationnel complet via des tables non durables présentant un schéma relationnel complet, avec divers types et contraintes de données.

Pour bien démarrer, utilisez l’état de session ASP.NET à mémoire optimisée en tirant profit des scripts publiés sur GitHub afin de remplacer les objets créés par le fournisseur d’état de session intégré de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : [aspnet-session-state](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/aspnet-session-state)

#### <a name="customer-case-studies"></a>Études de cas clients

- bwin a pu nettement augmenter le débit et réduire l’encombrement matériel pour l’état de session ASP.NET avec l’OLTP en mémoire dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] : [Gaming Site Can Scale to 250,000 Requests Per Second and Improve Player Experience](https://customers.microsoft.com/story/gaming-site-can-scale-to-250000-requests-per-second-an) (Un site de jeux peut prendre en charge jusqu’à 250 000 requêtes par seconde afin d’optimiser l’expérience des joueurs)
- bwin a augmenté encore plus le débit avec l’état de session ASP.NET et implémenté un système de mise en cache de niveau intermédiaire à l’échelle de l’entreprise grâce à l’OLTP en mémoire dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] : [How bwin is using [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] In-Memory OLTP to achieve unprecedented performance and scale](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) (Comment bwin utilise la technologie OLTP en mémoire de SQL Server 2015 pour atteindre des performances et un nombre d’utilisateurs sans précédent)

### <a name="tempdb-object-replacement"></a>Remplacement d’objet tempdb

Utilisez des tables non durables et des types de tables à mémoire optimisée pour remplacer vos structures tempdb standard, telles que les tables temporaires, les variables de table et les paramètres table.

Les variables de table et les tables non durables optimisées en mémoire réduisent généralement l’utilisation du processeur par rapport aux variables de table et aux tables #temp traditionnels, et suppriment complètement les E/S de journal.

#### <a name="implementation-considerations"></a>Considérations relatives à l’implémentation

Pour commencer, consultez : [Improving temp table and table variable performance using memory optimization.](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization) (Amélioration des performances des tables temporaires et des variables de table à l’aide de l’optimisation de la mémoire.)

#### <a name="customer-case-studies"></a>Études de cas clients

- Un client est parvenu à améliorer les performances de 40 % simplement en remplaçant les paramètres table traditionnels par des paramètres table à mémoire optimisée : [High Speed IoT Data Ingestion Using In-Memory OLTP in Azure](/archive/blogs/sqlserverstorageengine/a-technical-case-study-high-speed-iot-data-ingestion-using-in-memory-oltp-in-azure) (Intégration de données IoT haute vitesse à l’aide de l’OLTP en mémoire dans Azure)
- SentryOne a considérablement amélioré sa capacité d’ingestion des données grâce à une latence quasi nulle dans sa solution de supervision. Pour cela, l’entreprise a remplacé les tables tempdb par les tables OLTP en mémoire, dans le cadre de ses améliorations de scalabilité d’entreprise : [Solution provider breaks through performance ceiling with data monitoring innovation.](https://customers.microsoft.com/story/sentryone-partner-professional-services-sql-server-azure) (Un fournisseur de solution atteint des performances inédites en innovant dans le domaine de la supervision des données.)

### <a name="etl-extract-transform-load"></a>ETL (extraction, transformation, chargement)

Les flux de travail ETL incluent souvent le chargement de données dans une table de mise en lots, les transformations de données et le chargement dans les tables finales.

#### <a name="implementation-considerations"></a>Considérations relatives à l’implémentation

Utilisez des tables optimisées en mémoire non durables pour la mise en lots des données. Elles suppriment complètement les E/S et optimisent l’efficacité de l’accès aux données.

Si vous effectuez des transformations sur la table de mise en lots dans le cadre du flux de travail, vous pouvez utiliser des procédures stockées compilées en mode natif pour accélérer ces transformations. Si vous pouvez procéder à ces transformations en parallèle, l’optimisation de la mémoire vous offre des avantages supplémentaires en matière de mise à l’échelle.

## <a name="sample-script"></a>Exemple de script

Avant de pouvoir commencer à utiliser l’OLTP en mémoire, vous devez créer un groupe de fichiers MEMORY_OPTIMIZED_DATA. Nous vous recommandons également d’utiliser le niveau de compatibilité de base de données 130 (ou supérieur) et de définir l’option de base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT sur ON.

Vous pouvez utiliser le script situé à l’emplacement suivant pour créer le groupe de fichiers dans le dossier de données par défaut et configurer les paramètres recommandés :

- [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)

Le script suivant illustre les objets de l’OLTP en mémoire que vous pouvez créer dans votre base de données :

```sql
-- configure recommended DB option
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
GO
-- memory-optimized table
CREATE TABLE dbo.table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- non-durable table
CREATE TABLE dbo.temp_table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON,
      DURABILITY=SCHEMA_ONLY)
GO
-- memory-optimized table type
CREATE TYPE dbo.tt_table1 AS TABLE
( c1 INT IDENTITY,
  c2 NVARCHAR(MAX),
  is_transient BIT NOT NULL DEFAULT (0),
  INDEX ix_c1 HASH (c1) WITH (BUCKET_COUNT=1024))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- natively compiled stored procedure
CREATE PROCEDURE dbo.usp_ingest_table1
  @table1 dbo.tt_table1 READONLY
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC
    WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT,
          LANGUAGE=N'us_english')

  DECLARE @i INT = 1

  WHILE @i > 0
  BEGIN
    INSERT dbo.table1
    SELECT c2
    FROM @table1
    WHERE c1 = @i AND is_transient=0

    IF @@ROWCOUNT > 0
      SET @i += 1
    ELSE
    BEGIN
      INSERT dbo.temp_table1
      SELECT c2
      FROM @table1
      WHERE c1 = @i AND is_transient=1

      IF @@ROWCOUNT > 0
        SET @i += 1
      ELSE
        SET @i = 0
    END
  END

END
GO
-- sample execution of the proc
DECLARE @table1 dbo.tt_table1
INSERT @table1 (c2, is_transient) VALUES (N'sample durable', 0)
INSERT @table1 (c2, is_transient) VALUES (N'sample non-durable', 1)
EXECUTE dbo.usp_ingest_table1 @table1=@table1
SELECT c1, c2 from dbo.table1
SELECT c1, c2 from dbo.temp_table1
GO
```

## <a name="resources-to-learn-more"></a>Ressources supplémentaires

- [Technologies OLTP en mémoire pour des performances T-SQL plus rapides](./survey-of-initial-areas-in-in-memory-oltp.md)
- Vous trouverez la démonstration des performances avec l’OLTP en mémoire ici : [in-memory-oltp-perf-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- [Vidéo de 17 minutes expliquant l’OLTP en mémoire et présentant la démonstration](in-memory-oltp-in-memory-optimization.md#anchorname-17minute-video)
- [Script pour activer l’OLTP en mémoire et définir les options recommandées](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)
- [Documentation principale sur l’OLTP en mémoire](in-memory-oltp-in-memory-optimization.md)
- [Avantages en matière de performances et d’utilisation des ressources de l’OLTP en mémoire dans Azure SQL Database](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Improving temp table and table variable performance using memory optimization (Amélioration des performances des tables temporaires et des variables de table à l’aide de l’optimisation de la mémoire)](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)
- [Optimiser les performances à l’aide des technologies en mémoire dans SQL Database](/azure/sql-database/sql-database-in-memory)
- [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)

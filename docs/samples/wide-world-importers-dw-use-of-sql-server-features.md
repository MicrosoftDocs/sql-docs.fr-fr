---
title: Principales fonctionnalités de la base de données DW WideWorldImporters
description: Découvrez comment la base de données WideWorldImportersDW présente les principales fonctionnalités de SQL Server qui conviennent à l’entreposage et à l’analyse des données.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 07/01/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: ad1c38ab7c479497c50e84b305fe23d1441501d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056957"
---
# <a name="wideworldimportersdw-use-of-sql-server-features-and-capabilities"></a>WideWorldImportersDW l’utilisation des fonctionnalités et des fonctionnalités de SQL Server
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW est conçu pour présenter un grand nombre des principales fonctionnalités de SQL Server qui conviennent à l’entreposage et à l’analyse des données. La liste suivante répertorie les fonctionnalités et fonctionnalités de SQL Server, ainsi qu’une description de leur utilisation dans WideWorldImportersDW.

## <a name="polybase"></a>PolyBase

[S’applique à SQL Server (2016 et versions ultérieures)]

Polybase est utilisé pour combiner les informations de ventes de WideWorldImportersDW avec un jeu de données public sur les données démographiques afin de comprendre les villes susceptibles d’intéresser l’augmentation des ventes.

Pour activer l’utilisation de Polybase dans l’exemple de base de données, assurez-vous qu’il est installé, puis exécutez la procédure stockée suivante dans la base de données :

```sql
EXECUTE [Application].[Configuration_ApplyPolyBase]
```

Cette opération crée une table externe `dbo.CityPopulationStatistics` qui fait référence à un jeu de données public qui contient les données de remplissage des villes du États-Unis, hébergées dans le stockage d’objets BLOB Azure. Il est recommandé d’examiner le code de la procédure stockée pour comprendre le processus de configuration. Si vous souhaitez héberger vos propres données dans le stockage d’objets BLOB Azure et maintenir la sécurité d’un accès public général, vous devrez effectuer des étapes de configuration supplémentaires. La requête suivante retourne les données de ce jeu de données externe :

```sql
SELECT
        CityID, StateProvinceCode, CityName,
        YearNumber, LatestRecordedPopulation
    FROM
        dbo.CityPopulationStatistics;
```

Pour comprendre les villes qui peuvent présenter un intérêt pour une expansion supplémentaire, la requête suivante examine le taux de croissance des villes et retourne les plus grandes villes 100 les plus importantes avec une croissance significative, et où les importateurs étendus n’ont pas de présence de ventes. La requête implique une jointure entre la table distante `dbo.CityPopulationStatistics` et la table locale `Dimension.City` , ainsi qu’un filtre impliquant la table locale `Fact.Sales` .

```sql
WITH PotentialCities
AS
(
    SELECT cps.CityName,
            cps.StateProvinceCode,
            MAX(cps.LatestRecordedPopulation) AS PopulationIn2016,
            (MAX(cps.LatestRecordedPopulation) - MIN(cps.LatestRecordedPopulation)) * 100.0
                / MIN(cps.LatestRecordedPopulation) AS GrowthRate
    FROM dbo.CityPopulationStatistics AS cps
    WHERE cps.LatestRecordedPopulation IS NOT NULL
    AND cps.LatestRecordedPopulation <> 0
    GROUP BY cps.CityName, cps.StateProvinceCode
),
InterestingCities
AS
(
    SELECT DISTINCT pc.CityName,
                    pc.StateProvinceCode,
                    pc.PopulationIn2016,
                    FLOOR(pc.GrowthRate) AS GrowthRate
    FROM PotentialCities AS pc
    INNER JOIN Dimension.City AS c
    ON pc.CityName = c.City
    WHERE GrowthRate > 2.0
    AND NOT EXISTS (SELECT 1 FROM Fact.Sale AS s WHERE s.[City Key] = c.[City Key])
)
SELECT TOP(100) CityName, StateProvinceCode, PopulationIn2016, GrowthRate
FROM InterestingCities
ORDER BY PopulationIn2016 DESC;
```

## <a name="clustered-columnstore-indexes"></a>Index columnstore cluster

(Version complète de l’exemple)

Les index ColumnStore cluster (ICC) sont utilisés avec toutes les tables de faits, afin de réduire l’encombrement du stockage et d’améliorer les performances des requêtes. Avec l’ICC, le stockage de base pour les tables de faits utilise la compression de colonne.

Les index non cluster sont utilisés en plus de l’index ColumnStore cluster pour faciliter les contraintes de clé primaire et de clé étrangère. Ces contraintes ont été ajoutées par prudence : le processus ETL sourcete les données de la base de données WideWorldImporters, qui a des contraintes pour appliquer l’intégrité. La suppression des contraintes de clé primaire et étrangère, et de leurs index de prise en charge, réduirait l’encombrement de stockage des tables de faits.

**Taille des données**

L’exemple de base de données a une taille de données limitée, pour faciliter le téléchargement et l’installation de l’exemple. Toutefois, pour voir les véritables avantages en matière de performances des index ColumnStore, vous pouvez utiliser un plus grand jeu de données.

Vous pouvez exécuter l’instruction suivante pour augmenter la taille de la `Fact.Sales` table en insérant une autre 12 millions lignes d’exemples de données. Ces lignes sont toutes insérées pour l’année 2012, de sorte qu’il n’y a aucune interférence avec le processus ETL.

```sql
    EXECUTE [Application].[Configuration_PopulateLargeSaleTable]
```

L’exécution de cette instruction prendra environ 5 minutes. Pour insérer plus de 12 millions lignes, transmettez le nombre de lignes souhaité à insérer en tant que paramètre à cette procédure stockée.

Pour comparer les performances des requêtes avec et sans ColumnStore, vous pouvez supprimer et/ou recréer l’index cluster ColumnStore.

Pour supprimer l’index :

```sql
 DROP INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

Pour recréer :

```sql
CREATE CLUSTERED COLUMNSTORE INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

## <a name="partitioning"></a>Partitionnement

(Version complète de l’exemple)

La taille des données dans une Data Warehouse peut devenir très importante. Par conséquent, il est recommandé d’utiliser le partitionnement pour gérer le stockage des tables volumineuses dans la base de données.

Toutes les tables de faits plus volumineuses sont partitionnées par année. La seule exception est `Fact.Stock Holdings` , qui n’est pas basée sur la date et a une taille de données limitée par rapport aux autres tables de faits.

La fonction de partition utilisée pour toutes les tables partitionnées est `PF_Date` , et le schéma de partition utilisé est `PS_Date` .

## <a name="in-memory-oltp"></a>OLTP en mémoire

(Version complète de l’exemple)

WideWorldImportersDW utilise SCHEMA_ONLY tables optimisées en mémoire pour les tables de mise en lots. Toutes les `Integration.` * `_Staging` tables sont SCHEMA_ONLY les tables mémoire optimisées.

L’avantage des tables SCHEMA_ONLY est qu’elles ne sont pas journalisées et ne nécessitent pas d’accès au disque. Cela améliore les performances du processus ETL. Étant donné que ces tables ne sont pas journalisées, leur contenu est perdu en cas de défaillance. Toutefois, la source de données est toujours disponible, de sorte que le processus ETL peut simplement être redémarré en cas de défaillance.

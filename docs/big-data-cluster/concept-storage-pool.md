---
title: Qu’est-ce que le pool de stockage ?
titleSuffix: SQL Server big data clusters
description: Découvrez le rôle du pool de stockage SQL Server dans un cluster Big Data SQL Server 2019, ainsi que l’architecture et les fonctionnalités d’un pool de stockage SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9d9b74b3d0f3822152927197b71d77731a5c775c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047430"
---
# <a name="what-is-the-storage-pool-in-a-sql-server-big-data-cluster"></a>Qu’est-ce que le pool de stockage dans un cluster Big Data SQL Server ?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit le rôle du *pool de stockage SQL Server* dans un cluster Big Data SQL Server. Les sections suivantes décrivent l’architecture et les fonctionnalités d’un pool de stockage.

## <a name="storage-pool-architecture"></a>Architecture du pool de stockage

Le pool de stockage est le cluster HDFS (Hadoop) local d’un cluster Big Data SQL Server. Il fournit un stockage persistant pour les données non structurées et semi-structurées. Les fichiers de données, comme les fichiers Parquet ou de texte délimité, peuvent être stockés dans le pool de stockage. Pour rendre le stockage persistant, chaque pod du pool est attaché à un volume persistant. Les fichiers de pool de stockage sont accessibles via [PolyBase](../relational-databases/polybase/polybase-guide.md) par le biais de SQL Server ou directement à l’aide d’une passerelle Apache Knox.

Une configuration HDFS classique se compose d’un ensemble d’ordinateurs matériels auxquels du stockage est attaché. Les données sont réparties en blocs entre les différents nœuds pour la tolérance de panne et l’exploitation du traitement parallèle. L’un des nœuds du cluster fonctionne comme le nœud de nom et contient les informations de métadonnées relatives aux fichiers se trouvant dans les nœuds de données.

![Configuration HDFS classique](media/concept-storage-pool/classic-hdfs-setup.png)

Le pool de stockage est constitué de nœuds de stockage qui sont membres d’un cluster HDFS. Il exécute un ou plusieurs pods Kubernetes avec chaque pod hébergeant les conteneurs suivants :

- Un conteneur Hadoop lié à un volume persistant (stockage). Tous les conteneurs de ce type forment ensemble le cluster Hadoop. Dans le conteneur Hadoop se trouve un processus de gestionnaire de nœuds YARN qui peut créer des processus Worker Apache Spark à la demande. Le nœud principal Spark héberge le metastore Hive, l’historique Spark et les conteneurs de l’historique des travaux YARN.
- Une instance SQL Server pour lire des données à partir de HDFS à l’aide de la technologie OpenRowSet.
- `collectd` pour la collecte de données de métriques.
- `fluentbit` pour la collecte de données de journal.

![architecture d’un pool de stockage](media/concept-storage-pool/scale-big-data-on-demand.png)

## <a name="responsibilities"></a>Responsabilités

Les nœuds de stockage sont responsables des opérations suivantes :

- Ingestion de données par le biais d’Apache Spark.
- Stockage des données dans HDFS (format Parquet et texte délimité). HDFS assure également la persistance des données, car les données HDFS sont réparties sur tous les nœuds de stockage du cluster Big Data (BDC) SQL.
- Accès aux données par le biais des points de terminaison HDFS et SQL Server.

## <a name="accessing-data"></a>Accès aux données

Les principales méthodes permettant d’accéder aux données dans le pool de stockage sont les suivantes :

- Travaux Spark.
- L’utilisation de tables externes SQL Server pour permettre l’interrogation des données à l’aide des nœuds de calcul PolyBase et des instances SQL Server en cours d’exécution dans les nœuds HDFS.

Vous pouvez également interagir avec HDFS à l’aide des éléments suivants :

- Azure Data Studio.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].
- kubectl pour émettre des commandes vers le conteneur Hadoop.
- Passerelle HTTP HDFS.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

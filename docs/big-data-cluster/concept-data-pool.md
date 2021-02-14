---
title: Présentation des pools de données
titleSuffix: SQL Server big data clusters
description: Découvrez le rôle des pools de données SQL Server dans un cluster Big Data SQL Server, ainsi que l’architecture et les fonctionnalités d’un pool de données SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69ef4f739b35c55a31cb19fd8cb317d7750f8639
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047445"
---
# <a name="what-are-data-pools-in-a-sql-server-big-data-cluster"></a>Présentation des pools de données dans un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit le rôle des *pools de données SQL Server* dans un cluster Big Data SQL Server. Les sections suivantes décrivent l’architecture, les fonctionnalités et les scénarios d’utilisation d’un pool de données.

Cette vidéo de 5 minutes présente des pools de données et vous montre comment interroger des données à partir de pools de données :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Querying-Data-from-Big-Data-Cluster-Data-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="data-pool-architecture"></a>Architecture du pool de données

Un pool de données se compose d’une ou plusieurs instances de pool de données SQL Server qui fournissent un stockage SQL Server persistant pour le cluster. Il permet d’interroger les performances des données mises en cache par rapport à des sources de données externes et de décharger le travail. Les données sont ingérées dans le pool de données à l’aide de requêtes T-SQL ou de travaux Spark. Afin d’améliorer les performances dans les jeux de données volumineux, les données ingérées sont distribuées dans partitions et stockées dans toutes les instances SQL Server du pool. Les méthodes de distributions prises en charge sont tourniquet (Round Robin) et répliquées. Pour l’optimisation de l’accès en lecture, un index columstore en cluster est créé sur chaque table de chaque instance du pool de données. Un pool de données constitue le DataMart avec scale-out des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

![DataMart avec scale-out](media/concept-data-pool/data-virtualization-improvements.png)

L’accès aux instances SQL Server du pool de données est géré à partir de l’instance maître SQL Server. Une source de données externe pour le pool de données est créée, avec les tables externes PolyBase pour stocker le cache de données. En arrière-plan, le contrôleur crée une base de données dans le pool de données avec des tables qui correspondent aux tables externes. À partir de l’instance maître SQL Server, le workflow est transparent : le contrôleur redirige les requêtes de table externe vers les instances SQL Server du pool de données (peut-être via le pool de calcul), exécute des requêtes et retourne le jeu de résultats. Les données du pool de données peuvent uniquement être ingérées ou interrogées et ne peuvent pas être modifiées. Toutes les actualisations de données nécessitent donc une suppression de la table, suivie de la recréation de la table et du remplissage ultérieur des données.

## <a name="data-pool-scenarios"></a>Scénarios de pools de données

 La création de rapports est un scénario de pool de données commun. Par exemple, une requête complexe joignant plusieurs sources de données PolyBase, utilisées pour un rapport hebdomadaire, peut être déchargée dans le pool de données. Les données en cache fournissent un calcul rapide local et éliminent la nécessité de revenir aux jeux de données d’origine. De même, les données du tableau de bord qui nécessitent une actualisation périodique peuvent être mises en cache dans le pool de données pour les rapports optimisés. L’exploration de répétition Machine Learning peut également tirer parti de la mise en cache des jeux de données dans le pool de données.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

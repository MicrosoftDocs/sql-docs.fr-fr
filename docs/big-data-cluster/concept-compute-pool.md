---
title: Présentation des pools de calcul
titleSuffix: SQL Server big data clusters
description: Cet article explique le rôle d’un pool de calcul dans un cluster Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/15/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3a8816943ca952b11fdaae959f706e49e084386d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047609"
---
# <a name="what-are-compute-pools-in-a-sql-server-big-data-cluster"></a>Présentation des pools de calcul dans un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit le rôle des *pools de calcul SQL Server* dans un cluster Big Data SQL Server. Les pools de calcul fournissent des ressources de calcul de scale-out pour un cluster Big Data SQL Server. Ils servent à déplacer le travail de calcul, ou les jeux de résultats intermédiaires, à partir de l’instance maître SQL Server. Les sections suivantes décrivent l’architecture, les fonctionnalités et les scénarios d’utilisation d’un pool de calcul.

Vous pouvez également regarder cette vidéo de 5 minutes pour une introduction aux pools de calcul :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Big-Data-Cluster-Compute-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="compute-pool-architecture"></a>Architecture d’un pool de calcul

Un pool de calcul est constitué d’un ou de plusieurs pods de calcul exécutés dans Kubernetes. La création et la gestion automatisées de ces pods sont coordonnées par l’[instance maître de SQL Server](concept-master-instance.md). Chaque pod contient un ensemble de services de base, ainsi qu’une instance du moteur de base de données SQL Server.

![Architecture d’un pool de calcul](media/concept-compute-pool/compute-pool-architecture.png)

## <a name="scale-out-groups"></a>Groupes de scale-out

Un pool de calcul peut servir de groupe de scale-out PolyBase pour les requêtes distribuées sur différentes sources de données externes telles que SQL Server, Oracle, MongoDB, Teradata et HDFS. Si vous utilisez des pods de calcul dans Kubernetes, les clusters Big Data SQL Server peuvent automatiser la création et la configuration de pods de calcul pour les groupes de scale-out PolyBase.

## <a name="compute-pool-scenarios"></a>Scénarios de pool de calcul

Le pool de calcul est utilisé dans les scénarios suivants :

- Lorsque les requêtes soumises à l’instance maître utilisent une ou plusieurs tables situées dans le [pool de stockage](concept-storage-pool.md).

- Lorsque les requêtes soumises à l’instance maître utilisent une ou plusieurs tables avec une distribution par tourniquet situées dans le [pool de données](concept-data-pool.md).

- Lorsque les requêtes soumises à l’instance maître utilisent des tables **partitionnées** avec des sources de données externes SQL Server, Oracle, MongoDB et Teradata. Pour ce scénario, l’indicateur de requête OPTION (FORCE SCALEOUTEXECUTION) doit être activé

- Lorsque les requêtes soumises à l’instance maître utilisent une ou plusieurs tables situées dans la [hiérarchisation HDFS](hdfs-tiering.md).

Le pool de calcul n’est **pas** utilisé dans les scénarios suivants :

- Lorsque les requêtes soumises à l’instance maître utilisent une ou plusieurs tables dans un cluster Hadoop HDFS externe

- Lorsque les requêtes soumises à l’instance maître utilisent une ou plusieurs tables dans Stockage Blob Azure

- Lorsque les requêtes soumises à l’instance maître utilisent des tables **non partitionnées** avec des sources de données externes SQL Server, Oracle, MongoDB et Teradata.

- Lorsque l’indicateur de requête OPTION (DISABLE SCALEOUTEXECUTION) est activé

- Lorsque les requêtes soumises à l’instance maître s’appliquent à des bases de données situées sur l’instance maître

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

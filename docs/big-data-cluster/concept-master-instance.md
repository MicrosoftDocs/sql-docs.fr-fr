---
title: Qu’est-ce que l’instance principale ?
titleSuffix: SQL Server big data clusters
description: Cet article décrit l’instance principale SQL Server dans un cluster Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd97d5b0da5d88974dcd56aa0f32cbc3243532f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052099"
---
# <a name="what-is-the-master-instance-in-a-sql-server-big-data-cluster"></a>Qu’est-ce que l’instance principale dans un cluster Big Data SQL Server ?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit le rôle de l’*instance maître SQL Server* dans un cluster Big Data SQL Server. L’instance maître est une instance SQL Server s’exécutant dans un cluster Big Data SQL Server pour gérer la connectivité, les requêtes avec scale-out, les métadonnées, les bases de données utilisateur et les services Machine Learning.

L’instance principale SQL Server fournit les fonctionnalités suivantes :

## <a name="connectivity"></a>Connectivité

L’instance principale SQL Server fournit un point de terminaison TDS accessible depuis l’extérieur pour le cluster. Vous pouvez connecter des applications ou des outils SQL Server comme Azure Data Studio ou SQL Server Management Studio à ce point de terminaison, comme vous le feriez pour n’importe quelle autre instance SQL Server.

## <a name="scale-out-query-management"></a>Scale-out de la gestion des requêtes

L’instance principale SQL Server contient le moteur de requête avec scale-out qui est utilisé pour distribuer les requêtes entre les instances SQL Server sur les nœuds du [pool de calcul](concept-compute-pool.md). Le moteur de requête avec scale-out fournit également un accès via Transact-SQL à toutes les tables Hive dans le cluster, sans aucune configuration supplémentaire.

## <a name="metadata-and-user-databases"></a>Base de données de métadonnées et bases de données utilisateur

En plus des bases de données système SQL Server standard, l’instance principale SQL contient également les éléments suivants :

- Une base de données de métadonnées qui contient les métadonnées des tables HDFS.
- Un mappage des partitions du plan de données.
- Des détails sur les tables externes qui fournissent l’accès au plan de données du cluster.
- Des sources de données externes PolyBase et des tables externes définies dans les bases de données utilisateur.

Vous pouvez également choisir d’ajouter vos propres bases de données utilisateur à l’instance principale SQL Server.

## <a name="machine-learning-services"></a>Services Machine Learning

Les services Machine Learning SQL Server sont une fonctionnalité complémentaire du moteur de base de données, utilisée pour l’exécution de code Java, R et Python dans SQL Server. Cette fonctionnalité est basée sur l’infrastructure d’extensibilité SQL Server, qui isole les processus externes des processus principaux du moteur, mais qui s’intègre totalement aux données relationnelles sous forme de procédures stockées, de script T-SQL contenant des instructions R ou Python, ou de code Java, R ou Python contenant du T-SQL.

Dans le cadre d’un cluster Big Data SQL Server, les services Machine Learning sont disponibles sur l’instance principale SQL Server par défaut. Cela signifie qu’une fois que l’exécution de scripts externes est activée sur l’instance principale SQL Server, il sera possible d’exécuter des scripts Java, R et Python avec sp_execute_external_script.

### <a name="advantages-of-machine-learning-services-in-a-big-data-cluster"></a>Avantages des services Machine Learning dans un cluster Big Data

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] permet de joindre facilement des données Big Data aux données dimensionnelles généralement stockées dans la base de données des entreprises. La valeur des données Big Data augmente considérablement quand elles ne sont pas seulement dans les mains de certaines parties d’une organisation, mais sont également incluses dans les rapports, les tableaux de bord et les applications. En même temps, les scientifiques des données peuvent continuer à utiliser les outils de l’écosystème Spark/HDFS, et disposer d’un accès facile et en temps réel aux données de l’instance principale SQL Server et dans des sources de données externes accessibles _via_ l’instance principale SQL Server.

Avec [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vous pouvez en faire plus avec vos lacs de données d’entreprise. Les développeurs et les analystes SQL Server peuvent :

* Créer des applications qui utilisent des données provenant de lacs de données d’entreprise.
* Travailler sur l’ensemble des données avec des requêtes Transact-SQL.
* Utiliser l’écosystème existant d’outils et d’applications SQL Server pour accéder aux données d’entreprise et les analyser.
* Réduire le besoin de déplacement de données via la virtualisation des données et les mini-Data Warehouses.
* Continuer à utiliser Spark pour les scénarios Big Data.
* Créer des applications d’entreprise intelligentes avec Spark ou SQL Server pour entraîner des modèles sur des lacs de données.
* Exploiter des modèles dans les bases de données de production pour obtenir de meilleures performances.
* Diffuser des données directement dans des mini-Data Warehouses de l’entreprise pour une analytique en temps réel.
* Explorer les données visuellement avec des outils d’analyse interactive et décisionnels.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

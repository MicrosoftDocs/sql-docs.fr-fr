---
title: Ressources d’administration pour les Clusters Big Data (BDC)
titleSuffix: SQL Server
description: Cet article explique comment afficher l’état d’un Cluster Big Data à l’aide d’Azure Data Studio, de notebooks et de commandes Azure Data CLI (azdata).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 10/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194f74fa175ad3f1c08ceab542b6db7812987157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045634"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Ressources d’administration pour les Clusters Big Data (BDC) 

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment exécuter une application depuis l’extérieur vers l’intérieur d’un cluster Big Data SQL Server.

## <a name="know-your-architecture"></a>Connaître votre architecture

À compter de SQL Server 2019 (15.x), les Clusters Big Data SQL Server vous permettent de déployer des clusters évolutifs de conteneurs SQL Server, Spark et HDFS exécutés sur Kubernetes. Les articles suivants obtiennent une vue d’ensemble du cluster Big Data :
- [Que sont les Clusters Big Data SQL Server](big-data-cluster-overview.md)

Les Clusters Big Data SQL Server assurent des fonctionnalités d’autorisation et d’authentification uniformes et cohérentes. Les articles suivants obtiennent une vue d’ensemble de la sécurité du cluster Big Data :
- [Concepts de sécurité pour les Clusters Big Data SQL Server](concept-security.md)

## <a name="manage-and-operate-with-tools"></a>Gérer et utiliser des outils

Les articles suivants expliquent comment gérer et utiliser le Cluster Big Data de l’une des manières suivantes : 

- [Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md)
- [Gérer les clusters Big Data pour le tableau de bord du contrôleur SQL Server](manage-with-controller-dashboard.md)
- [Gérer des Clusters Big Data SQL Server avec des notebooks Azure Data Studio](notebooks-manage-bdc.md)
- [Gérer des Clusters Big Data (BDC) avec des notebooks](cluster-manage-notebooks.md)
- [Exécuter un exemple de notebook avec Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Analyser avec des outils

Les articles suivants expliquent comment analyser le Cluster Big Data de l’une des manières suivantes : 

- [Analyser le cluster BDC avec Azure Data Studio](cluster-monitor-ads.md)
- [Analyser le cluster BDC avec l’utilitaire Azdata](cluster-monitor-cmdlet.md)
- [Analyser le cluster BDC avec le tableau de bord Grafana](cluster-monitor-grafana.md)
- [Analyser le cluster BDC avec des notebooks Jupyter et Azure Data Studio](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Analyser et inspecter les journaux avec des blocs-notes

Les articles suivants répertorient un grand nombre de notebooks Jupyter disponibles dans Azure Data Studio.

- [Analyse du cluster avec des notebooks](cluster-monitor-notebooks.md)
- [Collecte et analyse des journaux dans le cluster avec des notebooks](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Ressources de résolution des problèmes des Clusters Big Data

Les articles suivants expliquent comment résoudre les problèmes liés au Cluster Big Data :

- [Résoudre les problèmes de cluster BDC avec l’utilitaire kubectl](cluster-troubleshooting-commands.md) 
- [Résoudre les problèmes du notebook pyspark](troubleshoot-pyspark-notebook.md)
- [Résoudre des problèmes liés au cluster BDC avec des notebooks Jupyter et Azure Data Studio](cluster-troubleshooter-notebooks.md)
- [Restauration des autorisations HDFS](troubleshoot-hdfs-restore-admin.md)

Les articles suivants expliquent comment résoudre les problèmes liés au Cluster Big Data déployé en mode Active Directory :
- [Résoudre les problèmes de cluster BDC en mode Active Directory](troubleshoot-active-directory.md) 
- [Résoudre des problèmes d’échecs de connexion en mode AD](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Résoudre des problèmes d’arrêt de déploiement en mode AD BDC](troubleshoot-ad-reverse-lookup-zone.md)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ?](big-data-cluster-overview.md).
---
title: Analyser le cluster avec des notebooks Jupyter et Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Analyse du cluster avec des notebooks Jupyter et Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bc370b42f247c4b8e57c5628feaae7385ba1502d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039372"
---
# <a name="monitoring-cluster-with-notebooks"></a>Analyse du cluster avec des notebooks

Cette page est un index des notebooks pour les Clusters Big Data SQL Server. Ces notebooks exécutables (.ipynb) sont conçus pour SQL Server 2019 afin de faciliter l’analyse des Clusters Big Data.

Chaque notebook est conçu pour vérifier ses propres dépendances. Une commande **Exécuter toutes les cellules** s’effectue correctement ou produit une exception avec un conseil en lien hypertexte vers un autre notebook qui va résoudre la dépendance manquante. Suivez le lien hypertexte vers le notebook suivant, appuyez sur **Exécuter toutes les cellules** puis, après une exécution réussie, revenez au notebook d’origine, et appuyez à nouveau sur **Exécuter toutes les cellules**.

Une fois toutes les dépendances installées, mais après l’échec de la commande **Exécuter toutes les cellules**, chaque notebook analyse les résultats et, dans la mesure du possible, produit un conseil de lien hypertexte vers un autre notebook pour faciliter la résolution du problème.


## <a name="monitoring-kubernetes"></a>Analyse de Kubernetes

Cette section contient un ensemble de notebooks utiles pour obtenir les informations et l’état d’un cluster Big Data SQL Server à l’aide de l’interface de ligne de commande (CLI) `azdata`.

|Nom |Description |
|---|---|---|---|
|TSG006 - Obtenir l’état des pods système|Consulter l’état de tous les pods système |
|TSG007- Obtenir l’état du pod BDC|Consulter l’état des pods du cluster Big Data|
|TSG008 - Obtenir des informations de version (Kubernetes)|Obtenir des informations sur le cluster Kubernetes.|
|TSG009- Obtenir des nœuds (Kubernetes)|Obtenir les contextes Kubernetes. |
|TSG010 - Obtenir les contextes de configuration|Utilisation d’une requête DMV pour obtenir plus d’informations sur l’erreur interne du processeur de requêtes|
|TSG015 - Afficher les services BDC (Kubernetes)|Obtenez l’état des services du cluster BDC déployé dans le cluster Kubernetes. |
|TSG016 - Décrire les pods BDC|Obtenez l’état des pods du BDC déployé dans le cluster Kubernetes. |
|TSG020 - Décrire les nœuds (Kubernetes)|Obtenez les informations sur les nœuds pour que le cluster BDC utilise l’interface de ligne de commande Kubectl. |
|TSG021 - Obtenir les informations de cluster (Kubernetes)|Obtenir des informations sur le cluster Kubernetes. |
|TSG022 - Obtenir l’adresse IP externe de l’hôte kubeadm|Obtenez l’adresse IP externe de l’hôte de kubeadm. |
|TSG023 - Obtenir tous les objets BDC (Kubernetes)|Obtenez un résumé de toutes les ressources Kubernetes pour l’espace de noms système et l’espace de noms du cluster Big Data. |
|TSG042 - Obtenir le nom du nœud et les montages externes pour les PVC de données et de journaux|Obtenez le nom du nœud hébergeant le pod avec les montages externe de données et de journaux. |
|TSG063 - Obtenir les classes de stockage (Kubernetes)|Utilisez ce notebook pour obtenir des classes de stockage Kubernetes. |
|TSG064 - Obtenir des revendications de volume persistant BDC|Affichez les revendications de volume persistant (PVC) pour le cluster Big Data. |
|TSG065 - Obtenir des secrets BDC (Kubernetes)|Affichez les secrets du cluster Big Data. |
|TSG066 - Obtenir l’événement BDC (Kubernetes)|Affichez les événements du cluster Big Data.|
|TSG072 - Obtenir des volumes persistants (Kubernetes)|Afficher le volume persistant (PV) du cluster Kubernetes Les volumes persistants sont des objets qui ne sont pas des espaces de noms. |
|TSG081 - Obtenir des espaces de noms (Kubernetes)|Obtenez les espaces de noms Kubernetes. |
|TSG089 - Décrire les pods non exécutés BDC|Affichez les pods BDC non exécutés pour le cluster Kubernetes. |
|TSG097 - Obtenir des StatefulSets BDC (Kubernetes)|Affichez les statefulsets BDC pour le cluster Kubernetes. |
|TSG098 - Obtenir des replicasets BDC (Kubernetes)|Affichez les replicasets BDC pour le cluster Kubernetes. |
|TSG099 - Obtenir des daemonsets BDC (Kubernetes)|Affichez les daemonsets BDC pour le cluster Kubernetes. |


## <a name="monitor-big-data-cluster-bdc"></a>Analyser le Cluster Big Data (BDC)

Cette section contient un ensemble de notebooks utiles pour obtenir les informations et l’état du cluster Kubernetes hébergeant un cluster Big Data (BDC) SQL Server.

|Nom |Description |
|---|---|---|---|
|TSG003 - Afficher les sessions Spark du BDC|Affichez les sessions Spark BDC. |
|TSG004 - Afficher les applications du BDC|Affichez les applications en cours d’exécution dans le cluster BDC.|
|TSG012 - Afficher l’état du BDC|Obtenez l’état actuel des différents composants du cluster BDC.|
|TSG013 - Afficher la liste des fichiers dans le pool de stockage (HDFS)|Obtenez la liste des fichiers dans le pool de stockage (HDFS). |
|TSG014 - Afficher les points de terminaison du BDC|Obtenez tous les points de terminaison disponibles pour le cluster BDC.|
|TSG017 - Afficher la configuration du BDC|Obtenez la configuration du BDC. |
|TSG033 - Afficher l’état SQL de BDC|Obtenez l’état SQL Server du BDC déployé dans le cluster Kubernetes. |
|TSG049 - Afficher l’état du contrôleur BDC|Obtenez l’état du contrôleur BDC déployé dans le cluster Kubernetes. |
|TSG068 - Afficher l’état HDFS de BDC|Obtenez l’état HDFS du BDC déployé dans le cluster Kubernetes. |
|TSG069 - Afficher l’état de la passerelle du cluster Big Data|Obtenez l’état de la passerelle BDC du BDC déployé dans le cluster Kubernetes. |
|TSG070 - Interroger le pool maître SQL| Exécutez une requête SQL sur l’instance maître. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).

---
title: Collecte et analyse des journaux avec des notebooks Jupyter et Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Enregistrement du cluster avec des notebooks Jupyter et Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8609fbe12e6218d328528dd5577bf1a40167162
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047779"
---
# <a name="gathering-and-analyzing-logs-in-the-cluster-with-notebooks"></a>Collecte et analyse des journaux dans le cluster avec des notebooks

Cette page est un index des notebooks pour les Clusters Big Data SQL Server. Ces notebooks exécutables (.ipynb) sont conçus pour SQL Server 2019 afin de faciliter l’enregistrement des Clusters Big Data.

Chaque notebook est conçu pour vérifier ses propres dépendances. Une commande **Exécuter toutes les cellules** s’effectue correctement ou produit une exception avec un conseil en lien hypertexte vers un autre notebook qui va résoudre la dépendance manquante. Suivez le lien hypertexte vers le notebook suivant, appuyez sur **Exécuter toutes les cellules** puis, après une exécution réussie, revenez au notebook d’origine, et appuyez à nouveau sur **Exécuter toutes les cellules**.

Une fois toutes les dépendances installées, mais après l’échec de la commande **Exécuter toutes les cellules**, chaque notebook analyse les résultats et, dans la mesure du possible, produit un conseil de lien hypertexte vers un autre notebook pour faciliter la résolution du problème.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Collecte des journaux à partir du Cluster Big Data (BDC)

Cette section contient un ensemble de notebooks utiles pour obtenir des journaux à partir d’un Cluster Big Data (BDC) SQL Server.

| Nom | Description |
|--|--|
| TSG001 - Exécuter azdata copy-logs | Utilisez l’interface de ligne de commande azdata pour copier des données dans des Clusters Big Data. |
| TSG061 - Obtenir la fin de tous les journaux de conteneurs pour les pods dans l’espace de noms BDC | Obtenez tous les journaux de conteneur des Pod du cluster BDC dans l’espace de noms. |
| TSG062 - Obtenir la fin de tous les journaux de conteneurs précédents pour les pods dans l’espace de noms du BDC | Obtenez tous les journaux de conteneur des pods précédents du cluster BDC dans l’espace de noms. |
| TSG083 - Exécuter kubectl cluster-info dump | Utilisez l’interface de ligne de commande kubetl pour sauvegarder les informations relatives aux clusters BDC. |
| TSG084 - Erreur interne du processeur de requêtes | Utilisation d’une requête DMV pour obtenir plus d’informations sur l’erreur interne du processeur de requêtes. |
| TSG091 - Obtenir les journaux CLI azdata | Obtenez les journaux azdata à partir de la machine locale. |



## <a name="analyse-logs-from-big-data-clusters-bdc"></a>Analyser les journaux à partir de Clusters Big Data (BDC)

Ensemble de notebooks permettant de collecter et d’analyser des journaux à partir d’un cluster Big Data SQL Server.  Le processus d’analyse suggère l’exécution des notebooks suivants pour rechercher les problèmes connus dans les journaux.

|Nom|Description |
|---|---|
|TSG030 - Fichiers des journaux des erreurs SQL Server|Obtenez des fichiers ErrorLog SQL Server et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents. |
|TSG031 - Journaux Polybase SQL Server|Obtenez des fichiers PolyBase SQL Server et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG034 - Journaux Livy|Obtenez des journaux Livy et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG035 - Journaux d’historique Spark|Obtenez des journaux d’historique Spark et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG036 - Journaux de contrôleur|Obtenez des journaux du contrôleur de dernière minute et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG046 - Journaux de la passerelle Knox|Knox envoie une erreur 500 au client et supprime les détails (pile) pointant vers la cause du problème sous-jacent. Utilisez donc ce notebook pour obtenir les journaux Knox du cluster. Obtenez des journaux de passerelle Knox et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG073 - Journaux InfluxDB|Obtenez des journaux InfluxDB et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG076 - Journaux Elasticsearch|Obtenez des journaux de recherche Elasticsearch et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG077 - Journaux Kibana|Obtenez des journaux Kibana et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG088 - Journaux datanode Hadoop|Obtenez des journaux Hadoop datanode et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG090 - Journaux nodemanager Yarn|Obtenez des journaux Yarn nodemanager et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG092 - Fin du journal Supervisord pour tous les conteneurs dans BDC|Obtenez le fichier journal Supervisord pour tous les conteneurs dans le BDC et analysez les entrées de journal et Suggérez d’autres guides de dépannage pertinents.|
|TSG093 - Fin du journal de l’agent pour tous les conteneurs dans BDC|Obtenez le fichier journal Supervisord pour tous les conteneurs dans le BDC et analysez les entrées de journal pour suggérer d’autres guides de résolution des problèmes pertinents.|
|TSG094 - Journaux Grafana|Obtenez des journaux Grafana et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG095 - Journaux namenode Hadoop|Obtenez des journaux Hadoop namenode et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|
|TSG096 - Journaux Zookeeper|Obtenez des journaux Zookeeper et analysez les entrées de journal pour suggérer des guides de résolution des problèmes pertinents.|

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).

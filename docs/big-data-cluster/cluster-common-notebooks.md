---
title: Scénarios communs fonctionnant avec des Clusters Big Data (BDC) avec des notebooks Jupyter et Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Scénarios communs fonctionnant avec BDC avec des notebooks Jupyter et Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cbf034873b690262ff6acb395ed8eca5d09ae79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039509"
---
# <a name="common-notebooks-for-sql-server-big-data-clusters"></a>Notebooks communs pour les Clusters Big Data SQL Server

Cet article répertorie des notebooks pour des Clusters Big Data SQL Server. Les notebooks exécutables (.ipynb) sont conçus pour SQL Server 2019 afin de faciliter l’affichage de scénarios communs sur des clusters Big Data.

Chaque notebook est conçu pour vérifier ses propres dépendances. Une option **Exécuter toutes les cellules** s’effectue correctement ou produit une exception avec un *conseil* en lien hypertexte vers un autre notebook qui va résoudre la dépendance manquante. Suivez le lien hypertexte vers le notebook suivant, appuyez sur **Exécuter toutes les cellules** puis, après une exécution réussie, revenez au notebook d’origine, et appuyez à nouveau sur **Exécuter toutes les cellules**.

Une fois toutes les dépendances installées, mais après l’échec de la commande **Exécuter toutes les cellules**, chaque notebook analyse les résultats et, dans la mesure du possible, produit un conseil de lien hypertexte vers un autre notebook pour faciliter la résolution du problème.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Collecte des journaux à partir du Cluster Big Data (BDC)

Les notebooks de cette section sont utilisés comme prérequis pour d’autres notebooks, notamment pour la connexion à un cluster et la déconnexion d’un cluster.

|Nom |Description |
|---|---|
|SOP005 - az login|Utiliser l’interface de ligne de commande az pour se connecter à Azure |
|SOP006 - az logout|Utiliser l’interface de ligne de commande az pour se déconnecter d’Azure|
|SOP007 - Informations de version (azdata, bdc, kubernetes)|Définissez les fonctions d’assistance utilisées dans le notebook sur les informations de contrôle de version.|
|SOP011 - Définir le contexte de configuration Kubernetes|Définissez la configuration Kubernetes à utiliser. |
|SOP028 - azdata login|Utiliser l’interface de ligne de commande azdata pour se connecter à un cluster Big Data |
|SOP033 - azdata logout|Utiliser l’interface de ligne de commande azdata pour se déconnecter d’un cluster Big Data |
|SOP034 - Attendre que le BDC soit sain|Se bloque jusqu’à ce que le cluster Big Data soit sain ou que le délai d’attente spécifié expire. Le paramètre min_pod_count indique que le contrôle d’intégrité ne sera pas transmis tant qu’il n’y a pas ce nombre de pods dans le cluster. Si qu’un nombre de pods existants au-delà de cette limite est défectueux, le cluster n’est pas sain.|
|OPR001 - Créer un déploiement d’applications|Utilisez ce notebook pour créer une application dans un Cluster Big Data. |
|OPR002 - Exécuter le déploiement d’applications|Utilisez ce notebook pour exécuter une application dans un Cluster Big Data. |
|OPR003 - Créer un cronjob|Utilisez ce notebook pour créer un cronjob dans un Cluster Big Data. |
|OPR004 - Suspendre un cronjob|Utilisez ce notebook pour suspendre un cronjob dans un Cluster Big Data. |
|OPR005 - Reprendre un cronjob|Utilisez ce notebook pour reprendre un cronjob dans un Cluster Big Data. |
|OPR006 - Supprimer un cronjob|Utilisez ce notebook pour supprimer un cronjob dans un Cluster Big Data. |
|OPR007 - Supprimer un déploiement d’applications|Utilisez ce notebook pour supprimer une application dans un Cluster Big Data. |
|OPR100 - Déployer et planifier le ou les notebooks|Utilisez ce notebook pour déployer une liste de notebooks sur un pod de déploiement d’applications, exécuter le déploiement d’applications selon une planification à l’aide d’un CronJob Kubernetes et installer un tableau de bord Grafana pour afficher les résultats.|
|OPR600 - Analyser l’infrastructure (Kubernetes)|Utilisez ce notebook pour analyser l’infrastructure.|
|OPR700 - Créer un tableau de bord Grafana pour les applications de déploiement d’applications|Utilisez ce notebook pour créer un tableau de bord dans Grafana afin d’analyser les résultats du déploiement d’applications et générer des alertes en cas d’échec du démarrage d’une application ou d’un contrôle de validité.|
|OPR900 - Résoudre les problèmes d’exécution du déploiement d’applications|Utilisez ce notebook pour exécuter le script de déploiement d’applications directement dans le conteneur (à l’aide de kubectl exec). Cela peut fournir davantage d’informations de débogage pour la résolution des problèmes, en particulier pour les problèmes liés à l’index de démarrage du script.|
|OPR901 - Résoudre les problèmes cronjob|Utilisez ce notebook pour exécuter le script cronjob directement dans le conteneur (à l’aide de kubectl exec). Cela peut fournir davantage d’informations de débogage pour résoudre les problèmes.|


## <a name="analyze-logs-from-big-data-clusters-bdc"></a>Analyser des journaux à partir de Clusters Big Data (BDC)

Ensemble d’exemples de notebooks illustrant des scénarios de cluster Big Data SQL Server.

|Nom |Description |
|---|---|
|SAM001a - Interroger un pool de stockage à partir du pool principal Server (1 sur 3) - Charger les exemples de données|Dans ce didacticiel en 3 parties, chargez des données dans le pool de stockage (HDFS) à l’aide de azdata, convertissez-les dans Parquet (à l’aide de Spark) et dans la troisième partie, interrogez les données à l’aide du pool principal (SQL Server). |
|SAM001b - Interroger un pool de stockage à partir du pool principal SQL Server (2 sur 3) - Convertir des données en Parquet|Dans cette deuxième partie d’un didacticiel en 3 parties, utilisez Spark pour convertir un fichier .csv en fichier Parquet.|
|SAM001c - Interroger un pool de stockage à partir du pool principal SQL Server (3 sur 3) - Interroger HDFS à partir de SQL Server|Dans cette troisième partie du didacticiel sur le pool de stockage, vous allez apprendre à créer une table externe pointant vers des données HDFS dans un cluster Big Data et à joindre ces données à des données de valeur élevée dans l’instance maître.|
|SAM002 - Pool de stockage (2 sur 2) - Interroger HDFS|Dans cette deuxième partie du didacticiel sur le pool de stockage, vous allez apprendre à créer une table externe pointant vers des données HDFS dans un cluster Big Data et à joindre ces données à des données de valeur élevée dans l’instance maître|
|SAM003 - Exemple de pool de données|Dans ce didacticiel, vous allez apprendre à créer une source de pool de données et une table externe dans le pool de données, puis à insérer des données dans des tables de pools de données et à charger des données d’une table de pools de données vers une autre. Joindre des données dans la table du pool de données et d’autres tables du pool de données, également troncation de tables et nettoyage. |
|SAM004 - Virtualiser des données provenant de MongoDB|Pour interroger les données d’une source de données externe MongoDB, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.|
|SAM005 - Virtualiser des données provenant d’Oracle|Pour interroger les données d’une source de données externe Oracle, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.|
|SAM006 - Virtualiser des données provenant de SQL Server|Pour interroger virtuellement les données d’une autre source de données SQL Server, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.|
|SAM007 - Virtualiser des données provenant de Teradata|Pour interroger les données d’une source de données externe Teradata, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.|
|SAM008 - Spark avec azdata|Commandes azdata et kubectl pour fonctionner avec les sessions Spark.|
|SAM009 - HDFS avec azdata|Commandes azdata et kubectl pour fonctionner avec HDFS.|
|SAM010 - Application avec azdata|Commandes azdata et kubectl pour fonctionner avec le déploiement d’applications. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).

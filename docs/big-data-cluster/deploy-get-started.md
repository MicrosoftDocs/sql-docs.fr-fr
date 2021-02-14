---
title: Bien démarrer
titleSuffix: SQL Server Big Data Clusters
description: Découvrez les étapes et les ressources nécessaires au déploiement de clusters Big Data SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1a6a064ad4a6077bffcd1642569d63ee0e086a7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038820"
---
# <a name="get-started-with-big-data-clusters-2019-deployment"></a>Bien démarrer avec le déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article présente une vue d’ensemble de la façon de déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]. L’article présente les concepts et fournit un cadre pour vous aider à comprendre les scénarios de déploiement. Les étapes de déploiement varient selon la plateforme choisie pour le client et le serveur. Pour une présentation des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)

Pour d’autres scénarios de déploiement de SQL Server, consultez :

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Conteneurs Docker](../linux/sql-server-linux-docker-container-deployment.md)

## <a name="quick-introduction"></a>Présentation rapide 

Regardez cette vidéo de 9 minutes pour obtenir une vue d’ensemble de la façon de déployer des clusters Big Data :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Clusters-deployment-overview/player?WT.mc_id=dataexposed-c9-niner]


> [!TIP]
> Pour obtenir rapidement un environnement avec Kubernetes et déployer un cluster Big Data afin de vous aider à bénéficier de ses capacités, utilisez l’un des exemples de scripts désignés dans [la section des scripts](#scripts). Après le déploiement, pour gérer le cluster, utilisez les [outils clients](#tools) dans la section suivante.


## <a name="client-tools"></a><a id="tools"></a> Outils clients

Les clusters Big Data nécessitent un ensemble spécifique d’outils clients. Avant de déployer un cluster Big Data sur Kubernetes, vous devez installer les outils nécessaires à votre déploiement. Des outils spécifiques sont requis pour divers scénarios. Chaque article doit expliquer les outils prérequis permettant d’effectuer différentes tâches. Pour obtenir la liste complète des outils et des liens d’installation, consultez [Installer les outils de Big Data SQL Server 2019](deploy-big-data-tools.md).

## <a name="kubernetes"></a>Kubernetes

Les clusters Big Data sont déployés sous la forme d’une série de conteneurs interdépendants qui sont gérés dans [Kubernetes](https://kubernetes.io/docs/home). Vous pouvez héberger Kubernetes de différentes façons. Même si vous disposez déjà d’un environnement Kubernetes, vous devez passer en revue les conditions requises relatives aux clusters Big Data.

- **Azure Kubernetes Service (AKS)**  : AKS vous permet de déployer un cluster Kubernetes géré dans Azure. Vous gérez uniquement les nœuds de l’agent. Avec AKS, vous n’avez pas besoin de provisionner votre matériel pour le cluster. Il est aussi facile d’utiliser un [script Python](quickstart-big-data-cluster-deploy.md) ou un [notebook de déploiement](notebooks-deploy.md) pour créer le cluster AKS et déployer le cluster Big Data en une seule étape. Pour plus d’informations sur la configuration d’AKS en vue d’un déploiement de cluster Big Data, consultez [Configurer Azure Kubernetes Service pour le déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](deploy-on-aks.md).

- **Azure Red Hat OpenShift (ARO)**  : ARO vous permet de déployer un cluster Red Hat OpenShift géré dans Azure. Vous gérez uniquement les nœuds de l’agent. Avec ARO, vous n’avez pas besoin de provisionner votre matériel pour le cluster. Il est aussi facile d’utiliser un [script Python](quickstart-big-data-cluster-deploy-aro.md) pour créer le cluster ARO et déployer le cluster Big Data en une seule étape. Ce modèle de déploiement est introduit dans SQL Server 2019 CU5. 

- **Plusieurs machines** : Vous pouvez également déployer Kubernetes sur plusieurs machines Linux, qui peuvent être des serveurs physiques ou des machines virtuelles. L’outil [kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) peut être utilisé pour créer le cluster Kubernetes. Vous pouvez utiliser un [script bash](deployment-script-single-node-kubeadm.md) pour automatiser ce type de déploiement. Cette méthode fonctionne bien si vous disposez déjà d’une infrastructure existante que vous souhaitez utiliser pour votre cluster Big Data. Pour plus d’informations sur l’utilisation de déploiements **kubeadm** avec des clusters Big Data, consultez [Configurer Kubernetes sur plusieurs machines pour le déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](deploy-with-kubeadm.md).

- **Red Hat OpenShift** : Déploiement sur votre propre cluster Red Hat OpenShift. Pour plus d’informations, voir [Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur OpenShift en local et sur Azure Red Hat OpenShift](deploy-openshift.md). Ce modèle de déploiement est introduit dans SQL Server 2019 CU5.

## <a name="deploy-a-big-data-cluster"></a>Déployer un cluster Big Data

Après avoir configuré Kubernetes, vous devez déployer un cluster Big Data à l’aide de la commande `azdata bdc create`. Lors du déploiement, plusieurs méthodes sont possibles.

- Si vous effectuez un déploiement sur un environnement Dev/Test, vous pouvez choisir d’utiliser l’une des [configurations par défaut](deployment-guidance.md#deploy) fournies par **azdata**.

- Pour personnaliser votre déploiement, vous pouvez créer et utiliser vos propres [fichiers de configuration de déploiement](deployment-guidance.md#configfile).

- Pour une installation totalement sans assistance, vous pouvez passer tous les autres paramètres dans des variables d’environnement. Pour plus d’informations, consultez [Déploiements sans assistance](deployment-guidance.md#unattended).


## <a name="deployment-scripts"></a><a id="scripts"></a> Scripts de déploiement

Les scripts de déploiement vous permettent de déployer les clusters Kubernetes et Big Data en une seule étape. Ils fournissent également souvent des valeurs par défaut pour les paramètres des clusters Big Data. Vous pouvez personnaliser n’importe quel script de déploiement de cluster Big Data.

Les scripts de déploiement actuellement disponibles sont les suivants :

- [Script Python -- Déployer un cluster Big Data SQL Server sur AKS (Azure Kubernetes Service)](quickstart-big-data-cluster-deploy.md)
- [Script bash -- Déployer un cluster Big Data sur un cluster kubeadm à nœud unique](deployment-script-single-node-kubeadm.md)

## <a name="deployment-notebooks"></a>Notebooks de déploiement

Vous pouvez également déployer un cluster Big Data en exécutant un notebook Azure Data Studio. Pour plus d’informations sur l’utilisation d’un notebook pour un déploiement sur AKS, consultez l’article suivant :

- [Déployer un cluster Big Data avec des notebooks Azure Data Studio](notebooks-deploy.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez réussi à déployer un cluster Big Data, [connectez-vous au cluster](connect-to-big-data-cluster.md) en vue de [charger des exemples de données](tutorial-load-sample-data.md) à utiliser dans le cadre de différentes procédures pas à pas.
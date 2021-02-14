---
title: Configurer Azure Kubernetes Service
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment configurer Azure Kubernetes Service (AKS) pour les déploiements de clusters Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2021cf90230bd22de775cef164336f9e31631991
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038789"
---
# <a name="configure-azure-kubernetes-service-for-sql-server-big-data-cluster-deployments"></a>Configurer Azure Kubernetes Service pour les déploiements de cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment configurer Azure Kubernetes Service (AKS) pour les déploiements [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

AKS simplifie la création, la configuration et la gestion d’un cluster de machines virtuelles préconfigurées avec un cluster Kubernetes pour exécuter des applications conteneurisées. Ceci vous permet d’utiliser vos compétences existantes ou de tirer parti d’une grande expertise croissante de la communauté, pour déployer et gérer des applications basées sur des conteneurs sur Microsoft Azure.

Cet article décrit les étapes de déploiement de Kubernetes sur AKS avec Azure CLI. Si vous n’avez pas d’abonnement Azure, créez un compte gratuit avant de commencer.

> [!TIP]
> Vous pouvez également créer un script pour déployer AKS et un cluster Big Data en une seule étape. Pour plus d’informations, consultez la procédure à suivre dans un [script Python](quickstart-big-data-cluster-deploy.md) ou dans un [notebook](notebooks-deploy.md) Azure Data Studio.

## <a name="prerequisites"></a>Prérequis

- [Déployer les outils Big Data SQL Server 2019](deploy-big-data-tools.md) :
   - **Kubectl**
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
   - **Azure CLI**

- Version 1.13 au minimum pour le serveur Kubernetes. Pour AKS, vous devez utiliser le paramètre `--kubernetes-version` pour spécifier une version différente de la version par défaut.

- Pour garantir un déploiement réussi et une expérience optimale lors de la validation des scénarios de base sur AKS, vous pouvez utiliser un nœud unique ou un cluster AKS à plusieurs nœuds, avec ces ressources disponibles :
   - 8 processeurs virtuels pour tous les nœuds
   - 64 Go de mémoire par machine virtuelle
   - 24 disques attachés ou plus pour tous les nœuds

   > [!TIP]
   > L’infrastructure Azure offre plusieurs options de taille pour les machines virtuelles : découvrez [ici](/azure/virtual-machines/windows/sizes) les choix possibles dans la région où vous prévoyez de déployer.

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Un groupe de ressources Azure est un groupe logique dans lequel des ressources Azure sont déployées et gérées. Les étapes suivantes permettent de se connecter à Azure et de créer un groupe de ressources pour le cluster AKS.

1. À l’invite de commandes, exécutez la commande suivante et suivez les invites pour vous connecter à votre abonnement Azure :

    ```azurecli
    az login
    ```

1. Si vous avez plusieurs abonnements, vous pouvez voir tous vos abonnements en exécutant la commande suivante :

   ```azurecli
   az account list
   ```

1. Si vous voulez passer à un autre abonnement, vous pouvez exécuter cette commande :

   ```azurecli
   az account set --subscription <subscription id>
   ```

1. Identifiez la région Azure dans laquelle vous souhaitez déployer le cluster et les ressources à l’aide de cette commande :

   ```azurecli
   az account list-locations -o table
   ```

1. Créez un groupe de ressources avec la commande **az group create**. L’exemple suivant crée un groupe de ressources nommé `sqlbdcgroup` à l’emplacement `westus2`.

   ```azurecli
   az group create --name sqlbdcgroup --location westus2
   ```

## <a name="verify-available-kubernetes-versions"></a>Vérifier les versions de Kubernetes disponibles

Utilisez la dernière version disponible de Kubernetes. La dernière version disponible dépend de l’emplacement où vous déployez le cluster. La commande suivante retourne les versions de Kubernetes disponibles à un emplacement spécifique.

Avant d’exécuter la commande, mettez à jour le script. Remplacez `<Azure data center>` par l’emplacement de votre cluster.

   **bash**

   ```bash
   az aks get-versions \
   --location <Azure data center> \
   --query orchestrators \
   --o table
   ```

   **PowerShell**

   ```powershell
   az aks get-versions `
   --location <Azure data center> `
   --query orchestrators `
   --o table
   ```

Choisissez la dernière version disponible pour votre cluster. Prenez note du numéro de version. Vous l’utiliserez à l’étape suivante.

## <a name="create-a-kubernetes-cluster"></a>Créer un cluster Kubernetes

1. Créez un cluster Kubernetes dans AKS avec la commande [az aks create](/cli/azure/aks). L’exemple suivant crée un cluster Kubernetes nommé *kubcluster* avec un nœud d’agent Linux de taille **Standard_L8s**.

   Avant d’exécuter le script, remplacez `<version number>` par le numéro de version que vous avez identifié à l’étape précédente.

   Veillez à créer le cluster AKS dans le même groupe de ressources que celui utilisé dans les sections précédentes.

   **bash :**

   ```bash
   az aks create --name kubcluster \
   --resource-group sqlbdcgroup \
   --generate-ssh-keys \
   --node-vm-size Standard_L8s \
   --node-count 1 \
   --kubernetes-version <version number>
   ```

   **PowerShell :**

   ```powershell
   az aks create --name kubcluster `
   --resource-group sqlbdcgroup `
   --generate-ssh-keys `
   --node-vm-size Standard_L8s `
   --node-count 1 `
   --kubernetes-version <version number>
   ```

   Vous pouvez augmenter ou diminuer le nombre de nœuds d’agent Kubernetes en changeant `--node-count <n>`, où `<n>` est le nombre de nœuds d’agent que vous souhaitez utiliser. Ceci n’inclut pas le nœud Kubernetes principal, qui est géré en arrière-plan par AKS. L’exemple précédent utilise seulement un nœud à des fins d’évaluation. Vous pouvez également changer `--node-vm-size` pour choisir une taille de machine virtuelle adaptée aux exigences de votre charge de travail. Utilisez la commande `az vm list-sizes --location westus2 -o table` pour lister les tailles de machine virtuelle disponibles dans votre région.

   Au bout de quelques minutes, la commande se termine et retourne des informations au format JSON sur le cluster.

   > [!TIP]
   > Si vous recevez des erreurs lors de la création du cluster dans AKS, consultez la [section de résolution des problèmes](#troubleshoot) de cet article.

1. Enregistrez la sortie JSON de la commande précédente pour une utilisation ultérieure.

## <a name="connect-to-the-cluster"></a>Se connecter au cluster

1. Pour configurer kubectl afin qu’il se connecte à votre cluster Kubernetes, exécutez la commande [az aks get-credentials](/cli/azure/aks#az-aks-get-credentials). Cette étape permet de télécharger les informations d’identification et de configurer l’interface CLI de kubectl pour leur utilisation.

   ```azurecli
   az aks get-credentials --resource-group=sqlbdcgroup --name kubcluster
   ```

1. Pour vérifier la connexion à votre cluster, utilisez la commande [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) pour retourner une liste des nœuds du cluster.  L’exemple ci-dessous montre la sortie si vous avez 1 nœud principal et 3 nœuds d’agent.

   ```bash
   kubectl get nodes
   ```

## <a name="troubleshooting"></a><a id="troubleshoot"></a> Dépannage

Si vous rencontrez des problèmes lors de la création d’un service Azure Kubernetes avec les commandes précédentes, essayez les solutions suivantes :

- Vérifiez que vous avez installé la [dernière version d’Azure CLI](/cli/azure/install-azure-cli).
- Essayez les mêmes étapes avec un autre groupe de ressources et un autre nom de cluster.
- Pour plus de détails, consultez la [documentation sur la résolution des problèmes dans AKS](/azure/aks/troubleshooting).

## <a name="next-steps"></a>Étapes suivantes

Les étapes décrites dans cet article ont permis de configurer un cluster Kubernetes dans AKS. L’étape suivante consiste à déployer un cluster Big Data SQL Server 2019 sur le cluster Kubernetes AKS. Pour plus d’informations sur le déploiement de clusters Big Data, consultez l’article suivant :

[Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md)
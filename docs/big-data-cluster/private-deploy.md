---
title: Déployer un contrôleur de domaine secondaire dans un cluster privé Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment déployer un cluster Big Data SQL Server avec un cluster privé Azure Kubernetes Service (AKS) avec la mise en réseau avancée (CNI).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f83c3d1e1a5bf0c9b74d058f144c4d07025c8c05
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620268"
---
# <a name="deploy-bdc-in-azure-kubernetes-service-aks-private-cluster"></a>Déployer un contrôleur de domaine secondaire dans un cluster privé Azure Kubernetes Service (AKS)

Cet article explique comment déployer des clusters Big Data SQL Server sur un cluster privé Azure Kubernetes Service (AKS). Cette configuration prend en charge l’utilisation restreinte d’adresses IP publiques dans un environnement réseau d’entreprise.

Un déploiement privé offre les avantages suivants :

* Utilisation restreinte des adresses IP publiques
* Le trafic réseau entre les serveurs d’applications et les pools de nœuds de cluster reste sur le réseau privé
* Configuration personnalisée pour le trafic de sortie obligatoire en fonction des exigences spécifiques

Cet article montre comment utiliser un cluster privé AKS pour restreindre l’utilisation de l’adresse IP publique lorsque les chaînes de sécurité respectives ont été appliquées.

## <a name="deploy-private-bdc-cluster-with-aks-private-cluster"></a>Déployer un cluster BDC privé avec un cluster privé AKS

Pour commencer, créez un [cluster AKS privé](/azure/aks/private-clusters) pour vous assurer que le trafic réseau entre le serveur d’API et les pools de nœuds reste sur le réseau privé uniquement. Le plan de contrôle ou le serveur d’API a des adresses IP internes dans un cluster privé AKS.

Cette section vous montre comment déployer un cluster BDC dans un cluster privé Azure Kubernetes Service (AKS) avec mise en réseau avancée (CNI).

## <a name="create-a-private-aks-cluster-with-advanced-networking"></a>Créer un cluster privé AKS avec mise en réseau avancée

```bash

export REGION_NAME=<your Azure region >
export RESOURCE_GROUP=< your resource group name >
export SUBNET_NAME=aks-subnet
export VNET_NAME=bdc-vnet
export AKS_NAME=< your aks private cluster name >
 
az group create -n $RESOURCE_GROUP -l $REGION_NAME
 
az network vnet create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $VNET_NAME \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name $SUBNET_NAME \
    --subnet-prefix 10.1.0.0/16
 

SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
 
echo $SUBNET_ID
## will be displayed something similar as the following: 
/subscriptions/xxxx-xxxx-xxx-xxxx-xxxxxxxx/resourceGroups/your-bdc-rg/providers/Microsoft.Network/virtualNetworks/your-aks-vnet/subnets/your-aks-subnet
```

### <a name="create-aks-private-cluster-with-advanced-networking-cni"></a>Créer un cluster privé AKS avec mise en réseau avancée (CNI)

Pour pouvoir passer à l’étape suivante, vous devez approvisionner un cluster AKS avec Standard Load Balancer et la fonctionnalité de cluster privé activée. Votre commande ressemble à ce qui suit : 

```bash
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Une fois le déploiement réussi, vous pouvez accéder au groupe de ressources `<MC_yourakscluster>` et vous constaterez que `kube-apiserver` est un point de terminaison privé. Par exemple, consultez la section suivante.

## <a name="connect-to-an-aks-cluster"></a>Se connecter à un cluster AKS

```azurecli
az aks get-credentials -n $AKS_NAME -g $RESOURCE_GROUP
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Générer un profil de déploiement de cluster Big Data (BDC)

Après vous être connecté à un cluster AKS, vous pouvez commencer à déployer le BDC et vous pouvez préparer la variable d’environnement et lancer un déploiement : 

```azurecli
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

Générer et configurer le profil de déploiement personnalisé du BDC :

```azurecli
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-private-sql-server-big-data-cluster-with-ha"></a>Déployer un cluster privé Big Data SQL Server avec une haute disponibilité

Si vous [déployez un cluster Big Data SQL Server (SQL-BDC) avec une haute disponibilité](deployment-high-availability.md), vous utiliserez le profil de déploiement `aks-dev-test-ha`. Une fois le déploiement réussi, vous pouvez utiliser la même commande `kubectl get svc` et un service `master-secondary-svc` supplémentaire est créé. Vous devez configurer `ServiceType` comme `NodePort`. Les autres étapes sont similaires à celles mentionnées dans la section précédente.

L'exemple suivant affecte la valeur `NodePort` à `ServiceType` :

```azurecli
azdata bdc config replace -c private-bdc-aks /bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Déployer un BDC dans un cluster privé AKS

```azurecli
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="check-deployment-status"></a>Vérifier l’état du déploiement

Le déploiement prend quelques minutes, et vous pouvez utiliser la commande suivante pour vérifier l’état du déploiement : 

```console
kubectl get pods -n mssql-cluster -w
```

## <a name="check-the-service-status"></a>Vérifier l’état du service

Utilisez la commande ci-dessous pour vérifier les services. Vérifiez leur intégrité sans adresses IP externes :

```console
kubectl get services -n mssql-cluster
```

Découvrez comment [gérer le BDC dans un cluster privé AKS](private-manage.md), puis l’étape suivante consiste à se [connecter au cluster BDC](connect-to-big-data-cluster.md).

Consultez les scripts d’automatisation pour ce scénario dans le [Référentiel d’exemples SQL Server sur GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Étapes suivantes

[Gérer un cluster privé](private-manage.md)

[Limiter le trafic sortant du cluster BDC privé](private-restrict-egress-traffic.md)

[Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md)

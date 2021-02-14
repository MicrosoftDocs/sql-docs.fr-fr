---
title: Effectuer un déploiement dans Azure Kubernetes Service (AKS) - Tutoriel
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment déployer des clusters Big Data SQL Server en mode AD sur Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 760fc7070f0673f61a28cd126be0c6542ac4ee93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039861"
---
# <a name="tutorial-deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Tutoriel : Déployer des clusters Big Data SQL Server en mode AD sur Azure Kubernetes Service (AKS)

Cet article explique comment déployer un cluster Big Data SQL Server en mode d’authentification Active Directory avec une architecture de référence. L’architecture de référence étend Active Directory Domain Services (AD DS) à Azure. Vous pouvez le déployer à partir du [Centre des architectures Azure](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain), à l’aide de [composants Azure](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks).

## <a name="prerequisites"></a>Prérequis

Avant de déployer un cluster Big Data SQL Server, vous devez :

* Accéder à une machine virtuelle Azure à des fins de gestion. Cette machine virtuelle doit pouvoir accéder au réseau virtuel Azure sur lequel vous allez déployer le cluster Big Data. Elle doit se trouver soit sur le même réseau virtuel, soit sur un [réseau virtuel appairé](/azure/virtual-network/virtual-network-manage-peering).
* [Installer les outils Big Data](deploy-big-data-tools.md) sur la machine virtuelle de gestion.
* Préparer le déploiement du cluster en [mode d’authentification Active Directory](active-directory-prerequisites.md) dans votre contrôleur AD local.

## <a name="create-aks-subnet"></a>Créer un sous-réseau AKS

1. Définir des variables d’environnement

   ```console
   export REGION_NAME=< your Azure Region >
   export RESOURCE_GROUP=<your resource group >
   export SUBNET_NAME=aks-subnet
   export VNet_NAME= adds-vnet
   export AKS_NAME= <your aks cluster name>
   ```

1. Créez le sous-réseau AKS.

   ```console
   SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNet_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
   ```

La capture d’écran suivante montre l’architecture des sous-réseaux dans le réseau virtuel.

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Cluster AKS avec Active Directory et cluster Big Data SQL Server":::

## <a name="create-an-aks-private-cluster"></a>Créer un cluster privé AKS

Vous pouvez utiliser la commande suivante pour déployer le cluster privé AKS. Si vous n’avez pas besoin d’un cluster privé, supprimez le paramètre `--enable-private-cluster` dans la commande. Pour plus d’informations sur les autres exigences, consultez [Comment déployer un cluster Azure Kubernetes (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster).

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.3.0.10 \
    --service-cidr 10.3.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Après l’avoir déployé, [connectez-vous au cluster AKS](/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Vérifier l’entrée DNS inversée pour le contrôleur de domaine

Avant de démarrer le déploiement du cluster Big Data en mode AD dans le cluster AKS, vérifiez que le contrôleur de domaine comprend un **enregistrement A** et un **enregistrement PTR** (entrée de DNS inversé) qui sont inscrits auprès du serveur DNS.

Pour vérifier ce paramètre, exécutez la commande `nslookup` ou exécutez le [script PowerShell](troubleshoot-ad-reverse-lookup-zone.md) suivant pour vérifier que l’entrée de DNS inversé (enregistrement PTR) est configurée.

## <a name="create-bdc-deployment-profile"></a>Créer un profil de déploiement pour le cluster Big Data

La commande suivante crée un profil de déploiement :

```console
azdata bdc config init --source kubeadm-prod  --target bdc-ad-aks
```

Les commandes suivantes sont utilisées pour définir les paramètres d’un profil de déploiement.

### <a name="controljson"></a>control.json

```console
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.logs.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].dnsName=controller.contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].dnsName=proxys.contoso.com"

# security settings 
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"192.168.0.4\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"ad1.contoso.com\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainDnsName=contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
```

### <a name="bdcjson"></a>bdc.json

```console
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=master.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=mastersec.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=gateway.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=approxy.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="initiate-deployment"></a>Lancer le déploiement

La commande suivante lance le déploiement d’un cluster Big Data :

```console
azdata bdc create --config-profile bdc-ad-aks --accept-eula yes
```

## <a name="next-steps"></a>Étapes suivantes

[Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md)

[Tutoriel : Chargement d’un exemple de données dans un cluster Big Data SQL Server](tutorial-load-sample-data.md)
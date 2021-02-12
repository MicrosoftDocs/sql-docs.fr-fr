---
title: Restreindre le trafic de sortie des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment restreindre le trafic de sortie des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 589031e40ba0f1e53afd5b4082d132358e9cda32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046019"
---
# <a name="restrict-egress-traffic-of-big-data-clusters-bdc-clusters-in-azure-kubernetes-service-aks-private-cluster"></a>Restreindre le trafic de sortie des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS)

Par défaut, AKS provisionne un équilibreur de charge de référence SKU standard à configurer et utiliser pour la sortie. Cependant, la configuration par défaut peut ne pas répondre aux exigences de tous les scénarios si les adresses IP publiques ne sont pas autorisées ou si des tronçons supplémentaires sont nécessaires pour la sortie. Définissez une table de routage définie par l’utilisateur si le cluster interdit les adresses IP publiques et se trouve derrière une appliance virtuelle réseau.

Par défaut, les clusters AKS disposent d’un accès Internet sortant illimité pour des raisons de gestion et de fonctionnement. Les nœuds Worker d’un cluster AKS doivent accéder à certains ports et noms de domaine complet (FQDN), par exemple :

* Pour les mises à jour de sécurité système du nœud Worker, le cluster doit extraire les images conteneurs système de base de Microsoft Container Registry (MCR)
* Les nœuds Worker AKS avec GPU activé doivent accéder à certains points de terminaison de Nvidia pour installer le pilote
* Dans le scénario où les clients utilisent AKS conjointement avec des services Azure, comme Azure Policy pour la conformité de l’entreprise ou Azure Monitor (avec Container Insights) 
* Espace de développement activé et autres scénarios de nature similaire

> [!NOTE]
> Actuellement, lorsque vous déployez un BDC dans un cluster privé Azure Kubernetes Service (AKS), il n’existe pas de dépendances entrantes en plus de ce que nous avons mentionné dans cet article. Vous pouvez consulter toutes les dépendances sortantes sur [Contrôler le trafic de sortie pour les nœuds de cluster dans Azure Kubernetes Service (AKS)](/azure/aks/limit-egress-traffic).

Cet article explique comment déployer un BDC dans un cluster privé AKS avec mise en réseau avancée et un itinéraire défini par l’utilisateur (UDR), ainsi qu’une intégration supplémentaire avec l’environnement de mise en réseau de niveau entreprise.

## <a name="use-azure-firewall-to-restrict-egress-traffic"></a>Limiter le trafic de sortie à l’aide du Pare-feu Azure

Le Pare-feu Azure fournit une balise FQDN Azure Kubernetes Service `(AzureKubernetesService)` pour simplifier cette configuration.

Consultez [Limitation du trafic de sortie à l’aide du Pare-feu Azure](/azure/aks/limit-egress-traffic#restrict-egress-traffic-using-azure-firewall) pour obtenir des informations complètes sur cette étiquette.

L’illustration suivante montre comment le trafic est limité sur un cluster privé AKS. 

:::image type="content" source="media/private-cluster-restrict-egress-traffic/aks-azure-firewall-egress.png" alt-text="Trafic de sortie du pare-feu de cluster privé AKS":::

Pour configurer une architecture de base avec le pare-feu Azure :

1. Créer le groupe de ressources et le réseau virtuel
2. Créer et configurer le pare-feu Azure
3. Créer une table d’itinéraires définie par l’utilisateur
4. Configurer des règles de pare-feu
5. Créer un principal du service (SP)
6. Créer un cluster privé AKS
7. Créer un profil de déploiement de BDC
8. Déployer un BDC

Les étapes suivantes fournissent des détails.

## <a name="create-the-resource-group-and-vnet"></a>Créer le groupe de ressources et le réseau virtuel

1. Définissez un ensemble de variables d’environnement pour la création de ressources.

   ```console
   export REGION_NAME=<region>
   export RESOURCE_GROUP=private-bdc-aksudr-rg
   export SUBNET_NAME=aks-subnet
   export VNET_NAME=bdc-vnet
   export AKS_NAME=bdcaksprivatecluster
   ```

1. Créer le groupe de ressources

   ```azurecli
   az group create -n $RESOURCE_GROUP -l $REGION_NAME
   ```

1. Créer le réseau virtuel

   ```azurecli
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
   ```

## <a name="create-and-set-up-azure-firewall"></a>Créer et configurer un pare-feu Azure

1. Définissez un ensemble de variables d’environnement pour la création de ressources.

   ```console
   export FWNAME=bdcaksazfw
   export FWPUBIP=$FWNAME-ip
   export FWIPCONFIG_NAME=$FWNAME-config

   az extension add --name azure-firewall
   ```

1. Créer un sous-réseau dédié pour le pare-feu

   > [!NOTE]
   > Vous ne pouvez pas modifier le nom du pare-feu après sa création

   ```azurecli
   az network vnet subnet create \
     --resource-group $RESOURCE_GROUP \
     --vnet-name $VNET_NAME \
     --name AzureFirewallSubnet \
     --address-prefix 10.3.0.0/24

    az network firewall create -g $RESOURCE_GROUP -n $FWNAME -l $REGION_NAME --enable-dns-proxy true

    az network public-ip create -g $RESOURCE_GROUP -n $FWPUBIP -l $REGION_NAME --sku "Standard"

    az network firewall ip-config create -g $RESOURCE_GROUP -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBIP --vnet-name $VNET_NAME
   ```

Par défaut, Azure achemine automatiquement le trafic entre les sous-réseaux, les réseaux virtuels et les réseaux locaux Azure. 

## <a name="create-user-defined-route-table"></a>Créer une table d’itinéraires définie par l’utilisateur

Créez une table d’itinéraire défini par l’utilisateur (UDR) avec un tronçon vers le pare-feu Azure.

```azurecli

export SUBID= <your Azure subscription ID>
export FWROUTE_TABLE_NAME=bdcaks-rt
export FWROUTE_NAME=bdcaksroute
export FWROUTE_NAME_INTERNET=bdcaksrouteinet

export FWPUBLIC_IP=$(az network public-ip show -g $RESOURCE_GROUP -n $FWPUBIP --query "ipAddress" -o tsv)
export FWPRIVATE_IP=$(az network firewall show -g $RESOURCE_GROUP -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)

# Create UDR and add a route for Azure Firewall

az network route-table create -g $RESOURCE_GROUP --name $FWROUTE_TABLE_NAME

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

## <a name="set-up-firewall-rules"></a>Configurer des règles de pare-feu 

```azurecli
# Add FW Network Rules

az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apiudp' --protocols 'UDP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 1194 --action allow --priority 100
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apitcp' --protocols 'TCP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 9000
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'time' --protocols 'UDP' --source-addresses '*' --destination-fqdns 'ntp.ubuntu.com' --destination-ports 123

# Add FW Application Rules

az network firewall application-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwar' -n 'fqdn' --source-addresses '*' --protocols 'http=80' 'https=443' --fqdn-tags "AzureKubernetesService" --action allow --priority 100
```

Vous pouvez associer la table d’itinéraire défini par l’utilisateur (UDR) au cluster AKS dans lequel vous avez précédemment déployé le BDC à l’aide de la commande suivante :

```azurecli
az network vnet subnet update -g $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="create--configure-service-principal-sp"></a>Créer et configurer le principal de service (SP)

Au cours de cette étape, vous devez créer le principal de service et attribuer une autorisation au réseau virtuel.

Voir l’exemple suivant : 

```azurecli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "bdcaks-sp" --skip-assignment

APPID=<your service principal ID >
PASSWORD=< your service principal password >
VNETID=$(az network vnet show -g $RESOURCE_GROUP --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role "Network Contributor"


RTID=$(az network route-table show -g $RESOURCE_GROUP -n $FWROUTE_TABLE_NAME --query id -o tsv)
az role assignment create --assignee $APPID --scope $RTID --role "Network Contributor"
```

## <a name="create-aks-cluster"></a>Créer un cluster AKS

Ensuite, créez le cluster AKS avec `userDefinedRouting` comme type sortant.

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --outbound-type userDefinedRouting \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --service-principal $APPID \
    --client-secret $PASSWORD \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Générer un profil de déploiement de cluster Big Data (BDC)

Vous pouvez créer des clusters BDC avec un profil personnalisé : 

```console
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

## <a name="generate-and-config-bdc-custom-deployment-profile"></a>Générer et configurer le profil de déploiement personnalisé du BDC : 
```console
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Déployer un BDC dans un cluster privé AKS

```console
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="use-third-party-firewall-instead-of-azure-firewall"></a>Utiliser un pare-feu tiers au lieu du pare-feu Azure

Utilisez un pare-feu tiers pour limiter le trafic sortant lors du déploiement du BDC avec un cluster privé AKS. Par exemple, consultez les [Pare-feu de la Place de marché Azure](https://azuremarketplace.microsoft.com/marketplace/apps?search=firewall&page=1). Les pare-feu tiers peuvent être utilisés dans des solutions de déploiement privées avec des configurations plus conformes. Le pare-feu doit fournir les règles de réseau suivantes :

* Toutes les [règles de réseau sortantes et noms FQDN requis pour les clusters AKS](/azure/aks/limit-egress-traffic#required-outbound-network-rules-and-fqdns-for-aks-clusters) et tous les points de terminaison et dépendances HTTP/HTTPS génériques qui peuvent varier avec votre cluster AKS en fonction d’un certain nombre de qualificateurs et de vos besoins réels.
* Les règles de réseau/noms de domaine complet/règles d’application obligatoires pour Azure Global, comme indiqué [ici](/azure/aks/limit-egress-traffic#azure-global-required-network-rules).
* Règles de nom FQDN/d’application facultatives mais recommandées pour les clusters AKS mentionnées [ici](/azure/aks/limit-egress-traffic#optional-recommended-fqdn--application-rules-for-aks-clusters). 

Vérifiez comment [gérer le BDC dans un cluster privé AKS](private-manage.md), puis l’étape suivante consiste à se [connecter au cluster BDC](connect-to-big-data-cluster.md).

Consultez les scripts d’automatisation pour ce scénario dans le [Référentiel d’exemples SQL Server sur GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Étapes suivantes

[Gérer un cluster Big Data dans un cluster privé AKS](private-manage.md)

[Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md)

---
title: Gérer des clusters Big Data (BDC) dans un cluster privé Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment gérer un cluster Big Data SQL Server dans un cluster privé Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 768818a9e034961aa0460515e3db84ce3a4a8092
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043869"
---
# <a name="manage-big-data-cluster-in-aks-private-cluster"></a>Gérer un cluster Big Data dans un cluster privé AKS

Cet article explique comment gérer un cluster privé Azure Kubernetes Service (AKS) avec des clusters Big Data (BDC) SQL Server déployés dans Azure.

Comme décrit dans [Créer un cluster privé](/azure/aks/private-clusters/), le point de terminaison du serveur de l’API de cluster privé AKS n’a pas d’adresse IP publique. Pour gérer le serveur d’API, utilisez une machine virtuelle qui a accès au réseau virtuel Azure (VNet) du cluster AKS.

## <a name="azure-vm---same-vnet"></a>Machine virtuelle Azure - même réseau virtuel

La méthode la plus simple consiste à déployer une machine virtuelle Azure dans le même réseau virtuel que le cluster AKS.

1. Déployez une machine virtuelle Azure dans le même réseau virtuel avec votre cluster AKS. On l’appelle parfois *serveur de rebond (jumpbox)* .
1. Connectez-vous à cette machine virtuelle et [Installez les outils de Big Data SQL Server 2019](deployment-guidance.md#install-sql-server-2019-big-data-tools).

Pour des raisons de sécurité, vous pouvez utiliser les fonctionnalités d’AKS pour que les plages d’adresses IP autorisées du serveur d’API limitent l’accès au serveur d’API (dans le plan de contrôle d’AKS). L’accès limité autorise des adresses IP spécifiques, comme une machine virtuelle jumpbox ou une machine virtuelle de gestion, ou une plage d’adresses IP pour un groupe de développeurs, et l’adresse IP du serveur frontal public du pare-feu.

## <a name="other-options"></a>Autres options

Les alternatives à l’utilisation d’un jumpbox sont les suivantes :

* Utilisez une machine virtuelle dans un réseau distinct et configurez l'[appairage de réseaux virtuels](/azure/virtual-network/virtual-network-peering-overview) sur le réseau virtuel.

* Connexion [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) ou par [passerelle VPN](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

   [Les options de connexion au cluster privé](/azure/aks/private-clusters#options-for-connecting-to-the-private-cluster) décrivent chacune de ces méthodes ci-dessus.

* Si votre service s’exécute derrière un [équilibreur de charge standard Azure](/azure/aks/load-balancer-standard), il peut être activé pour [Azure Private Link](/azure/private-link/private-link-service-overview#limitations). Avec Azure Private Link, vous pouvez activer l’accès privé à partir d’autres réseaux virtuels Azure.

* Dans un scénario hybride, vous pouvez également configurer une connexion par [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) ou [passerelle VPN](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="next-steps"></a>Étapes suivantes

[Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md)
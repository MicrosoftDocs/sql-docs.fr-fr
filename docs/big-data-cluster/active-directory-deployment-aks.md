---
title: Effectuer un déploiement dans Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Explique les concepts et les informations de planification permettant de déployer des clusters Big Data SQL Server en mode AD sur Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e59fd3f4eacab796117172370a83a0184ed47914
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047914"
---
# <a name="deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Déployer des clusters Big Data SQL Server en mode AD sur Azure Kubernetes Service (AKS)

Les clusters Big Data SQL Server prennent en charge le [mode de déploiement Active Directory (AD)](./active-directory-prerequisites.md) pour la **gestion des identités et des accès (IAM)** . La création de la gestion des identités et des accès (IAM) pour **Azure Kubernetes Service (AKS)** s’est révélée difficile, car les protocoles standard comme OAuth 2.0 et OpenID Connect, qui sont très bien pris en charge par la Plateforme d’identité Microsoft, ne le sont pas par SQL Server.  

Cet article explique comment déployer un cluster Big Data en mode AD dans [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes). 

## <a name="architecture-topologies"></a>Topologies d’architecture

**Active Directory Domain Services (AD DS)** s’exécute sur une machine virtuelle Azure [de la même manière](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) que sur de nombreuses instances locales.  Après avoir promu les nouveaux contrôleurs de domaine dans Azure, définissez le serveur DNS principal et le serveur DNS secondaire du réseau virtuel, puis abaissez le niveau des serveurs DNS locaux vers un niveau tertiaire ou inférieur. L’authentification AD permet aux clients joints à un domaine sur [Linux de s’authentifier auprès de SQL Server](../linux/sql-server-linux-active-directory-auth-overview.md) en utilisant leurs informations d’identification de domaine et le protocole Kerberos.

Il existe plusieurs façons de déployer un cluster Big Data en mode AD dans AKS.  Cet article présente deux méthodes qui sont plus faciles à implémenter et à intégrer aux architectures existantes de niveau entreprise :

* **Étendre le domaine Active Directory local à Azure.** Cette méthode [permet à votre environnement Active Directory](/azure/architecture/reference-architectures/identity/adds-extend-domain) de fournir des services d’authentification distribuée à l’aide d’Active Directory Domain Services (AD DS) sur Azure. Vous répliquez votre instance locale d’Active Directory Domain Services (AD DS) pour réduire la latence provoquée par l’envoi des demandes d’authentification entre le cloud et l’instance locale d’AD DS. Il existe un cas d’usage classique pour cette solution : lorsque votre application est hébergée en partie localement et en partie dans Azure, et que vos demandes d’authentification doivent constamment faire l’aller-retour.

   Pour savoir comment déployer cette solution étape par étape, consultez [cette architecture de référence](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain).

* **Étendre la forêt de ressources Active Directory Domain Services (AD DS) à Azure.** Dans cette architecture, vous créez dans Azure un domaine qui est approuvé par votre forêt AD locale. Cette architecture montre une [approbation unidirectionnelle entre le domaine dans Azure et la forêt locale](/azure/architecture/reference-architectures/identity/adds-forest).

   Cette approbation permet aux utilisateurs locaux d’accéder aux ressources du domaine dans Azure. Pour savoir comment déployer cette solution étape par étape, consultez [cette architecture de référence](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest).

Les architectures de référence décrites ci-dessus vous permettent de créer une zone d’atterrissage comprenant toutes les ressources à déployer de zéro, ainsi que toute autre solution de contournement basée sur l’architecture existante. En plus de ces architectures de référence, vous devez déployer le cluster Big Data dans un cluster AKS situé sur un sous-réseau distinct mais toujours compris dans votre réseau virtuel cible ou réseau virtuel appairé.

L’image suivante représente une architecture classique :

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Cluster AKS avec Active Directory et cluster Big Data SQL Server":::

## <a name="recommendations"></a>Recommandations

Les recommandations suivantes s’appliquent à la plupart des déploiements de clusters Big Data en mode AD sur AKS. Les options disponibles seront mentionnées dans chaque composant afin de vous aider à choisir la meilleure intégration pour l’architecture de niveau entreprise.

### <a name="networking-recommendations"></a>Recommandations pour la mise en réseau

Quelques composants clés peuvent être utilisés pour connecter votre environnement local à Azure :

* **Passerelle VPN Azure** : Une passerelle VPN est une passerelle de réseau virtuel qui est utilisée pour envoyer du trafic chiffré entre un réseau virtuel Azure et un emplacement local via l’Internet public. Vous allez utiliser à la fois la passerelle de réseau virtuel Azure et la passerelle de réseau virtuel local. Pour plus d’informations sur leur configuration, consultez [Qu’est-ce qu’une passerelle VPN ?](/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* **Azure ExpressRoute** : Les connexions ExpressRoute ne sont pas établies via le réseau public Internet et offrent plus de sécurité, de fiabilité et de rapidité, ainsi que moins de latence que les connexions classiques sur Internet. Le choix de votre option de connectivité aura une incidence sur la latence, les performances et le niveau de contrat SLA de votre solution, en fonction de la référence SKU. Pour plus d’informations, consultez [À propos des passerelles de réseau virtuel ExpressRoute](/azure/expressroute/expressroute-about-virtual-network-gateways).

La plupart des clients utilisent un modèle Jumpbox ou [Azure Bastion](/azure/bastion/bastion-overview) pour accéder à une autre infrastructure Azure. **Azure Private Link** vous permet d’accéder de façon sécurisée aux services Azure PaaS (y compris à AKS dans ce scénario), ainsi qu’à d’autres services hébergés dans Azure, par le biais d’un point de terminaison privé situé dans votre réseau virtuel. Le trafic entre votre réseau virtuel et le service transite par le réseau principal de Microsoft, éliminant ainsi toute exposition à l’Internet public. Vous pouvez également créer votre propre service Private Link dans votre réseau virtuel et le distribuer en privé à vos clients.

### <a name="active-directory-and-azure-recommendation"></a>Recommandations concernant Active Directory et Azure

L’instance locale d’AD DS stocke des informations sur les comptes d’utilisateur et permet à d’autres utilisateurs autorisés appartenant au même réseau d’accéder à ces informations en authentifiant les identités associées aux utilisateurs, ordinateurs, applications ou autres ressources incluses dans une limite de sécurité. Dans la plupart des scénarios hybrides, l’authentification de l’utilisateur a lieu lors de la connexion à votre environnement AD DS local via ExpressRoute ou la passerelle VPN.  

Pour un déploiement de cluster Big Data en mode AD, la solution permettant d’[intégrer l’instance locale d’Active Directory à Azure](/azure/architecture/reference-architectures/identity/) doit respecter les prérequis suivants :

* Un [compte AD disposant d’une autorisation spécifique](active-directory-prerequisites.md) qui permet de créer des utilisateurs, des groupes et des comptes d’ordinateurs à l’intérieur de l’unité d’organisation indiquée dans votre annuaire Active Directory local.
* Un serveur DNS pour la [résolution des DNS internes](active-directory-dns-reconciliation.md). Il doit contenir à la fois des **enregistrements A (recherche directe)** et des **enregistrements PTR (recherche inversée)** dans le serveur DNS avec les noms de ce domaine. Spécifiez les paramètres du DNS de domaine dans le profil de déploiement du cluster Big Data.  

## <a name="next-steps"></a>Étapes suivantes

[Tutoriel : Déployer des clusters Big Data SQL Server en mode AD sur Azure Kubernetes Service (AKS)](active-directory-deployment-aks-tutorial.md)
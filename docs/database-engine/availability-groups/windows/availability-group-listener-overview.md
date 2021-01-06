---
title: Qu’est-ce qu’un écouteur de groupe de disponibilité ?
description: 'Vue d’ensemble de l’écouteur du groupe de disponibilité Always On et de la façon dont il route automatiquement le trafic vers le serveur prévu. '
ms.custom: seodec18
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2dc23852fc5792d29c25b151f7c6180ba32295ce
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641140"
---
# <a name="what-is-an-availability-group-listener"></a>Qu’est-ce qu’un écouteur de groupe de disponibilité ?  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Un écouteur de groupe de disponibilité est un nom de réseau virtuel (VNN) auquel les clients peuvent se connecter afin d’accéder à une base de données dans un réplica principal ou secondaire d’un groupe de disponibilité Always On. Avec un écouteur, un client peut se connecter à un réplica sans avoir à fournir le nom de l’instance physique du serveur SQL Server. Du fait que l’écouteur route le trafic, il n’est pas nécessaire de modifier la chaîne de connexion du client après un basculement. 

Un écouteur de groupe de disponibilité consiste en un nom d'écouteur DNS (Domain Name System), en une désignation de port d'écoute et en une ou plusieurs adresses IP. Seul le protocole TCP est pris en charge par l'écouteur de groupe de disponibilité.  Le nom DNS de l'écouteur doit être unique dans le domaine et dans NetBIOS.  Quand vous créez un écouteur, celui-ci devient une ressource de cluster, à laquelle sont associés un nom de réseau virtuel (VNN), une adresse IP virtuelle (VIP) et une dépendance de groupe de disponibilité. Un client utilise le nom DNS pour résoudre le nom VNN en plusieurs adresses IP et tente ensuite de se connecter à chaque adresse, jusqu'à ce qu'une demande de connexion réussisse ou jusqu'à ce que la requête de connexion expire.  
  
Si le routage en lecture seule est configuré pour un ou plusieurs [réplicas secondaires accessibles en lecture](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md), les connexions clientes d’intention de lecture établies avec l’écouteur sont automatiquement redirigées vers un réplica secondaire accessible en lecture. 
  
Cet article offre une vue d’ensemble d’un écouteur de groupe de disponibilité. Vous pouvez également [configurer l’écouteur](create-or-configure-an-availability-group-listener-sql-server.md), puis découvrir comment [établir une connexion à l’écouteur](listeners-client-connectivity-application-failover.md).
  
  
##  <a name="listener-parameters"></a><a name="AGlConfig"></a> Paramètres de l’écouteur  

 Un écouteur de groupe de disponibilité utilise les paramètres suivants :
  
 **Un nom DNS unique**  
 Ce nom est aussi appelé un nom de réseau virtuel (VNN). Les règles d'attribution de noms Active Directory pour les noms d'hôte DNS s'appliquent. Pour plus d'informations, consultez l'article [Conventions d'affectation de noms dans Active Directory pour les ordinateurs, les domaines, les sites et les unités d'organisation](https://support.microsoft.com/kb/909264) de la Base de connaissances.  
  
**Une ou plusieurs adresses IP virtuelles (VIP)**  
 Les adresses IP virtuelles sont configurées pour un ou plusieurs sous-réseaux vers lesquels le groupe de disponibilité peut basculer.  
  
**Configuration d’adresse IP**  
 Pour un écouteur de groupe de disponibilité donné, l’adresse IP peut utiliser le protocole DHCP (Dynamic Host Configuration Protocol) ou bien une ou plusieurs adresses IP statiques. L’utilisation de DHCP peut augmenter les délais de connexion durant le basculement. Ce protocole n’est donc pas recommandé dans des environnements de production. Les groupes de disponibilité qui s’étendent sur plusieurs sous-réseaux ou qui utilisent des configurations réseau hybrides doivent utiliser une adresse IP statique. 
 
  
##  <a name="listener-port"></a><a name="SelectListenerPort"></a> Port de l’écouteur 
 Lors de la configuration d'un écouteur de groupe de disponibilité, vous devez indiquer un port.  Vous pouvez configurer le port par défaut sur 1433, afin de permettre de simplifier les chaînes de connexion du client. Si vous utilisez 1433, vous n'avez pas besoin d'indiquer un numéro de port dans une chaîne de connexion. De plus, étant donné que chaque écouteur de groupe de disponibilité portera un nom de réseau virtuel distinct, chaque écouteur de groupe de disponibilité configuré sur un même WSFC pourra être configuré pour référencer le même port par défaut 1433.  
  
 Vous pouvez également indiquer un port d'écoute non standard ; toutefois, cela signifie que vous devrez également spécifier explicitement un port cible dans votre chaîne de connexion lors de chaque connexion à l'écouteur de groupe de disponibilité.  Vous devrez également ouvrir l'autorisation sur le pare-feu pour le port non standard.  
  
 Si vous utilisez le port par défaut 1433 comme nom VNN de l'écouteur du groupe de disponibilité, vous devez toujours vérifier qu'aucun autre service sur le nœud de cluster n'utilise ce port ; dans le cas contraire, cela provoquerait un conflit de ports.  
  
 Si l'une des instances de SQL Server écoute déjà sur le port TCP 1433 par l'intermédiaire de l'écouteur d'instance et qu'il n'existe aucun autre service (instances supplémentaires de SQL Server comprises) sur l'ordinateur écoutant sur le port 1433, cela ne provoque pas un conflit de ports avec l'écouteur du groupe de disponibilité.  Cela est dû au fait que l'écouteur du groupe de disponibilité peut partager le même port TCP au sein du même processus de service.  Toutefois, plusieurs instances de SQL Server (côte à côte) ne doivent pas être configurées pour écouter sur le même port.  
  
  
##  <a name="behavior-of-client-connections-on-failover"></a><a name="CCBehaviorOnFailover"></a> Comportement des connexions clientes lors du basculement  

 Lorsqu'un basculement de groupe de disponibilité se produit, les connexions persistantes existantes au groupe de disponibilité prennent fin et le client doit établir une nouvelle connexion afin de continuer à utiliser la même base de données primaire ou base de données secondaire en lecture seule.  Lorsqu'un basculement se produit côté serveur, la connectivité au groupe de disponibilité peut échouer, forçant l'application cliente à réessayer une nouvelle connexion jusqu'à ce que le serveur principal soit entièrement remis en ligne.  
  
 Si le groupe de disponibilité revient en ligne pendant une tentative de connexion d’une application cliente, mais avant l’expiration du délai d’attente de connexion, le pilote client peut se connecter au cours de l’une de ses tentatives de reprise interne, auquel cas, aucune erreur n’est visible dans l’application.  


## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous êtes familiarisé avec le fonctionnement d’un écouteur de groupe de disponibilité, [créez votre écouteur](create-or-configure-an-availability-group-listener-sql-server.md), puis configurez votre application pour la [connecter à l’écouteur](listeners-client-connectivity-application-failover.md). Vous pouvez également examiner différentes [stratégies de supervision des groupes de disponibilité](monitoring-of-availability-groups-sql-server.md) pour garantir l’intégrité de votre groupe de disponibilité. 

Pour plus d’informations sur les groupes de disponibilité, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). 
  

  
  

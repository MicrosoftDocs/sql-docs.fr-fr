---
description: Ajouter des dépendances à une ressource SQL Server
title: Ajouter des dépendances à une ressource FCI SQL Server
descriptoin: Describes how to add dependencies to an Always On failover cluster instance (FCI) resource using the Failover Cluster Manager.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- resource dependencies [SQL Server]
- failover clustering [SQL Server], dependencies
- clusters [SQL Server], dependencies
- dependencies [SQL Server], clustering
ms.assetid: 25dbb751-139b-4c8e-ac62-3ec23110611f
author: cawrites
ms.author: chadam
ms.openlocfilehash: f9582167d337ed9a06f95030f4c68857a4abd389
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642839"
---
# <a name="add-dependencies-to-a-sql-server-resource"></a>Ajouter des dépendances à une ressource SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment ajouter des dépendances à une ressource d’instance de cluster de basculement (FCI) Always On à l’aide du composant logiciel enfichable Gestionnaire du cluster de basculement. Le composant logiciel enfichable Gestionnaire du cluster de basculement est l'application de gestion du service de cluster de basculement Windows Server (WSFC).  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [Conditions préalables requises](#Prerequisites)  
  
-   **Pour ajouter une dépendance à une ressource SQL Server à l'aide du** [Gestionnaire du cluster de basculement Windows](#WinClusManager)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Il est important de noter que si vous ajoutez d'autres ressources au groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , celles-ci doivent avoir obligatoirement un nom de réseau SQL unique et une adresse IP SQL propre.  
  
 N'utilisez pas de ressources SQL existantes de noms sur le réseau et d'adresses IP pour autre chose que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si les ressources [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont partagées avec d'autres ressources, les problèmes suivants peuvent se poser :  
  
-   Des pannes inattendues peuvent se produire.  
  
-   Les installations des Service Packs peuvent échouer.  
  
-   Le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut échouer. Si ce problème se produit, vous ne pouvez pas installer d'autres instances [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou effectuer des opérations de maintenance régulière.  
  
 Tenez compte de ces problèmes supplémentaires :  
  
-   FTP avec la réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] : pour les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui utilisent FTP avec la réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , votre service FTP doit utiliser un des mêmes disques physiques que pour l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurée pour utiliser le service FTP.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] : si vous ajoutez une ressource à un groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et s'il existe une dépendance sur la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour garantir que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est disponible, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] vous recommande d'ajouter une dépendance à la ressource de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . N'ajoutez pas une dépendance à la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour garantir que l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] reste hautement disponible, configurez la ressource de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent de façon à ce qu’elle n’affecte pas le groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en cas d’échec de cette dernière. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
-   Ressources de partage de fichiers ou d'imprimantes : lorsque vous installez des ressources de partage de fichiers ou de clusters d'imprimantes, celles-ci ne doivent pas se trouver sur les mêmes ressources des disques physiques que l'ordinateur qui exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si elles se trouvent sur les ressources des disques physiques, vous pouvez constater une dégradation des performances et une perte des services sur l'ordinateur qui exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Considérations relatives à MS DTC : après avoir installé le système d'exploitation et configuré votre FCI, vous devez configurer le Coordinateur de transactions distribuées (MS DTC) de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour travailler dans un cluster à l'aide du composant logiciel enfichable Gestionnaire du cluster de basculement. L'échec de la mise en cluster de MS DTC ne bloquera pas l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , mais les fonctionnalités des applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent être affectées si MS DTC n'est pas configuré correctement.  
  
     Si vous installez MS DTC dans votre groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et si d'autres ressources dépendent de MS DTC, MS DTC ne sera pas disponible si ce groupe est hors connexion ou lors d'un basculement. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recommande de placer MS DTC dans un groupe distinct avec sa propre ressource de disque physique, si possible.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Si vous installez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans un groupe de ressources WSFC qui comporte plusieurs lecteurs de disques et si vous choisissez de placer vos données sur un des lecteurs, la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sera configurée comme étant dépendante uniquement sur ce lecteur. Pour placer les données ou les journaux sur un autre disque, vous devez d'abord ajouter une dépendance à la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour le disque supplémentaire.  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WinClusManager"></a> Utilisation du composant logiciel enfichable Gestionnaire du cluster de basculement  
 **Pour ajouter une dépendance à une ressource SQL Server**  
  
-   Ouvrez le composant logiciel enfichable Gestionnaire du cluster de basculement.  
  
-   Recherchez le groupe qui contient la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] applicable que vous voulez rendre dépendante.  
  
-   Si la ressource du disque se trouve déjà dans ce groupe, passez à l'étape 4. Sinon, recherchez le groupe qui contient le disque. Si ce groupe et le groupe qui contient [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'appartiennent pas au même nœud, déplacez le groupe qui contient la ressource du disque vers le nœud propriétaire du groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Sélectionnez la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ouvrez la boîte de dialogue **Propriétés** et utilisez l'onglet **Dépendances** pour ajouter le disque à l'ensemble de dépendances [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
  

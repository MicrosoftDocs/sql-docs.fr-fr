---
title: Récupérer à partir d’une défaillance de cluster de basculement
description: Découvrez comment effectuer une récupération après le basculement d’une instance de cluster de basculement à l’aide du composant logiciel enfichable Gestionnaire du cluster de basculement dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], recovery from failure
- failover clustering [SQL Server], recovery from failure
- hardware failures [SQL Server]
- recovering failover cluster failures [SQL Server]
ms.assetid: 3d151d0c-e841-4325-8606-c094de37d7d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 91e18137748b34012c73860071fa0be93af0277f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642748"
---
# <a name="recover-from-failover-cluster-instance-failure"></a>Récupérer à partir d’une défaillance de cluster de basculement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment récupérer des échecs de cluster à l'aide du composant logiciel enfichable Gestionnaire du cluster de basculement après un basculement dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Le composant logiciel enfichable Gestionnaire du cluster de basculement est l'application de gestion du service de cluster de basculement Windows Server (WSFC).  
  
-   [Récupérer en cas d'erreur irréparable](#Scenario1)  
  
-   [Récupérer en cas d'erreur logicielle](#Scenario2)  
  
##  <a name="recover-from-an-irreparable-failure"></a><a name="Scenario1"></a> Récupérer en cas d'erreur irréparable  
 Utilisez les étapes suivantes pour récupérer une erreur irréparable. L'erreur peut être provoquée, par exemple, par la défaillance d'un contrôleur de disque ou du système d'exploitation. Dans ce cas, l'erreur est due à une défaillance matérielle dans le nœud 1 d'un cluster à deux nœuds.  
  
1.  Après la défaillance du nœud 1, la FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bascule sur le nœud 2.  
  
2.  Supprimez le nœud 1 de la FCI. Pour cela, à partir du nœud 2, ouvrez le composant logiciel enfichable Gestionnaire du cluster de basculement, cliquez avec le bouton droit sur Node1, cliquez sur **Déplacer des actions**, puis sur **Supprimer le nœud**.  
  
3.  Assurez-vous que le nœud 1 a été supprimé de la définition de cluster.  
  
4.  Installez le nouveau matériel pour remplacer le matériel défaillant dans le nœud 1.  
  
5.  Ajoutez le nœud 1 au cluster existant à l'aide du composant logiciel enfichable Gestionnaire du cluster de basculement. Pour plus d’informations, consultez [Avant l’installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
6.  Vérifiez que les comptes administrateurs sont bien les mêmes sur tous les nœuds de cluster.  
  
7.  Exécutez le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] afin d'ajouter le nœud 1 à la FCI. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
##  <a name="recover-from-a-reparable-failure"></a><a name="Scenario2"></a> Récupérer d'une erreur réparable  
 Utilisez les étapes suivantes pour récupérer suite à une erreur réparable. Dans ce cas, la défaillance est causée par le nœud 1, défectueux ou hors connexion, mais il n'est pas rompu de manière irrémédiable. Cette erreur peut être due à une défaillance du système d'exploitation, à une défaillance matérielle ou à une défaillance dans l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] elle-même.  
  
1.  Après la défaillance du nœud 1, la FCI bascule sur le nœud 2.  
  
2.  Résolvez le problème du nœud 1.  
  
3.  Vérifiez que le service WSFC fonctionne et que tous les nœuds sont en ligne.  
  
4.  Effectuez le basculement de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers le nœud récupéré.  
  
  

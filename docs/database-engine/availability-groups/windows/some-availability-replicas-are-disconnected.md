---
title: Certains réplicas de disponibilité sont déconnectés
description: Causes possibles et méthodes de résolution quand le réplica de groupe de disponibilité est déconnecté d’un groupe de disponibilité Always On SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp7allconnected.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: aea808be-6f0f-40c2-9aa2-a2a435ec6443
author: cawrites
ms.author: chadam
ms.openlocfilehash: cfa9fb0297f4ba3e9cdab859816ecfc4d8fb54bf
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583866"
---
# <a name="some-availability-replicas-are-disconnected"></a>Certains réplicas de disponibilité sont déconnectés
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État de la connexion des réplicas de disponibilité|  
|**Problème**|Certains réplicas de disponibilité sont déconnectés.|  
|**Catégorie**|**Avertissement**|  
|**Facette**|Groupe de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie détermine l'état de la connexion de tous les réplicas de disponibilité et recherche les réplicas de disponibilité dont l'état est DISCONNECTED. La stratégie se trouve dans un état non sain lorsque l'état d'un réplica de disponibilité est DISCONNECTED. Autrement, l'état de la stratégie est sain.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles sont situées sous [Certains réplicas de disponibilité sont déconnectés](https://go.microsoft.com/fwlink/p/?LinkId=220855) sur TechNet Wiki.  
  
## <a name="possible-causes"></a>Causes possibles  
 Dans ce groupe de disponibilité, au moins un réplica secondaire n'est pas connecté au réplica principal. L'état de la connexion est DISCONNECTED.  
  
## <a name="possible-solution"></a>Solution possible  
 Utilisez l'état de la stratégie de réplica de disponibilité pour rechercher le réplica de disponibilité dont l'état est DISCONNECTED, puis résolvez le problème qui affecte le réplica de disponibilité.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

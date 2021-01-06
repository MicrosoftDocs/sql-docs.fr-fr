---
title: Certains réplicas de disponibilité n’ont pas un rôle sain | Microsoft Docs
description: Availability Replicas Role State détermine l'état de connexion de tous les réplicas de disponibilité et vérifie s'il existe des réplicas de disponibilité qui n'ont pas un rôle sain.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp6allroleshealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 7ec5b337-7201-4a66-a541-7560f8b18784
author: cawrites
ms.author: chadam
ms.openlocfilehash: db542dca2f4c200764e5065bff155c908fafcd25
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641412"
---
# <a name="some-availability-replicas-do-not-have-a-healthy-role"></a>Certains réplicas de disponibilité n'ont pas un rôle sain
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État du rôle des réplicas de disponibilité|  
|**Problème**|Certains réplicas de disponibilité n'ont pas un rôle sain.|  
|**Catégorie**|**Avertissement**|  
|**Facette**|Groupe de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie détermine l'état de connexion de tous les réplicas de disponibilité et vérifie s'il existe des réplicas de disponibilité qui n'ont pas un rôle sain. La stratégie se trouve dans un état non sain lorsqu'un réplica de disponibilité n'est ni principal ni secondaire. Autrement, l'état de la stratégie est sain.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles sont situées sous [Certains réplicas de disponibilité n’ont pas un rôle sain](https://go.microsoft.com/fwlink/p/?LinkId=220854) sur TechNet Wiki.  
  
## <a name="possible-causes"></a>Causes possibles  
 Dans ce groupe de disponibilité, au moins un réplica de disponibilité n'a pas de rôle principal ou secondaire pour le moment.  
  
## <a name="possible-solution"></a>Solution possible  
 Utilisez l'état de la stratégie de réplica de disponibilité pour rechercher le réplica de disponibilité dont le rôle n'est ni principal ni secondaire, puis résolvez le problème qui affecte le réplica de disponibilité.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

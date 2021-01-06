---
title: L'état de synchronisation des données d'une base de données de disponibilité n'est pas sain
description: Identifiez les raisons possibles pour lesquelles l’état de synchronisation des données d’une base de données dans un groupe de disponibilité Always On n’est pas sain.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 89f95d15-33c6-4768-bccd-9dbf8c4f49a9
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab55e83942a2591a335759ae3df82b42d3d9db9b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643245"
---
# <a name="data-synchronization-state-of-some-availability-database-is-not-healthy"></a>L'état de synchronisation des données d'une base de données de disponibilité n'est pas sain
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État de synchronisation des données du réplica de disponibilité|  
|**Problème**|L'état de synchronisation des données d'une base de données de disponibilité n'est pas sain.|  
|**Catégorie**|**Avertissement**|  
|**Facette**|Réplica de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie l'état de synchronisation des données de la base de données de disponibilité (également appelée « réplica de base de données »). La stratégie se trouve dans un état non sain lorsque l'état de synchronisation de données est NOT SYNCHRONIZING ou que l'état du réplica de base de données à validation synchrone n'est pas SYNCHRONIZED.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], vous trouverez des informations sur les causes et les solutions possibles dans la page [L’état de synchronisation des données d’une base de données de disponibilité n’est pas sain](https://go.microsoft.com/fwlink/p/?LinkId=220863) sur le wiki TechNet.  
  
## <a name="possible-causes"></a>Causes possibles  
 Au moins une base de données de disponibilité sur le réplica a un état de synchronisation des données non sain. S'il s'agit d'un réplica de disponibilité en validation asynchrone, toutes les bases de données de disponibilité doivent être dans l'état SYNCHRONIZING. S'il s'agit d'un réplica de disponibilité à validation synchrone, toutes les bases de données de disponibilité doivent être dans l'état SYNCHRONIZED. Ce problème peut avoir les causes suivantes :  
  
-   Le réplica de disponibilité est peut-être déconnecté.  
  
-   Le déplacement des données est peut-être suspendu.  
  
-   La base de données n'est peut-être pas accessible.  
  
-   Il peut exister un problème de délai temporaire en raison de la latence réseau ou de la charge sur le réplica principal ou secondaire.  
  
## <a name="possible-solution"></a>Solution possible  
 Résolvez tout problème de connexion ou de suspension du déplacement des données. Vérifiez les événements liés à ce problème à l'aide de SQL Server Management Studio et recherchez l'erreur de base de données. Suivez les étapes de résolution spécifiques à cette erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

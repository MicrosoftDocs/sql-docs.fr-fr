---
title: La base de données secondaire n’est pas attachée | Microsoft Docs
description: L’État de jointure de la base de données de disponibilité vérifie l’état de jointure de la base de données secondaire dans le cadre de la gestion basée sur des stratégies pour groupes de disponibilité Always On.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 39cbbe7a884d5e8fe45d0877b7a581016e1e95c2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640545"
---
# <a name="secondary-database-is-not-joined"></a>La base de données secondaire n'est pas attachée
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État de la jointure de la base de données de disponibilité|  
|**Problème**|La base de données secondaire n'est pas jointe.|  
|**Catégorie**|**Avertissement**|  
|**Facette**|Base de données de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie l'état de jointure de la base de données secondaire (également appelée « réplica de base de données secondaire »). La stratégie se trouve dans un état non sain lorsque le réplica de dataset n'est pas joint. Autrement, l'état de la stratégie est sain.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles sont situées sous [La base de données secondaire n’est pas jointe](https://go.microsoft.com/fwlink/p/?LinkId=220862) sur le Wiki TechNet.  
  
## <a name="possible-causes"></a>Causes possibles  
 Cette base de données secondaire n'est pas jointe au groupe de disponibilité. La configuration de cette base de données secondaire est incomplète.  
  
## <a name="possible-solution"></a>Solution possible  
 Utilisez Transact-SQL, PowerShell ou SQL Server Management Studio pour joindre le réplica secondaire au groupe de disponibilité. Pour plus d’informations sur la jointure des réplicas secondaires aux groupes de disponibilité, consultez [Jointure d’un réplica secondaire à un groupe de disponibilité (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

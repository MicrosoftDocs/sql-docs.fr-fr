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
ms.openlocfilehash: 1e41945d69a499908d11d8ebc8cbda7688d544a8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766098"
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

## <a name="possible-causes"></a>Causes possibles  
 Cette base de données secondaire n'est pas jointe au groupe de disponibilité. La configuration de cette base de données secondaire est incomplète.  
  
## <a name="possible-solution"></a>Solution possible  
 Utilisez Transact-SQL, PowerShell ou SQL Server Management Studio pour joindre le réplica secondaire au groupe de disponibilité. Pour plus d’informations sur la jointure des réplicas secondaires aux groupes de disponibilité, consultez [Jointure d’un réplica secondaire à un groupe de disponibilité (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

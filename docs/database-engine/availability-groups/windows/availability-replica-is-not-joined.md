---
title: Le réplica de disponibilité n’est pas joint à un groupe de disponibilité
description: Découvrez comment identifier les raisons possibles pour lesquelles un réplica n’est pas joint à un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp4joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 9c0d10b1-9e12-430c-83b9-ca2bd0a3afc4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d04b8a881ca006264c0258922a045102a23be92b
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584567"
---
# <a name="availability-replica-is-not-joined-to-an-always-on-availability-group"></a>Le réplica de disponibilité n’est pas joint à un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État de jointure du réplica de disponibilité|  
|**Problème**|Le réplica de disponibilité n'est pas joint.|  
|**Catégorie**|**Avertissement**|  
|**Facette**|Réplica de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie l'état de jointure du réplica de disponibilité. La stratégie se trouve dans un état défectueux lorsque le réplica de disponibilité est ajouté au groupe de disponibilité, mais n'est pas joint correctement. Autrement, l'état de la stratégie est sain.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles sont situées sous [Le réplica de disponibilité n’est pas joint](https://go.microsoft.com/fwlink/p/?LinkId=220859) sur TechNet Wiki.  
  
## <a name="possible-causes"></a>Causes possibles  
 Le réplica secondaire n'est pas joint au groupe de disponibilité. Pour qu'un réplica de disponibilité soit correctement joint au groupe de disponibilité, l'état de jointure doit être une instance autonome jointe (1) ou un cluster de basculement joint (2).  
  
## <a name="possible-solution"></a>Solution possible  
 Utilisez Transact-SQL, PowerShell ou SQL Server Management Studio pour joindre le réplica secondaire au groupe de disponibilité. Pour plus d’informations sur la jointure des réplicas secondaires aux groupes de disponibilité, consultez [Jointure d’un réplica secondaire à un groupe de disponibilité (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

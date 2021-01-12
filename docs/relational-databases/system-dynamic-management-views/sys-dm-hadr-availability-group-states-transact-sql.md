---
description: sys.dm_hadr_availability_group_states (Transact-SQL)
title: sys.dm_hadr_availability_group_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_availability_group_states
- sys.dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_group_states dynamic management view
ms.assetid: d18019dd-f8dc-4492-b035-b1a639369b65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5cdbf3f295fc8781e25a6df3ae78d63c22a3b6a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092810"
---
# <a name="sysdm_hadr_availability_group_states-transact-sql"></a>sys.dm_hadr_availability_group_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque Always On groupe de disponibilité qui possède un réplica de disponibilité sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Chaque ligne affiche les états qui définissent l'intégrité d'un groupe de disponibilité donné.  
  
> [!NOTE]  
>  Pour obtenir la liste complète de, interrogez l’affichage catalogue [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificateur unique du groupe de disponibilité.|  
|**primary_replica**|**varchar(128)**|Nom de l'instance de serveur qui héberge le réplica principal actuel.<br /><br /> NULL = N'est pas le réplica principal ou impossible de communiquer avec le cluster de basculement WSFC.|  
|**primary_recovery_health**|**tinyint**|Indique l'état de récupération du réplica principal, un des suivants :<br /><br /> 0 = en cours<br /><br /> 1 = En ligne<br /><br /> NULL<br /><br /> Sur les réplicas secondaires, la colonne **primary_recovery_health** a la valeur null.|  
|**primary_recovery_health_desc**|**nvarchar(60)**|Description de **primary_replica_health**, parmi :<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**secondary_recovery_health**|**tinyint**|Indique l’intégrité de récupération d’un réplica de réplica secondaire, parmi les suivants :<br /><br /> 0 = en cours<br /><br /> 1 = En ligne<br /><br /> NULL<br /><br /> Sur le réplica principal, la colonne **secondary_recovery_health** a la valeur null.|  
|**secondary_recovery_health_desc**|**nvarchar(60)**|Description de **secondary_recovery_health**, parmi :<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**synchronization_health**|**tinyint**|Reflète un cumul de la **synchronization_health** de tous les réplicas de disponibilité dans le groupe de disponibilité. Vous trouverez ci-dessous les valeurs possibles et leurs descriptions.<br /><br /> 0 : non intègre. Aucun des réplicas de disponibilité n’a un **synchronization_health** sain (2 = sain).<br /><br /> 1 : partiellement sain. L'état de synchronization de certains des réplicas de disponibilité est sain.<br /><br /> 2 : intègre. L'état de synchronization de chaque réplica de disponibilité est sain.<br /><br /> Pour plus d’informations sur l’intégrité de la synchronisation des réplicas, consultez la **synchronization_health** colonne dans [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md).|  
|**synchronization_health_desc**|**nvarchar(60)**|Description de **synchronization_health**, parmi :<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Vues et fonctions de gestion dynamiques de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  

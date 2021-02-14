---
description: sys.dm_hadr_availability_replica_cluster_nodes (Transact-SQL)
title: sys.dm_hadr_availability_replica_cluster_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_availability_replica_cluster_nodes
- sys.dm_hadr_availability_replica_cluster_nodes_TSQL
- dm_hadr_availability_replica_cluster_nodes_TSQL
- sys.dm_hadr_availability_replica_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_availability_replica_cluster_nodes dynamic management view
ms.assetid: dbd7e416-badd-4332-a45c-438aa0145a99
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 053b2f9778b6da8765690ccd91a00c9d3ea57a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354153"
---
# <a name="sysdm_hadr_availability_replica_cluster_nodes-transact-sql"></a>sys.dm_hadr_availability_replica_cluster_nodes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque réplica de disponibilité (indépendamment de l’état de jointure) des groupes de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC).  

 ##  <a name="connected_state"></a>  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_name**|**nvarchar (256)**|Nom du groupe de disponibilité.|  
|**replica_server_name**|**nvarchar (256)**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge le réplica.|  
|**node_name**|**nvarchar (256)**|Nom du nœud de cluster.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  

---
description: sys.dm_hadr_instance_node_map (Transact-SQL)
title: sys.dm_hadr_instance_node_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sys.dm_hadr_instance_node_map_TSQL
- sys.sys.dm_hadr_instance_node_map
- sys.dm_hadr_instance_node_map_TSQL
- sys.dm_hadr_instance_node_map
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC
- sys.sys.dm_hadr_instance_node_map dynamic management view
ms.assetid: ccfaf62c-9f87-43cf-a5e7-8942e91dd041
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a89e98b9c7a66478367306fe5683f564df4b09bf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092833"
---
# <a name="sysdm_hadr_instance_node_map-transact-sql"></a>sys.dm_hadr_instance_node_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Pour chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge un réplica de disponibilité joint à son groupe de disponibilité Always on, retourne le nom du nœud de cluster de basculement Windows Server (WSFC) qui héberge l’instance de serveur. Cette vue de gestion dynamique permet les utilisations suivantes :  
  
-   Cette vue de gestion dynamique est utile pour détecter un groupe de disponibilité avec plusieurs réplicas de disponibilité hébergés sur le même nœud WSFC, ce qui correspond à une configuration non prise en charge qui peut se produire après un basculement FCI si le groupe de disponibilité n'est pas correctement configuré. Pour plus d’informations, consultez [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
-   Lorsque plusieurs instances de SQL Server sont hébergées sur le même nœud WSFC, la DLL de ressource utilise cette vue de gestion dynamique pour déterminer l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle se connecter.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ag_resource_id**|**nvarchar (256)**|ID unique du groupe de disponibilité en tant que ressource dans le WSFC.|  
|**instance_name**|**nvarchar (256)**|Nom- / *instance* de serveur : d’une instance de serveur qui héberge un réplica pour le groupe de disponibilité.|  
|**node_name**|**nvarchar (256)**|Nom du nœud WSFC.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

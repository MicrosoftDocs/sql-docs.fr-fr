---
description: sys.dm_hadr_name_id_map (Transact-SQL)
title: sys. dm_hadr_name_id_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_name_id_map
- sys.dm_hadr_name_id_map_TSQL
- dm_hadr_name_id_map
- dm_hadr_name_id_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_name_id_map dynamic management view
ms.assetid: e07bb8a9-37de-4a39-a257-950d7c3ae8fb
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: e6c719710c6759f837edb0d29d618e7f32d52187
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88419723"
---
# <a name="sysdm_hadr_name_id_map-transact-sql"></a>sys.dm_hadr_name_id_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche le mappage des groupes de disponibilité Always On que l’instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a joint à trois ID uniques : un ID de groupe de disponibilité, un ID de ressource WSFC et un ID de groupe WSFC. L'objectif de ce mappage est de gérer le scénario dans lequel la ressource/le groupe WSFC est renommé.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ag_name**|**nvarchar (256)**|Nom du groupe de disponibilité. Il s’agit d’un nom spécifié par l’utilisateur qui doit être unique au sein du cluster de cluster de basculement Windows Server (WSFC).|  
|**ag_id**|**uniqueidentifier**|Identificateur unique (GUID) du groupe de disponibilité.|  
|**ag_resource_id**|**nvarchar (256)**|ID unique du groupe de disponibilité en tant que ressource dans le cluster WSFC.|  
|**ag_group_id**|**nvarchar (256)**|Identificateur unique du groupe WSFC du groupe de disponibilité.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

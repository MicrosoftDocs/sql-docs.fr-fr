---
description: sys.dm_hadr_ag_threads (Transact-SQL)
title: sys.dm_hadr_ag_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_ag_threads_TSQL
- sys.dm_hadr_ag_threads
- dm_hadr_ag_threads_TSQL
- dm_hadr_ag_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_ag_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: c8487dd07e424b7edccc17b8a9587cfc91f74268
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100354135"
---
# <a name="sysdm_hadr_ag_threads-transact-sql"></a>sys.dm_hadr_ag_threads (Transact-SQL)

Les DMV de télémétrie de thread HADR (**sys.dm_hadr_ag_threads** et [sys.dm_hadr_db_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-db-threads-transact-sql.md)) permettent aux utilisateurs d’obtenir rapidement une vue d’ensemble de l’utilisation des threads par groupe de disponibilité et par base de données à haute disponibilité. La compréhension de l’utilisation de ce thread est un point de référence important pour le paramétrage des groupes de disponibilité.

Cette vue de gestion dynamique (DMV) indique l’utilisation des threads au niveau du groupe de disponibilité.

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificateur du groupe de disponibilité.|
|**name**|**nvarchar(128)**|Nom du groupe de disponibilité.|
|**num_databases**|**int**|Nombre de bases de données dans le groupe de disponibilité.|
|**num_capture_threads**|**int**|Nombre de threads de capture de journaux utilisés par toutes les bases de données de ce groupe de disponibilité.|
|**num_redo_threads**|**int**|Nombre de threads de restauration par progression utilisés par toutes les bases de données de ce groupe de disponibilité.|
|**num_parallel_redo_threads**|**int**|Nombre de threads de restauration par progression parallèles utilisés par toutes les bases de données de ce groupe de disponibilité.|
|**num_hadr_threads**|**int**|Nombre de tous les threads Always on utilisés par toutes les bases de données de ce groupe de disponibilité.|

## <a name="permissions"></a>Autorisations  

 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  

 [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
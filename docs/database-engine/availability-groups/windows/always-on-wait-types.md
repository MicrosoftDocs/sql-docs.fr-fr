---
title: Identifier les attentes associées aux groupes de disponibilité
description: Identifier les attentes associées aux groupes de disponibilité Always On à l’aide de Transact-SQL (T-SQL) et d’événements étendus.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
author: rothja
ms.author: jroth
ms.openlocfilehash: ae57df2dfb3aa43f22ebed392b9ac35fd46c5d33
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641402"
---
# <a name="identify-waits-associated-with-availability-groups"></a>Identifier les attentes associées aux groupes de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Quand vous tentez de résoudre les problèmes de latence des groupes de disponibilité Always On, vous pouvez monitorer l’accumulation des statistiques d’attente à l’aide des types d’attentes spécifiques aux groupes de disponibilité dans la DMV (vue de gestion dynamique) [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
 Pour obtenir des informations générales sur l’utilisation des statistiques d’attente, consultez [Attentes et files d’attente dans SQL Server 2005](/previous-versions/sql/sql-server-2005/administrator/cc966413(v=technet.10)). Ce document a été écrit pour SQL Server 2005, mais les informations qu’il contient s’appliquent aux versions ultérieures de SQL Server.  
  
## <a name="query-for-availability-groups-wait-types"></a>Requête permettant d’obtenir les types d’attentes des groupes de disponibilité  
 Utilisez la requête T-SQL ci-dessous pour récupérer toutes les statistiques d’attente avec les types d’attentes des groupes de disponibilité :  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 Pour monitorer les statistiques d’attente en capturant des événements étendus, utilisez la commande T-SQL suivante.  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 Pour afficher le mappage clé-valeur du type d’attente, exécutez la requête suivante :  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Types d’attentes](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  

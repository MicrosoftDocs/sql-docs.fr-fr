---
description: Tables d’événements étendus - trace_xe_event_map
title: trace_xe_event_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- trace_xe_event_map_TSQL
- trace_xe_event_map
dev_langs:
- TSQL
helpviewer_keywords:
- trace_xe_event_map
- extended events [SQL Server], tables
ms.assetid: 537aa292-3540-47e8-be28-56dc01abc343
author: cawrites
ms.author: chadam
ms.openlocfilehash: e1cbee9046bb00b504288779748a1befb4e3d2b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093721"
---
# <a name="extended-events-tables---trace_xe_event_map"></a>Tables d’événements étendus - trace_xe_event_map
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque événement Événements étendus mappé à une classe d'événements Trace SQL. Cette table est stockée dans la base de données Master, dans le schéma sys.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|trace_event_id|**smallint**|ID de la classe d'événement Trace SQL qui est mappée.|  
|package_name|**nvarchar(60)**|Nom du package Événements étendus où réside l'événement mappé.|  
|xe_event_name|**nvarchar(60)**|Nom de l'événement Événements étendus mappé à la classe d'événements Trace SQL.|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser la requête suivante pour identifier les événements Événements étendus qui sont équivalents aux classes d'événements Trace SQL :  
  
```  
SELECT te.name, xe.package_name, xe.xe_event_name  
FROM sys.trace_events AS te  
LEFT JOIN sys.trace_xe_event_map AS xe  
   ON te.trace_event_id = xe.trace_event_id  
WHERE xe.trace_event_id IS NOT NULL  
```  
  
 Les classes d'événements n'ont pas toutes des événements Événements étendus équivalents. Vous pouvez utiliser la requête suivante pour répertorier les classes d'événements qui n'ont pas d'équivalent Événements étendus :  
  
```  
SELECT te.trace_event_id, te.name  
FROM sys.trace_events AS te  
LEFT JOIN sys.trace_xe_event_map AS xe  
   ON te.trace_event_id = xe.trace_event_id  
WHERE xe.trace_event_id IS NULL  
```  
  
 Dans la requête précédente, la plupart des classes d'événements retournées se rapportent à l'audit. Nous vous recommandons d'utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit à des fins d'audit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit utilise Événements étendus pour faciliter la création d'un audit. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>Voir aussi  
 [trace_xe_action_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-action-map.md)  
  
  

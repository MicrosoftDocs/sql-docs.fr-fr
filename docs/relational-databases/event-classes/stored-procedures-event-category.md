---
description: Catégorie d'événements Procédures stockées
title: Procédures stockées, catégorie d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Stored Procedures event category [SQL Server]
- SQL Server event classes, Stored Procedures event category
- event classes [SQL Server], Stored Procedures event category
ms.assetid: 71bebaa3-a05a-4695-b349-078cecd0949a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 000fab845e06d28d2671032ae032bdae10881b24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161441"
---
# <a name="stored-procedures-event-category"></a>Catégorie d'événements Procédures stockées
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La catégorie d’événements **Procédures stockées** contient des événements de procédure stockée généraux.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Classe d'événements RPC:Completed](../../relational-databases/event-classes/rpc-completed-event-class.md)|Indique qu'un appel de procédure distante (RPC) s'est terminé.|  
|[PreConnect:Completed, classe d’événements](../../relational-databases/event-classes/preconnect-completed-event-class.md)|Indique quand la fonction classifieur de Resource Governor termine de s'exécuter.|  
|[PreConnect:Starting, classe d'événements](../../relational-databases/event-classes/preconnect-starting-event-class.md)|Indique quand la fonction classifieur de Resource Governor commence à s'exécuter.|  
|[Classe d'événements RPC Output Parameter](../../relational-databases/event-classes/rpc-output-parameter-event-class.md)|Trace les valeurs de paramètres de sortie des appels de procédure distante après l'exécution.|  
|[Classe d'événements RPC:Starting](../../relational-databases/event-classes/rpc-starting-event-class.md)|Indique le démarrage d'un appel de procédure distante.|  
|[Classe d'événements SP:CacheHit](../../relational-databases/event-classes/sp-cachehit-event-class.md)|Indique que la procédure stockée se trouve dans le cache.|  
|[Classe d'événements SP:CacheInsert](../../relational-databases/event-classes/sp-cacheinsert-event-class.md)|Indique que la procédure stockée a été placée dans le cache.|  
|[Classe d'événements SP:CacheMiss](../../relational-databases/event-classes/sp-cachemiss-event-class.md)|Indique que la procédure stockée est introuvable dans le cache.|  
|[Classe d'événements SP:CacheRemove](../../relational-databases/event-classes/sp-cacheremove-event-class.md)|Indique que la procédure stockée a été supprimée du cache.|  
|[Classe d'événements SP:Completed](../../relational-databases/event-classes/sp-completed-event-class.md)|Indique que l'exécution de la procédure stockée s'est terminée.|  
|[Classe d'événements SP:Recompile](../../relational-databases/event-classes/sp-recompile-event-class.md)|Indique que la procédure stockée a été recompilée.|  
|[Classe d'événements SP:Starting](../../relational-databases/event-classes/sp-starting-event-class.md)|Indique que l'exécution de la procédure stockée démarre.|  
|[Classe d'événements SP:StmtCompleted](../../relational-databases/event-classes/sp-stmtcompleted-event-class.md)|Indique qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] d'une procédure stockée s'est terminée.|  
|[SP:StmtStarting, classe d’événements](../../relational-databases/event-classes/sp-stmtstarting-event-class.md)|Indique qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] d'une procédure stockée a commencé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  

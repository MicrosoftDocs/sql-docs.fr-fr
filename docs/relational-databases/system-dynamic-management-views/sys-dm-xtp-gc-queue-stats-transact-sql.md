---
description: sys.dm_xtp_gc_queue_stats (Transact-SQL)
title: sys.dm_xtp_gc_queue_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xtp_gc_stats
- dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_gc_stats dynamic management view
ms.assetid: addef774-318d-46a7-85df-f93168a800cb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 98d27fff17b897c7dfaba34b7afd2e21181a5cef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096436"
---
# <a name="sysdm_xtp_gc_queue_stats-transact-sql"></a>sys.dm_xtp_gc_queue_stats (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Génère des informations sur chaque file d'attente de travaux de garbage collection sur le serveur, ainsi que diverses statistiques sur chacune d'entre elles. Il existe une file d'attente par processeur logique.  
  
 Le thread garbage collection principal (le thread inactif) trace les lignes mises à jour, supprimées et insérées de toutes les transactions terminées depuis la dernière invocation du thread de garbage collection principal. Lorsque le thread de garbage collection s'exécute, il détermine si l'horodateur de la transaction active la plus ancienne a changé. Si la transaction active la plus ancienne a changé, alors le thread inactif empile les éléments de travail (par segments de 16 lignes) pour les transactions dont les jeux d'écritures ne sont plus requis. Par exemple, si vous supprimez 1 024 lignes, vous verrez 64 éléments de travail de garbage collection en file d'attente, chacun contenant 16 lignes supprimées.  Après qu'une transaction utilisateur est validée, elle sélectionne tous les éléments empilés de son planificateur. S'il n'y a pas d'éléments empilés sur le planificateur, elle recherche des éléments sur toutes les files d'attente dans le nœud NUMA.  
  
 Vous pouvez déterminer si le garbage collection libère de la mémoire pour les lignes supprimées en exécutant sys.dm_xtp_gc_queue_stats pour voir si le travail empilé est traité. Si les entrées du current_queue_depth ne sont pas traitées ou si aucun nouvel élément de travail n’est ajouté au current_queue_depth, cela indique que garbage collection ne libère pas de mémoire. Par exemple, garbage collection ne peut pas être effectué s’il existe une transaction longue.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  

|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|queue_id|**int**|Identificateur unique de la file d'attente.|  
|total_enqueues|**bigint**|Nombre total d'éléments de travail garbage collection empilés dans cette file d'attente depuis que le serveur a démarré.|  
|total_dequeues|**bigint**|Nombre total d'éléments de travail garbage collection dépilés de cette file d'attente depuis que le serveur a démarré.|  
|current_queue_depth|**bigint**|Nombre actuel d'éléments de travail garbage collection présents dans la file d'attente. Cet élément peut impliquer un ou plusieurs éléments récupérés par le garbage collector.|  
|maximum_queue_depth|**bigint**|Profondeur maximale de cette file d'attente.|  
|last_service_ticks|**bigint**|Cycles de l'UC au moment où la file d'attente a été traitée pour la dernière fois.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="user-scenario"></a>Scénario d'utilisateur  
 Ce résultat montre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est exécuté sur 4 cœurs ou que l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possède une affinité sur 4 cœurs :  
  
 Ce résultat montre qu'il n'existe aucun élément de travail à traiter dans les files d'attente. Pour la file d’attente 0, le nombre total d’éléments de travail mis en file d’attente depuis le démarrage SQL est 15625 et la profondeur de file d’attente maximale est de 15625.  
  
```  
queue_id total_enqueues total_dequeues current_queue_depth  maximum_queue_depth  last_service_ticks  
----------------------------------------------------------------------------------------------------  
0        15625                15625    0                    15625                1233573168347  
1        15625                15625    0                    15625                1234123295566  
2        15625                15625    0                    15625                1233569418146  
3        15625                15625    0                    15625                1233571605761  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  

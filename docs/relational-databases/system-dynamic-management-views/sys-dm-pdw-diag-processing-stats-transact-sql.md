---
description: sys.dm_pdw_diag_processing_stats (Transact-SQL)
title: sys.dm_pdw_diag_processing_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: df659c55-4f63-45f8-8afe-ce300031bc5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 0164a7f806afcacc35af0e2bffd2c6cbe69ee762
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144245"
---
# <a name="sysdm_pdw_diag_processing_stats-transact-sql"></a>sys.dm_pdw_diag_processing_stats (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Affiche des informations relatives à tous les événements de diagnostic internes qui peuvent être incorporés dans les sessions de diagnostic définies par l’administrateur. Interrogez cette vue pour comprendre les statistiques sous-jacentes aux sous-systèmes de diagnostic et d’événement qui pilotent le remplissage de toutes les autres vues DMV. Il existe un groupe de files d’attente pour chaque processus sur chaque nœud.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|Nœud d’appareil dont ce journal provient.|  
|**process_id**|**int**|Identificateur du processus en cours d’exécution de cette statistique.|  
|**target_name**|**nvarchar(255)**|Nom de la file d'attente.|  
|**queue_size**|**int**|Nombre d’éléments dans la file d’attente de traitement. La taille de la file d’attente est généralement 0. Un nombre positif indique que le système est soumis à une contrainte et qu’il génère des travaux en souffrance des événements. Un nombre positif dans les autres colonnes signifie que le système est endommagé pour cette file d’attente et toutes les vues DMV associées.|  
|**lost_events_count**|**bigint**|Nombre d’événements perdus.|  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

---
description: sys.dm_pdw_wait_stats (Transact-SQL)
title: sys.dm_pdw_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: cfb8d905-c34f-44de-9574-dde81e170916
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: e3b8bdac31ca372506a23e852b0f73d5ec643b3a
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035192"
---
# <a name="sysdm_pdw_wait_stats-transact-sql"></a>sys.dm_pdw_wait_stats (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations relatives à l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] État du système d’exploitation associé aux instances en cours d’exécution sur les différents nœuds. Pour obtenir la liste des types d’attente et leur description, consultez [sys.dm_os_wait_stats](https://msdn.microsoft.com/library/ms179984\(v=sql.120\).aspx).  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|**pdw_node_id**|**int**|ID du nœud auquel cette entrée fait référence.||  
|**wait_name**|**nvarchar(255)**|Nom du type d'attente.||  
|**max_wait_time**|**bigint**|Durée d’attente maximale de ce type d’attente.||  
|**request_count**|**bigint**|Nombre d’attentes de ce type d’attente en suspens.||  
|**signal_time**|**bigint**|Différence entre le moment où le thread qui attend a été signalé et le moment où il a commencé à s'exécuter.||  
|**completed_count**|**bigint**|Nombre total d’attentes de ce type terminées depuis le dernier redémarrage du serveur.||  
|**wait_time**|**bigint**|Temps d’attente total pour ce type d’attente dans millisecons. Signal_time inclus.||  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [sys.dm_pdw_waits &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql.md)  
  
  

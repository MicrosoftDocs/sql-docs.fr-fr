---
description: sys.dm_pdw_lock_waits (Transact-SQL)
title: sys.dm_pdw_lock_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8ef966f8-d14e-40d3-9626-3508ada9b8fb
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9704cdc31250760b3e8946c713e3fdc40504a7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142427"
---
# <a name="sysdm_pdw_lock_waits-transact-sql"></a>sys.dm_pdw_lock_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur les demandes en attente de verrous.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Position de la demande dans la liste d’attente.|ordinal de base 0. Cela n’est pas unique pour toutes les entrées d’attente.|  
|session_id|**nvarchar(32)**|ID de la session dans laquelle l’état d’attente s’est produit.|Consultez session_id dans [sys.dm_pdw_exec_sessions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|type|**nvarchar(255)**|Type d’attente représenté par cette entrée.|Valeurs possibles :<br /><br /> Partagé<br /><br /> SharedUpdate<br /><br /> ExclusiveUpdate<br /><br /> Exclusif|  
|object_type|**nvarchar(255)**|Type d’objet affecté par l’attente.|Valeurs possibles :<br /><br /> OBJECT<br /><br /> DATABASE<br /><br /> SYSTEM<br /><br /> SCHEMA<br /><br /> APPLICATION|  
|object_name|**nvarchar(386**|Nom ou GUID de l’objet spécifié qui a été affecté par l’attente.|Les tables et les vues sont affichées avec des noms en trois parties.<br /><br /> Les index et les statistiques sont affichés avec des noms en quatre parties.<br /><br /> Les noms, les principaux et les bases de données sont des noms de chaîne.|  
|request_id|**nvarchar(32)**|ID de la demande sur laquelle l’état d’attente s’est produit.|ID de la demande.<br /><br /> Il s’agit d’un GUID pour les demandes de chargement.|  
|request_time|**datetime**|Heure à laquelle le verrou ou la ressource a été demandé.||  
|acquire_time|**datetime**|Heure à laquelle le verrou ou la ressource a été acquis (e).||  
|state|**nvarchar(50)**|État de l’état d’attente.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorité de l’élément en attente.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

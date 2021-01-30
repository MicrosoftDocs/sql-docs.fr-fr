---
description: sys.dm_pdw_resource_waits (Transact-SQL)
title: sys.dm_pdw_resource_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/26/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: f6685853de3fed6397886d654135bb6ce499e7da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203400"
---
# <a name="sysdm_pdw_resource_waits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Affiche des informations d’attente pour tous les types de ressources dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Position de la demande dans la liste d’attente.|ordinal de base 0. Cela n’est pas unique pour toutes les entrées d’attente.|  
|session_id|**nvarchar(32)**|ID de la session dans laquelle l’état d’attente s’est produit.|Consultez session_id dans [sys.dm_pdw_exec_sessions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|type|**nvarchar(255)**|Type d’attente représenté par cette entrée.|Valeurs possibles :<br /><br /> Connexion<br /><br /> Concurrence des requêtes locales<br /><br /> Concurrence des requêtes distribuées<br /><br /> Concurrence DMS<br /><br /> Concurrence de sauvegarde|  
|object_type|**nvarchar(255)**|Type d’objet affecté par l’attente.|Valeurs possibles :<br /><br /> **DESSIN**<br /><br /> **DATABASE**<br /><br /> **REQUISE**<br /><br /> **SCHEMA**<br /><br /> **OEUVRE**|  
|object_name|**nvarchar(386**|Nom ou GUID de l’objet spécifié qui a été affecté par l’attente.|Les tables et les vues sont affichées avec des noms en trois parties.<br /><br /> Les index et les statistiques sont affichés avec des noms en quatre parties.<br /><br /> Les noms, les principaux et les bases de données sont des noms de chaîne.|  
|request_id|**nvarchar(32)**|ID de la demande sur laquelle l’état d’attente s’est produit.|Identificateur QID de la demande.<br /><br /> Identificateur GUID pour les demandes de chargement.|  
|request_time|**datetime**|Heure à laquelle le verrou ou la ressource a été demandé.||  
|acquire_time|**datetime**|Heure à laquelle le verrou ou la ressource a été acquis (e).||  
|state|**nvarchar(50)**|État de l’état d’attente.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorité de l’élément en attente.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|Interne|Voir les [attentes des ressources de surveillance](#monitor-resource-waits) ci-dessous|  
|resource_class|**nvarchar(20**|Interne |Voir les [attentes des ressources de surveillance](#monitor-resource-waits) ci-dessous|  
  
## <a name="monitor-resource-waits"></a>Surveiller les attentes des ressources 
Avec l’introduction des [groupes de charges de travail](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation), les emplacements de concurrence ne sont plus applicables.  Utilisez la requête ci-dessous et la `resources_requested` colonne pour comprendre les ressources nécessaires à l’exécution de la demande.

```sql
select rw.wait_id
      ,rw.session_id
      ,rw.type
      ,rw.object_type
      ,rw.object_name
      ,rw.request_id
      ,rw.request_time
      ,rw.acquire_time
      ,rw.state
      ,resources_requested = s.effective_request_min_resource_grant_percent
      ,r.group_name
  from sys.dm_workload_management_workload_groups_stats s
  join sys.dm_pdw_exec_requests r on r.group_name = s.name collate SQL_Latin1_General_CP1_CI_AS
  join sys.dm_pdw_resource_waits rw on rw.request_id = r.request_id
```

## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  

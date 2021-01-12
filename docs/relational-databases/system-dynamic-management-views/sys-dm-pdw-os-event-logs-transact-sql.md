---
description: sys.dm_pdw_os_event_logs (Transact-SQL)
title: sys.dm_pdw_os_event_logs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a0daa8cf-72e2-4349-8be1-d3cc0f9b1e02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: af5633fc5e1a169c71197ac77a715e4b28da2b57
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092693"
---
# <a name="sysdm_pdw_os_event_logs-transact-sql"></a>sys.dm_pdw_os_event_logs (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contient des informations sur les différents journaux des événements Windows sur les différents nœuds.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Nœud d’appareil dont ce journal provient.<br /><br /> pdw_node_id et log_name forment la clé de cette vue.||  
|log_name|**nvarchar(255)**|Nom du journal des événements Windows.<br /><br /> pdw_node_id et log_name forment la clé de cette vue.||  
|log_source|**nvarchar(255)**|Nom de la source du journal des événements Windows.||  
|event_id|**int**|ID de l’événement. Non unique.||  
|event_type|**nvarchar(255)**|Type de l’événement, identifiant la gravité.|« Information », « Warning », « Error »|  
|event_message|**nvarchar(4000)**|Détails de l’événement.||  
|generate_time|**datetime**|Heure de création de l’événement.||  
|write_time|**datetime**|Heure à laquelle l’événement a été écrit dans le journal.||  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section métadonnées dans la rubrique [limites de capacité](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) . 
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

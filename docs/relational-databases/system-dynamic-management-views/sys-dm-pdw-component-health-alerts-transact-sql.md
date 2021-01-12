---
description: sys.dm_pdw_component_health_alerts (Transact-SQL)
title: sys.dm_pdw_component_health_alerts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 88f05392-1e97-4693-ba60-a4910af3c000
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 83a3a237da01eb677ef8282940b334fa28b42637
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093973"
---
# <a name="sysdm_pdw_component_health_alerts-transact-sql"></a>sys.dm_pdw_component_health_alerts (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Stocke les alertes précédemment émises sur les composants de l’appliance.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Identificateur unique d’un [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] nœud.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id et alert_instance_id constituent la clé de cette vue.|NOT NULL|  
|component_id|**int**|ID du composant. Consultez [sys.pdw_health_components &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id et alert_instance_id constituent la clé de cette vue.|NOT NULL|  
|component_instance_id|**nvarchar(255)**|pdw_node_id, component_id, component_instance_id, alert_id et alert_instance_id constituent la clé de cette vue.|NOT NULL|  
|alert_id|**int**|ID du type d’alerte. Consultez [sys.pdw_health_alerts &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id et alert_instance_id constituent la clé de cette vue.|NOT NULL|  
|alert_instance_id|**nvarchar (36)**|Identifie une instance d’une alerte donnée.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id et alert_instance_id constituent la clé de cette vue.|NOT NULL|  
|previous_value|**nvarchar(255)**|Utilisé lorsque l’alerte est de type StatusChange. Il s’agit de l’état du composant précédent. La valeur est NULL pour les alertes de type Threshold. Pour obtenir la liste des types d’alerte, consultez [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|current_value|**nvarchar(255)**|Utilisé lorsque l’alerte est de type StatusChange. Il s’agit de l’état actuel du composant. La valeur est NULL pour les alertes de type Threshold. Pour obtenir la liste des types d’alerte, consultez [sys.pdw_health_alerts &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|create_time|**datetime**|Heure et date de génération de l’alerte.|NOT NULL|  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

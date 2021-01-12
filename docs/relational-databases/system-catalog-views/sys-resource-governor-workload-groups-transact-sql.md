---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83d699e2da5e1de1e25a2618171924843c569188
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101751"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la configuration de groupe de charge de travail stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chaque groupe de charges de travail peut s'abonner à un seul et unique pool de ressources.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|group_id|**int**|ID unique du groupe de charges de travail. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom du groupe de charges de travail. N'accepte pas la valeur NULL.|  
|importance|**sysname**|**Remarque :** L’importance s’applique uniquement aux groupes de charge de travail dans le même pool de ressources.<br /><br /> Importance relative d'une demande dans ce groupe de charges de travail. L’importance est l’un des éléments suivants, avec la valeur par défaut moyenne : faible, moyenne, élevée.<br /><br /> N'accepte pas la valeur NULL.|  
|request_max_memory_grant_percent|**int**|Allocation de mémoire maximale, en pourcentage, pour une demande unique. La valeur par défaut est 25. N'accepte pas la valeur NULL.<br /><br /> **Remarque :** Si ce paramètre est supérieur à 50 pour cent, les requêtes volumineuses s’exécuteront une par une. Par conséquent, le risque de rencontrer une erreur de mémoire insuffisante est plus important pendant l'exécution de la demande.|  
|request_max_cpu_time_sec|**int**|Limite maximale d'utilisation de l'UC, en secondes, pour une demande unique. La valeur par défaut 0 spécifie qu'il n'y a pas de limite. N'accepte pas la valeur NULL.<br /><br /> **Remarque :** Pour plus d’informations, consultez [classe d’événements CPU Threshold Exceeded](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|request_memory_grant_timeout_sec|**int**|Délai d'allocation de mémoire, en secondes, pour une demande unique. La valeur par défaut 0 utilise un calcul interne basé sur le coût de requête. N'accepte pas la valeur NULL.|  
|max_dop|**int**|Degré maximal de parallélisme pour le groupe de charges de travail. La valeur par défaut 0 utilise des paramètres globaux. N'accepte pas la valeur NULL.<br /><br /> **Nœud :** Ce paramètre remplace l’option de requête **MAXDOP**.|  
|group_max_requests|**int**|Nombre maximal de demandes simultanées. La valeur par défaut 0 spécifie qu'il n'y a pas de limite. N'accepte pas la valeur NULL.|  
|pool_id|**int**|ID du pool de ressources utilisé par ce groupe de charges de travail.|  
|external_pool_id|**int**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures.<br /><br /> ID du pool de ressources externe utilisé par ce groupe de charges de travail.|  
  
## <a name="remarks"></a>Notes  
 L'affichage catalogue affiche les métadonnées stockées. Pour afficher la configuration en mémoire, utilisez la vue de gestion dynamique correspondante, [sys.dm_resource_governor_workload_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 La configuration stockée et en mémoire peut être différente si la configuration du gouverneur de ressources a été modifiée mais que l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE n'a pas été appliquée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW ANY DEFINITION pour consulter le contenu, requiert l'autorisation CONTROL SERVER pour modifier le contenu.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  

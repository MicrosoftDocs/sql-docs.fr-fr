---
description: sys.resource_governor_resource_pools (Transact-SQL)
title: sys.resource_governor_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fef5cb969c9dd0d3a0253362a2e3739e490db916
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203770"
---
# <a name="sysresource_governor_resource_pools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la configuration de pool de ressources stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chaque ligne de la vue détermine la configuration d'un pool.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|ID unique du pool de ressources. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom du pool de ressources. N'accepte pas la valeur NULL.|  
|min_cpu_percent|**int**|Bande passante processeur moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|max_cpu_percent|**int**|Bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|min_memory_percent|**int**|Quantité de mémoire garantie pour toutes les demandes dans le pool de ressources. Cette valeur n'est pas partagée avec d'autres pools de ressources. N'accepte pas la valeur NULL.|  
|max_memory_percent|**int**|Pourcentage de la mémoire totale du serveur qui peut être utilisé par les demandes dans ce pool de ressources. N'accepte pas la valeur NULL. La valeur maximale effective dépend des valeurs minimales du pool. Par exemple, max_memory_percent peut avoir la valeur 100, alors que sa valeur maximale effective est inférieure.|  
|cap_cpu_percent|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront. Limite la bande passante maximale de l'UC au niveau spécifié. La plage autorisée pour la valeur est comprise entre 1 et 100.|  
|min_iops_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Opérations d'E/S minimales par seconde (IOPS) par paramètre de volume de ce pool. 0 = aucune réservation. Ne peut pas être null.|  
|max_iops_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Opérations d'E/S maximales par seconde (IOPS) par paramètre de volume de ce pool. 0 = illimitées. Ne peut pas être null.|  
  
## <a name="remarks"></a>Notes  
 L'affichage catalogue affiche les métadonnées stockées. Pour afficher la configuration en mémoire, utilisez la vue de gestion dynamique correspondante, [sys.dm_resource_governor_resource_pools &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW ANY DEFINITION pour consulter le contenu, requiert l'autorisation CONTROL SERVER pour modifier le contenu.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  

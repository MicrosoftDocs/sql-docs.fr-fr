---
description: sys.dm_os_memory_brokers (Transact-SQL)
title: sys.dm_os_memory_brokers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_brokers
- dm_os_memory_brokers_TSQL
- sys.dm_os_memory_brokers_TSQL
- dm_os_memory_brokers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_brokers dynamic management view
ms.assetid: 48dd6ad9-0d36-4370-8a12-4921d0df4b86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6b32eed535ad35e8755343c40542acd57b0f5247
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184883"
---
# <a name="sysdm_os_memory_brokers-transact-sql"></a>sys.dm_os_memory_brokers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les allocations qui sont internes à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le suivi de la différence entre les compteurs de mémoire de processus **sys.dm_os_process_memory** et les compteurs internes peut indiquer l’utilisation de la mémoire par des composants externes dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espace mémoire.  
  
 Les gestionnaires d'allocation mémoire répartissent équitablement les allocations de mémoire entre les différents composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en fonction de l'utilisation actuelle et prévue. Ils n'effectuent pas d'allocations. Ils n'effectuent que le suivi des allocations pour calculer la distribution.  
  
 Le tableau suivant fournit des informations sur les gestionnaires d'allocation mémoire.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_memory_brokers**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|ID du pool de ressources s'il est associé à un pool du gouverneur de ressources.|  
|**memory_broker_type**|**nvarchar(60)**|Type de gestionnaire d'allocation mémoire. Il existe actuellement trois types de courtiers de mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , répertoriés ci-dessous avec leurs descriptions.<br /><br /> **MEMORYBROKER_FOR_CACHE** : mémoire allouée pour être utilisée par les objets mis en cache (et non par le cache du pool de mémoires tampons).<br /><br /> **MEMORYBROKER_FOR_STEAL** : mémoire volée à partir du pool de mémoires tampons. Cette mémoire ne peut pas être réutilisée par d'autres composants tant qu'elle n'est pas libérée par le propriétaire actuel.<br /><br /> **MEMORYBROKER_FOR_RESERVE** : mémoire réservée pour une utilisation future par les demandes en cours d’exécution.|  
|**allocations_kb**|**bigint**|Quantité de mémoire, en kilo-octets (Ko), allouée à ce type de gestionnaire d'allocation mémoire.|  
|**allocations_kb_per_sec**|**bigint**|Taux d'allocations de mémoire, en kilo-octets (Ko) par seconde. Cette valeur peut être négative pour les désallocations de mémoire.|  
|**predicted_allocations_kb**|**bigint**|Quantité prédite de mémoire allouée par le gestionnaire d'allocation mémoire. Cette valeur est basée sur le modèle d'utilisation de la mémoire.|  
|**target_allocations_kb**|**bigint**|Quantité recommandée de mémoire allouée, en kilo-octet (Ko), basée sur les paramètres actuels et le modèle d'utilisation de la mémoire. Ce gestionnaire d'allocation mémoire doit augmenter ou diminuer la quantité pour obtenir cette valeur.|  
|**future_allocations_kb**|**bigint**|Nombre projeté d'allocations, en kilo-octet (Ko), qui seront effectuées dans les prochaines secondes.|  
|**overall_limit_kb**|**bigint**|Quantité maximale de mémoire, en kilo-octets (Ko), que le répartiteur peut allouer.|  
|**last_notification**|**nvarchar(60)**|Recommandation relative à l'utilisation de la mémoire basée sur les paramètres actuels et le modèle d'utilisation. Les valeurs valides sont les suivantes :<br /><br /> grow<br /><br /> shrink<br /><br /> stable|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="see-also"></a>Voir aussi  

  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  



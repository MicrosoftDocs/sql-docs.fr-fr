---
description: sys.availability_group_listeners (Transact-SQL)
title: sys.availability_group_listeners (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_group_listeners_TSQL
- sys.availability_group_listeners
- sys.availability_group_listeners_TSQL
- availability_group_listeners
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_group_listeners catalog view
- Availability Groups [SQL Server], listeners
ms.assetid: b5e7d1fb-3ffb-4767-8135-604c575016b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fbc07c0323f8efb1032555ba83d7758ef99bc6d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338935"
---
# <a name="sysavailability_group_listeners-transact-sql"></a>sys.availability_group_listeners (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Pour chaque groupe de disponibilité Always On, retourne aucune ligne, ce qui indique qu'aucun nom réseau n'est associé au groupe de disponibilité, ou retourne une ligne pour chaque configuration d'écouteur de groupe de disponibilité dans le cluster de clustering de basculement Windows Server (WSFC). Cette vue affiche la configuration en temps réel obtenue à partir du cluster.  
  
> [!NOTE]  
>  Cet affichage catalogue ne décrit pas les détails d'une configuration IP, qui a été définie dans le cluster WSFC.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|ID de groupe de disponibilité (**group_id**) à partir de [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md).|  
|**listener_id**|**nvarchar (36)**|GUID à partir de l'ID de ressource de cluster.|  
|**dns_name**|**nvarchar (63)**|Nom réseau configuré (nom d'hôte) de l'écouteur du groupe de disponibilité.|  
|**port**|**int**|Numéro de port TCP configuré pour l'écouteur du groupe de disponibilité.<br /><br /> NULL = L'écouteur a été configuré en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et son numéro de port n'a pas été ajouté au groupe de disponibilité. Pour ajouter le port, pleaseuse l’option modifier l’ÉCOUTEur de l’instruction [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**is_conformant**|**bit**|Indique si cette configuration IP est conforme. Peut prendre une des valeurs suivantes :<br /><br /> 1 = L'écouteur est conforme. Seules les relations « ou » existent entre les adresses IP (Internet Protocol). *Conforme* englobe chaque configuration IP créée par l’instruction [Create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] . De plus, si une configuration IP créée en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple à l'aide du Gestionnaire de cluster de basculement WSFC, peut être modifiée par l'instruction TSQL ALTER AVAILABILITY GROUP, la configuration IP obtient la qualification conforme.<br /><br /> 0 = L'écouteur n'est pas conforme. En général, cela indique une adresse IP qui ne peut pas être configurée à l'aide des commandes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et, à la place, a été définie directement dans le cluster WSFC.|  
|**ip_configuration_string_from_cluster**|**nvarchar(max)**|Chaînes de configuration IP de cluster, le cas échéant, pour cet écouteur. NULL = L'écouteur n'a pas d'adresse IP virtuelle. Par exemple :<br /><br /> Adresse IPv4 : `65.55.39.10`.<br /><br /> Adresse IPv6 : `2001::4898:23:1002:20f:1fff:feff:b3a3`|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

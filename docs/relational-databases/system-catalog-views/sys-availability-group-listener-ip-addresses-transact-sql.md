---
description: sys.availability_group_listener_ip_addresses (Transact-SQL)
title: sys.availability_group_listener_ip_addresses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_group_listener_ip_addresses
- sys.availability_group_listener_ip_addresses
- availability_group_listener_ip_addresses_TSQL
- sys.availability_group_listener_ip_addresses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.availability_group_listener_ip_addresses catalog view
ms.assetid: e515fa6b-1354-4110-9b70-ab2e6164c992
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc6887ad63b56330eab09d20f0b6fa86de4d3d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341382"
---
# <a name="sysavailability_group_listener_ip_addresses-transact-sql"></a>sys.availability_group_listener_ip_addresses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque adresse IP associée à un écouteur du groupe de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC).  
  
 Clé primaire : **listener_id**  +  **ip_address**  +  **ip_sub_mask**  
  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**listener_id**|**nvarchar (36)**|GUID de ressource de cluster de clustering de basculement Windows Server (WSFC).|  
|**ip_address**|**nvarchar (48)**|Adresse IP virtuelle configurée de l'écouteur du groupe de disponibilité. Retourne une seule adresse IPv4 ou IPv6.|  
|**ip_subnet_mask**|**nvarchar(15**|Masque de sous-réseau IP configuré pour l'adresse IPv4, le cas échéant, qui est configurée pour l'écouteur du groupe de disponibilité.<br /><br /> NULL = Sous-réseau IPv6|  
|**is_dhcp**|**bit**|Indique si l'adresse IP est configurée par DHCP, une des valeurs suivantes :<br /><br /> 0 = L'adresse IP n'est pas configurée par DHCP.<br /><br /> 1 = L'adresse IP est configurée par DHCP|  
|**network_subnet_ip**|**nvarchar (48)**|Adresse IP de sous-réseau de réseau qui spécifie le sous-réseau auquel l'adresse IP appartient.|  
|**network_subnet_prefix_length**|**int**|Longueur de préfixe de sous-réseau de réseau du sous-réseau auquel l'adresse IP appartient.|  
|**network_subnet_ipv4_mask**|**nvarchar (45)**|Masque de sous-réseau de réseau du sous-réseau auquel l'adresse IP appartient. **network_subnet_ipv4_mask** pour spécifier les options du <dhcp network_subnet_option> dans une clause with DHCP de l’instruction [Create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) ou [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br /><br /> NULL = Sous-réseau IPv6|  
|**state**|**tinyint**|État de la ressource IP ONLINE/OFFLINE du cluster WSFC, un des suivants :<br /><br /> 1 = En ligne. La ressource IP est en ligne.<br /><br /> 0 = Hors connexion. La ressource IP est hors connexion.<br /><br /> 2 = En ligne en attente. La ressource IP est hors connexion mais est remise en ligne.<br /><br /> 3 = Échec. La ressource IP est remise en ligne mais un échec se produit.|  
|**state_desc**|**nvarchar(60)**|Description de l' **État**, parmi les suivants :<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> ONLINE_PENDING<br /><br /> FAILED|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  

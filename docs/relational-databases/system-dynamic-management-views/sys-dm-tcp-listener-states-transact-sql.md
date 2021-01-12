---
description: sys.dm_tcp_listener_states (Transact-SQL)
title: sys.dm_tcp_listener_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tcp_listener_states
- dm_tcp_listener_states
- sys.dm_tcp_listener_states_TSQL
- dm_tcp_listener_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.dm_tcp_listener_states dynamic management view
ms.assetid: 9997ffed-a4c1-428f-8bac-3b9e4b16d7cf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d3b155d84f499c4dd2ba8ed9333932364c7957ea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093959"
---
# <a name="sysdm_tcp_listener_states-transact-sql"></a>sys.dm_tcp_listener_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne contenant les informations dynamiques d'état pour chaque écouteur TCP.  
  
> [!NOTE]
> L'écouteur du groupe de disponibilité peut écouter le même port que l'écouteur de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans ce cas, les écouteurs sont répertoriées séparément, de la même façon que pour un écouteur Service Broker.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**listener_id**|**int**|ID interne de l’écouteur. N'accepte pas la valeur NULL.<br /><br /> Clé primaire|  
|**ip_address**|**nvarchar (48)**|Adresse IP d'écouteur en ligne et actuellement écoutée. Les adresses IPv4 et IPv6 sont autorisées. Si un écouteur possède les deux types d'adresses, ils sont répertoriés séparément. Un caractère générique IPv4, s’affiche sous la forme « 0.0.0.0 ». Un caractère générique IPv6, s’affiche sous la forme «  :: ».<br /><br /> N'accepte pas la valeur NULL.|  
|**is_ipv4**|**bit**|Type d'adresse IP<br /><br /> 1 = IPv4<br /><br /> 0 = IPv6|  
|**port**|**int**|Numéro de port sur lequel l'écouteur écoute. N'accepte pas la valeur NULL.|  
|**type**|**tinyint**|Type d'écouteur, un des suivants :<br /><br /> 0 = [!INCLUDE[tsql](../../includes/tsql-md.md)]<br /><br /> 1 = Service Broker<br /><br /> 2 = Mise en miroir de la base de données<br /><br /> N'accepte pas la valeur NULL.|  
|**type_desc**|**nvarchar(20**|Description du **type**, parmi :<br /><br /> TSQL<br /><br /> SERVICE_BROKER<br /><br /> DATABASE_MIRRORING<br /><br /> N'accepte pas la valeur NULL.|  
|**state**|**tinyint**|État de l'écouteur du groupe de disponibilité, un des suivants :<br /><br /> 1 = En ligne. L'écouteur écoute et traite les demandes.<br /><br /> 2 = Redémarrage en attente. L'écouteur est hors connexion, en attente d'un redémarrage.<br /><br /> Si l'écouteur du groupe de disponibilité écoute sur le même port que l'instance de serveur, ces deux écouteurs ont toujours le même état.<br /><br /> N'accepte pas la valeur NULL.<br /><br /> Remarque : les valeurs de cette colonne proviennent de l’objet TSD_listener. La colonne ne prend pas en charge un état hors connexion, car lorsque TDS_listener est hors connexion, elle ne peut pas être interrogée pour obtenir l'état.|  
|**state_desc**|**nvarchar (16)**|Description de l' **État**, parmi les suivants :<br /><br /> ONLINE<br /><br /> PENDING_RESTART<br /><br /> N'accepte pas la valeur NULL.|  
|**heure-début**|**datetime**|Horodateur qui indique quand l'écouteur a été démarré. N'accepte pas la valeur NULL.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Vues et fonctions de gestion dynamiques de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  

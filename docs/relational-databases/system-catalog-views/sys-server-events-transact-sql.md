---
description: sys.server_events (Transact-SQL)
title: sys.server_events (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_events_TSQL
- sys.server_events_TSQL
- server_events
- sys.server_events
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_events catalog view
ms.assetid: 996f6c9b-6426-4847-95d9-6b77541422be
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 350da163f64c09276cd57ebbf6c2c63b4e2e022c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188049"
---
# <a name="sysserver_events-transact-sql"></a>sys.server_events (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne par événement pour lequel est activé un déclencheur DDL de niveau serveur ou une notification d'événement de niveau serveur. Les colonnes **object_id** et **type** identifient de manière unique l’événement de serveur.  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la notification d'événements de niveau serveur ou du déclencheur DDL de niveau serveur à activer.|  
|**type**|**int**|Type de l'événement à l'origine de l'activation de la notification d'événement ou du déclencheur DDL.|  
|**type_desc**|**nvarchar(60)**|Description de l'événement à l'origine de l'activation du déclencheur DDL ou de la notification d'événement.|  
|**event_group_type**|**int**|Groupe d'événements sur lequel le déclencheur ou la notification d'événements est créé(e), ou null si le déclencheur n'est pas créé sur un groupe d'événements.|  
|**event_group_type_desc**|**nvarchar(60)**|Description du groupe d’événements sur lequel le déclencheur ou la notification d’événements est créé, ou null s’il n’est pas créé sur un groupe d’événements|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  

---
description: sys.server_event_session_actions (Transact-SQL)
title: sys.server_event_session_actions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.server_event_session_actions
- server_event_session_actions_TSQL
- server_event_session_actions
- sys.server_event_session_actions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_session_actions catalog view
- xe
ms.assetid: 1d8c604e-4361-4846-8661-14cfd1c44f63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ba5135df225d5c54aee6af57cd59c5e6e140a2ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172656"
---
# <a name="sysserver_event_session_actions-transact-sql"></a>sys.server_event_session_actions (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque action d'un événement d'une session d'événements.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID de la session d'événements. N'accepte pas la valeur NULL.|  
|event_id|**int**|ID de l'événement. Cet ID est unique dans l'objet de la session d'événements. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom de l’action. Autorise la valeur NULL.|  
|package|**sysname**|Nom du package d'événement qui contient l'événement. Autorise la valeur NULL.|  
|module|**sysname**|Nom du module qui contient l'événement. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Cette vue a les cardinalités de relation suivantes.  
  
| Du | À | Relation |
| ---- | -- | ------------ |
|sys.server_event_session_actions.event_session_id|sys.sys.server_event_sessions.event_session_id|Plusieurs-à-un|  
|sys.server_event_session_actions.event_id<br /><br /> sys.server_event_session_actions.event_session_id|sys.server_event_session_events.event_session_id<br /><br /> sys.server_event_session_events.event_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  

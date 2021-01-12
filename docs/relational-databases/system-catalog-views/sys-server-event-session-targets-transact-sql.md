---
description: sys.server_event_session_targets (Transact-SQL)
title: sys.server_event_session_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_event_session_targets_TSQL
- sys.server_event_session_targets_TSQL
- sys.server_event_session_targets
- server_event_session_targets
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_session_targets catalog view
- xe
ms.assetid: dda4879d-57ae-4267-b410-1ef5c37404c7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a3b36cfd632ec01ab483021f7ee9705d7871540d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097893"
---
# <a name="sysserver_event_session_targets-transact-sql"></a>sys.server_event_session_targets (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque cible d'événement d'une session d'événements.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID de la session d'événements. N'accepte pas la valeur NULL.|  
|target_id|**int**|ID de la cible. Cet ID est unique au sein de l'objet de la session d'événements. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom de la cible d'événement. N'accepte pas la valeur NULL.|  
|package|**sysname**|Nom du package d'événement qui contient la cible d'événement. N'accepte pas la valeur NULL.|  
|module|**sysname**|Nom du module qui contient la cible d'événement. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Cette vue a les cardinalités de relation suivantes.  
  
| Du | À | Relationship |
| ---- | -- | ------------ |
|sys.server_event_session_targets.event_session_id|sys.server_event_sessions sys.server_event_sessions.event_session_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  

---
description: MStracer_history (Transact-SQL)
title: MStracer_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MStracer_history
- MStracer_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MStracer_history system table
ms.assetid: 97237a0c-d574-4b17-8a94-1a8730b31d98
author: cawrites
ms.author: chadam
ms.openlocfilehash: c419865662e3a88bf327035376ddc8a383dbd591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211570"
---
# <a name="mstracer_history-transact-sql"></a>MStracer_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MStracer_history** conserve un enregistrement de tous les jetons de suivi qui ont été reçus au niveau de l’abonné. Cette table est stockée dans la base de données de distribution et sert au moment de la réplication à l'analyse des performances.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**parent_tracer_id**|**int**|Identifie de manière unique un jeton de suivi.|  
|**agent_id**|**int**|Identifie l'Agent qui a géré l'enregistrement du jeton de suivi.|  
|**subscriber_commit**|**datetime**|Date et heure auxquelles l'enregistrement du jeton de suivi a été validé sur l'Abonné.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

---
description: MStracer_tokens (Transact-SQL)
title: MStracer_tokens (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MStracer_tokens_TSQL
- MStracer_tokens
dev_langs:
- TSQL
helpviewer_keywords:
- MStracer_tokens system table
ms.assetid: b273aa48-8188-4213-8e2c-311543c3236f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5c004c7a578e1fd2a21cd44f8149d2f850dec8d1
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89547038"
---
# <a name="mstracer_tokens-transact-sql"></a>MStracer_tokens (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MStracer_tokens** conserve un enregistrement des enregistrements de jeton de suivi insérés dans une publication. Cette table est stockée dans la base de données de distribution et sert au moment de la réplication à l'analyse des performances.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**tracer_id**|**int**|Identifie un enregistrement de jeton de suivi.|  
|**publication_id**|**int**|Identifie la publication dans laquelle l'enregistrement du jeton de suivi a été inséré.|  
|**publisher_commit**|**datetime**|Date et heure du moment où l'enregistrement du jeton de suivi a été validé sur le serveur de publication.|  
|**distributor_commit**|**datetime**|Date et heure du moment où l'enregistrement du jeton de suivi a été validé sur le serveur de distribution.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

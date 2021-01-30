---
description: MSsubscription_articles (Transact-SQL)
title: MSsubscription_articles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_articles
- MSsubscription_articles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_articles system table
ms.assetid: dbc1737f-261e-4017-b9cd-703b9fc4ac78
author: cawrites
ms.author: chadam
ms.openlocfilehash: 16d9d91b7c3948f18d90b235c4f853d1220c5380
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211600"
---
# <a name="mssubscription_articles-transact-sql"></a>MSsubscription_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tableau **MSsubscription_articles** contient des informations sur les articles d’un abonnement en file d’attente. La table est remplie seulement pour les types de réplications Mise à jour en attente et Mise à jour immédiate avec la mise à jour en file d'attente comme mode de basculement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID de l'Agent qui fournit des services à cet article.|  
|**artid**|**int**|ID d’article de la table **sysarticles** .|  
|**article**|**sysname**|Nom de l’article de la table **sysarticles** .|  
|**dest_table**|**sysname**|Nom de la table de destination à partir de la table **sysarticles** .|  
|**propriétaire**|**sysname**|Propriétaire de l'abonnement.|  
|**cft_table**|**sysname**|Nom de la table de conflits de l'article pour le type de réplication Mise à jour en attente.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

---
description: MSmerge_identity_range_allocations (Transact-SQL)
title: MSmerge_identity_range_allocations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_identity_range_allocations
- MSmerge_identity_range_allocations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_identity_range_allocations system table
ms.assetid: 6362e35e-0ab3-4638-855b-1ce013f5fd6d
author: cawrites
ms.author: chadam
ms.openlocfilehash: c86e1c65af5852d08ea3e1bd331ab04be8c5a5ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209603"
---
# <a name="msmerge_identity_range_allocations-transact-sql"></a>MSmerge_identity_range_allocations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_identity_range_allocations** permet d’effectuer le suivi de l’historique des affectations de plages d’identité, aux éditeurs et aux abonnés, pour les articles publiés. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**nvarchar(128)**|Nom de la base de données de publication.|  
|**édition**|**nvarchar(128)**|Nom de la publication.|  
|**article**|**nvarchar(128)**|Nom de l’article.|  
|**côté**|**nvarchar(128)**|Nom de l'Abonné.|  
|**subscriber_db**|**nvarchar(128)**|Nom de la base de données d’abonnement.|  
|**is_pub_range**|**bit**|Indique si la plage d'identité est affectée à un serveur de publication.|  
|**ranges_allocated**|**tinyint**|Nombre de plages d'identité affectées.|  
|**range_begin**|**numérique (38)**|Valeur de départ de la plage.|  
|**range_end**|**numérique (38)**|Dernière valeur de la plage.|  
|**next_range_begin**|**numérique (38)**|Valeur de début de la plage suivante à affecter.|  
|**next_range_end**|**numérique (38)**|Valeur de fin de la plage suivante à affecter.|  
|**max_used**|**numérique (38)**|Valeur d'identité la plus élevée utilisée.|  
|**time_of_allocation**|**datetime**|Heure à laquelle l'affectation a été faite.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

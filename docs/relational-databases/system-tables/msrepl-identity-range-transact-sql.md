---
description: MSrepl_identity_range (Transact-SQL)
title: MSrepl_identity_range (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_identity_range_TSQL
- MSrepl_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_identity_range system table
ms.assetid: 6e92a8e8-7667-4c98-b1c4-46735bac50d8
author: cawrites
ms.author: chadam
ms.openlocfilehash: f2608a2012f22fa8db8fdbab1b982e77aacf8e0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179170"
---
# <a name="msrepl_identity_range-transact-sql"></a>MSrepl_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tableau **MSrepl_identity_range** fournit la prise en charge de la gestion des plages d’identité. Cette table est stockée dans les bases de données de publication, de distribution et d'abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données de publication.|  
|**TableName**|**sysname**|Nom de la table.|  
|**identity_support**|**int**|Indique si la gestion automatique des plages d'identité est activée. La valeur 0 indique que la gestion automatique des plages d'identité n'est pas activée.|  
|**next_seed**|**bigint**|Si la gestion automatique des plages d'identité est activée, indique le point de départ de la plage suivante.|  
|**pub_range**|**bigint**|Taille de la plage d'identité du serveur de publication.|  
|**range**|**bigint**|Taille des valeurs d'identité consécutives qui seraient affectées aux abonnés dans le cas d'un ajustement.|  
|**max_identity**|**bigint**|Limite maximale de la plage d'identité.|  
|**threshold**|**int**|Seuil de la plage d'identité exprimé en pourcentage.|  
|**current_max**|**bigint**|Maximum actuel qui est susceptible d'être attribué.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

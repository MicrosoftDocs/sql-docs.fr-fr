---
description: MSpub_identity_range (Transact-SQL)
title: MSpub_identity_range (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpub_identity_range_TSQL
- MSpub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSpub_identity_range system table
ms.assetid: 68746eef-32e1-42bc-aff0-9798cd0e88b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 526620d1fe5ed7c00eed23b4433f3f7630205153
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098541"
---
# <a name="mspub_identity_range-transact-sql"></a>MSpub_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tableau **MSpub_identity_range** fournit la prise en charge de la gestion des plages d’identité. Cette table est stockée dans les bases de données de publication et d'abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|ID de la table dont la colonne d'identité est gérée par la réplication.|  
|**range**|**bigint**|Contrôle la taille de la plage des valeurs d'identité consécutives qui seraient affectées à l'abonnement dans le cadre d'un ajustement.|  
|**pub_range**|**bigint**|Contrôle la taille de la plage des valeurs d'identité consécutives qui seraient affectées à la publication dans le cadre d'un ajustement.|  
|**current_pub_range**|**bigint**|Plage actuellement utilisée par la publication. Elle peut être différente de *pub_range* si elle est affichée après avoir été modifiée par **sp_changearticle** et avant l’ajustement suivant de la plage.|  
|**threshold**|**int**|Valeur de pourcentage qui contrôle le moment où l'Agent de distribution affecte une nouvelle plage d'identité. Lorsque le pourcentage de valeurs spécifié dans *seuil* est utilisé, la agent de distribution crée une nouvelle plage d’identité.|  
|**last_seed**|**bigint**|Limite inférieure de la plage actuelle.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

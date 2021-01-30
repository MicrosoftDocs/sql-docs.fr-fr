---
description: sys.dm_repl_tranhash (Transact-SQL)
title: sys.dm_repl_tranhash (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_repl_tranhash
- sys.dm_repl_tranhash_TSQL
- dm_repl_tranhash_TSQL
- dm_repl_tranhash
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_tranhash dynamic management view
ms.assetid: 0cc52338-e805-4ed4-9835-b19bbf72448e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 234a2549d4eaffee6133d35a660c1bbcfb26dd60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99137354"
---
# <a name="sysdm_repl_tranhash-transact-sql"></a>sys.dm_repl_tranhash (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les transactions en cours de réplication dans une publication transactionnelle.  
  
|column_name|data_type|description|  
|------------------|----------------|-----------------|  
|**compartiments**|**bigint**|Nombre de compartiments dans la table de hachage.|  
|**hashed_trans**|**bigint**|Nombre de transactions validées qui ont été répliquées dans le traitement en cours.|  
|**completed_trans**|**bigint**|Nombre de transactions terminées jusqu'à présent.|  
|**compensated_trans**|**bigint**|Nombre de transactions contenant des restaurations partielles.|  
|**first_begin_lsn**|**nvarchar (64)**|Numéro séquentiel dans le journal (NSE) de départ le plus ancien dans le traitement en cours.|  
|**last_commit_lsn**|**nvarchar (64)**|Numéro NSE de la dernière validation dans le traitement en cours.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation VIEW DATABASE STATE sur la base de données de publication pour appeler **dm_repl_tranhash**.  
  
## <a name="remarks"></a>Notes  
 Les informations ne sont retournées que pour les objets de base de données répliqués actuellement chargés dans le cache des articles de réplication.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

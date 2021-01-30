---
description: MSmerge_altsyncpartners (Transact-SQL)
title: MSmerge_altsyncpartners (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_altsyncpartners_TSQL
- MSmerge_altsyncpartners
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_altsyncpartners system table
ms.assetid: da51b0f8-5ad0-4aeb-96ed-2b3672a2a6e2
author: cawrites
ms.author: chadam
ms.openlocfilehash: de4f7eeb04d3c64c4eb08c1435cc7d6e671fca08
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212282"
---
# <a name="msmerge_altsyncpartners-transact-sql"></a>MSmerge_altsyncpartners (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tableau **MSmerge_altsyncpartners** effectue le suivi de l’Association des partenaires de synchronisation actuels pour un serveur de publication. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**subid**|**uniqueidentifier**|Identificateur du serveur de publication d'origine.|  
|**alternate_subid**|**uniqueidentifier**|Identificateur de l'Abonné qui est le partenaire de synchronisation alternatif.|  
|**description**|**nvarchar(255)**|Description du partenaire de synchronisation alternatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

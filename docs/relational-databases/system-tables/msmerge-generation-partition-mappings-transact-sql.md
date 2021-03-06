---
title: MSmerge_generation_partition_mappings (T-SQL)
description: Décrit la procédure stockée MSmerge_generation_partition_mappings utilisée pour effectuer le suivi des modifications apportées aux partitions dans une publication de fusion.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_generation_partition_mappings_TSQL
- MSmerge_generation_partition_mappings
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_generation_partition_mappings system table
ms.assetid: 443a4024-ce48-4772-9ee5-95bd6fb6476b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f4a06b0474eef46d7d31d26439b4be4828ba2c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186924"
---
# <a name="msmerge_generation_partition_mappings-transact-sql"></a>MSmerge_generation_partition_mappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_generation_partition_mappings** permet d’effectuer le suivi des modifications apportées aux partitions dans une publication de fusion. Cette table est stockée dans les bases de données de publication et scubscription.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publication_number**|**smallint**|Identifie la publication de fusion.|  
|**toute**|**bigint**|Valeur de génération.|  
|**partition_id**|**int**|Identifie la partition.|  
|**changecount**|**int**|Nombre de fois où la partition a été modifiée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

---
title: MSmerge_generation_partition_mappings (T-SQL)
description: Décrit la procédure stockée MSmerge_generation_partition_mappings utilisée pour effectuer le suivi des modifications apportées aux partitions dans une publication de fusion.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_generation_partition_mappings_TSQL
- MSmerge_generation_partition_mappings
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_generation_partition_mappings system table
ms.assetid: 443a4024-ce48-4772-9ee5-95bd6fb6476b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ae8e89600a76da91ccf06d0a561319af9bec3f1
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89544509"
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
  
  

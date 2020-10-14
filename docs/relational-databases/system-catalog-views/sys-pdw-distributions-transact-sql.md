---
description: sys.pdw_distributions (Transact-SQL)
title: sys.pdw_distributions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 572b5187-9753-4063-adf8-65dea87d11f8
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 4181b82b6512211d91d23c76b797aedeec761b7f
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036760"
---
# <a name="syspdw_distributions-transact-sql"></a>sys.pdw_distributions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur les distributions sur l’appliance. Elle répertorie une ligne par distribution d’appliance.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|distribution_id|**int**|ID numérique unique associé à la distribution.<br /><br /> Clé pour cette vue.|1 au nombre de nœuds de calcul dans l’appliance multipliés par le nombre de distributions par nœud de calcul.|  
|pdw_node_id|**int**|ID du nœud sur lequel cette distribution se trouve.|Consultez pdw_node_id dans [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|name|**nvarchar(32)**|Identificateur de chaîne associé à la distribution, utilisé comme suffixe sur les tables distribuées.|Chaîne composée de’A-Z', 'a-z', ' 0-9 ', ' _ ', '-'.|  
|position|**int**|Position de la distribution au sein d’un nœud pour les autres distributions sur ce nœud.|1 au nombre de distributions par nœud.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

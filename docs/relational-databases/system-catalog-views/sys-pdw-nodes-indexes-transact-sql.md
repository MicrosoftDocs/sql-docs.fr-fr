---
description: sys.pdw_nodes_indexes (Transact-SQL)
title: sys.pdw_nodes_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 261bcb7f-a906-4979-b274-bc5f1aa66426
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a9fee9ec599444091e0543b0ed7e4fd326fdaa02
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186505"
---
# <a name="syspdw_nodes_indexes-transact-sql"></a>sys.pdw_nodes_indexes (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Retourne les index de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|object_id|**int**|ID de l’objet auquel appartient cet index.||  
|name|**sysname**|Nom de l'index. Le nom est unique dans l’objet. NULL = Segment||  
|index_id|**int**|ID de l’index. index_id n'est unique qu'à l'intérieur de l'objet.<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = index cluster<br /><br /> > 1 = Index non-cluster||  
|type|**tinyint**|Type de l'index :<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = Clustered<br /><br /> 2 = Non cluster<br /><br /> 5 = index ColumnStore optimisé en mémoire xVelocity en cluster|  
|type_desc|**nvarchar(60)**|Description du type d'index :<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> COLUMNSTORE EN CLUSTER||  
|is_unique|**bit**|0 = L'index n'est pas unique.|Toujours 0.|  
|data_space_id|**int**|ID de l’espace de données pour cet index. L'espace de données est soit un groupe de fichiers, soit un schéma de partition.<br /><br /> 0 = object_id est une fonction table.||  
|ignore_dup_key|**bit**|0 = IGNORE_DUP_KEY est OFF.|Toujours 0.|  
|is_primary_key|**bit**|1 = L'index fait partie d'une contrainte PRIMARY KEY.|Toujours 0.|  
|is_unique_constraint|**bit**|1 = L'index fait partie d'une contrainte UNIQUE.|Toujours 0.|  
|fill_factor|**tinyint**|> 0 = pourcentage de FILLFACTOR utilisé lors de la création ou de la reconstruction de l’index.<br /><br /> 0 = Valeur par défaut|Toujours 0.|  
|is_padded|**bit**|0 = PADINDEX est OFF.|Toujours 0.|  
|is_disabled|**bit**|1 = L'index est désactivé.<br /><br /> 0 = L'index n'est pas désactivé.||  
|is_hypothetical|**bit**|0 = L'index n'est pas hypothétique.|Toujours 0.|  
|allow_row_locks|**bit**|1 = Index autorisant les verrous de ligne|Toujours 1.|  
|allow_page_locks|**bit**|1 = Index autorisant les verrous de page|Toujours 1.|  
|has_filter|**bit**|0 = Index ne disposant pas de filtre.|Toujours 0.|  
|filter_definition|**nvarchar(max)**|Expression pour le sous-ensemble de lignes inclus dans l'index filtré.|Toujours NULL.|  
|pdw_node_id|**int**|Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|NOT NULL|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

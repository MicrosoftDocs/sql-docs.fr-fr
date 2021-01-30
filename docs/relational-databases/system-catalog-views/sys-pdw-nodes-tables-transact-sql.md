---
description: sys.pdw_nodes_tables (Transact-SQL)
title: sys.pdw_nodes_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 473b5d14-171b-4a16-9195-acf36d3f786c
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2dd5f56a6b394d25ad8d1f2aa7e661a2bf21155e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207809"
---
# <a name="syspdw_nodes_tables-transact-sql"></a>sys.pdw_nodes_tables (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient une ligne pour chaque objet table qu’un principal possède ou sur lequel une autorisation a été accordée au principal.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. Objects](../system-catalog-views/sys-objects-transact-sql.md).||  
|lob_data_space_id|**int**||Toujours 0.|  
|filestream_data_space_id|**int**|ID d’espace de données pour un groupe de fichiers FILESTREAM ou [!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|NULL|  
|max_column_id_used|**int**|ID de colonne maximal utilisé par cette table.||  
|lock_on_bulk_load|**bit**|La table est verrouillée pour le chargement en masse.|TBD|  
|uses_ansi_nulls|**bit**|Lorsque la table a été créée, l'option de base de données SET ANSI_NULLS avait pour valeur ON.|1|  
|is_replicated|**bit**|1 = la table est publiée à l’aide de la réplication.|entre la réplication n’est pas prise en charge.|  
|has_replication_filter|**bit**|1 = la table possède un filtre de réplication.|0|  
|is_merge_published|**bit**|1 = la table est publiée à l'aide de la réplication de fusion.|entre non pris en charge.|  
|is_sync_tran_subscribed|**bit**|1 = la table est abonnée à l'aide d'un abonnement avec mise à jour immédiate.|entre non pris en charge.|  
|has_unchecked_assembly_data|**bit**|1 = La table contient des données persistantes qui dépendent d'un assembly dont la définition a été modifiée lors de la dernière exécution de ALTER ASSEMBLY. Cette valeur est réinitialisée à 0 après exécution correcte de l'opération DBCC CHECKDB ou DBCC CHECKTABLE suivante.|entre aucune prise en charge du CLR.|  
|text_in_row_limit|**int**|0 = l'option « text in row » n'est pas définie.|Toujours 0.|  
|large_value_types_out_of_row|**bit**|1 = les types de valeur élevée sont stockés en dehors de la ligne.|Toujours 0.|  
|is_tracked_by_cdc|**bit**|1 = la table est activée pour la capture de données modifiées|Toujours 0 ; aucune prise en charge de CDC.|  
|lock_escalation|**tinyint**|Valeur de l’option LOCK_ESCALATION pour la table : 2 = AUTO|Toujours 2.|  
|lock_escalation_desc|**nvarchar(60)**|Description textuelle de l’option lock_escalation.|Always ꞌ AUTO ꞌ.|  
|pdw_node_id|**int**|Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|NOT NULL|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

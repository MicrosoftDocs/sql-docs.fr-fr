---
description: cdc.index_columns (Transact-SQL)
title: CDC. index_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.index_columns_TSQL
- cdc.index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.index_columns
ms.assetid: 256ec8a5-3031-40a8-9fdb-99db42ea453d
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 0725f3eaa01da493681b2f45186c077a46d20e5e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88460411"
---
# <a name="cdcindex_columns-transact-sql"></a>cdc.index_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque colonne d'index associée à une table de modifications. Les colonnes d'index sont utilisées par la capture des données modifiées pour identifier de façon unique les lignes dans la table source. Par défaut, les colonnes de la clé primaire de la table source sont incluses. Toutefois, si un index unique de la table source est spécifié quand la capture des données modifiées est activée sur la table source, les colonnes de cet index sont utilisées à la place. Une clé primaire ou un index unique est requis sur la table source si le suivi des modifications nettes est activé. Pour plus d’informations, consultez [sys. sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 Nous vous recommandons de ne pas interroger les tables système directement. Au lieu de cela, exécutez la procédure stockée [sys. sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) .  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table de modifications.|  
|**column_name**|**sysname**|Nom de la colonne d'index.|  
|**index_ordinal**|**tinyint**|Ordinal (de base 1) de la colonne dans l'index.|  
|**column_id**|**int**|ID de la colonne dans la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [CDC. change_tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  

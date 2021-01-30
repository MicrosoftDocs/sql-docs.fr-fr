---
description: cdc.captured_columns (Transact-SQL)
title: cdc.captured_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.captured_columns
- cdc.captured_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.captured_columns
ms.assetid: 7bb4d408-d764-4ef6-802c-f271c8d39c2a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bd03070e83857515aa6b2704780ede3f107ec7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183366"
---
# <a name="cdccaptured_columns-transact-sql"></a>cdc.captured_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque colonne suivie dans une instance de capture. Par défaut, toutes les colonnes de la table source sont capturées. Toutefois, les colonnes peuvent être incluses ou exclues lorsque la table source est activée pour la capture des données modifiées en spécifiant une liste de colonnes. Pour plus d’informations, consultez [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 Nous vous recommandons de ne **pas interroger directement les tables système**. Au lieu de cela, exécutez la procédure stockée [sys.sp_cdc_get_source_columns](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md) .  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table de modifications à laquelle appartient la colonne capturée.|  
|**column_name**|**sysname**|Nom de la colonne capturée.|  
|**column_id**|**int**|ID de la colonne capturée dans la table source.|  
|**column_type**|**sysname**|Type de la colonne capturée.|  
|**column_ordinal**|**int**|Ordinal de colonne (de base 1) dans la table de modifications. Les colonnes de métadonnées dans la table de modifications sont exclues. L'ordinal 1 est assigné à la première colonne capturée.|  
|**is_computed**|**bit**|Indique que la colonne capturée est une colonne calculée dans la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [cdc.change_tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  

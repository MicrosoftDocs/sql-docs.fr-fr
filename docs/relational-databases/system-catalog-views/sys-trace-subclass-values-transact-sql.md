---
description: sys.trace_subclass_values (Transact-SQL)
title: sys. trace_subclass_values (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.trace_subclass_values
- trace_subclass_values_TSQL
- sys.trace_subclass_values_TSQL
- trace_subclass_values
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_subclass_values catalog view
ms.assetid: 542b19ca-61c8-41ca-aa2e-0aba8906cc24
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 2c13e9d2f1ce861a2240495f504e945141a01c12
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88400265"
---
# <a name="systrace_subclass_values-transact-sql"></a>sys.trace_subclass_values (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L’affichage catalogue **sys. trace_subclass_values** contient une liste de valeurs de colonnes nommées. Ces valeurs de sous-classe ne changent pas pour une version donnée de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Pour obtenir la liste complète des événements de trace pris en charge, consultez [SQL Server référence](../../relational-databases/event-classes/sql-server-event-class-reference.md)de la classe d’événements.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt les affichages catalogue des événements étendus.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|ID de l'événement de trace. Ce paramètre se trouve également dans l’affichage catalogue **sys. trace_events** .|  
|**trace_column_id**|**smallint**|ID de la colonne de trace utilisé pour l'énumération. Ce paramètre se trouve également dans l’affichage catalogue **sys. trace_columns** .|  
|**subclass_name**|**nvarchar(128)**|Signification de la valeur de colonne.|  
|**subclass_value**|**smallint**|Valeur de colonne.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys. traces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys. trace_categories &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys. trace_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys. trace_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys. trace_event_bindings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)  
  
  

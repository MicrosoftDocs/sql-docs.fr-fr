---
description: cdc.ddl_history (Transact-SQL)
title: cdc.ddl_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.ddl_history_TSQL
- cdc.ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.ddl_history
ms.assetid: cb97ea71-da2f-441a-bbd2-db1f5f48ab49
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7536929659cf4c1c329116a27ab456efc0d9e5e0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094922"
---
# <a name="cdcddl_history-transact-sql"></a>cdc.ddl_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque modification du langage de définition de données (DDL) apportée aux tables qui sont activées pour la capture des données modifiées. Vous pouvez utiliser cette table pour déterminer le moment où une modification DDL a eu lieu sur une table source et identifier cette modification. Les tables sources qui n'ont pas subi de modifications DDL n'auront pas d'entrées dans cette table.  
  
 Nous vous recommandons de ne pas interroger les tables système directement. Au lieu de cela, exécutez la procédure stockée [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md) .  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**source_object_id**|**int**|Identificateur de la table source à laquelle la modification DDL a été appliquée.|  
|**object_id**|**int**|ID de la table de modifications associée à une instance de capture pour la table source.|  
|**required_column_update**|**bit**|Indique que le type de données d'une colonne capturée a été modifié dans la table source. Ce changement a modifié la colonne dans la table de modifications.|  
|**ddl_command**|**nvarchar(max)**|Instruction DDL appliquée à la table source.|  
|**ddl_lsn**|**binary(10)**|Numéro séquentiel dans le journal associé à la validation de la modification DDL.|  
|**ddl_time**|**datetime**|Date et heure auxquelles la modification DDL a été apportée à la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  

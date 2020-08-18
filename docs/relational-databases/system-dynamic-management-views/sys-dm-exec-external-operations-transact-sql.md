---
description: sys. dm_exec_external_operations (Transact-SQL)
title: sys. dm_exec_external_operations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_EXTERNAL_OPERATIONS_TSQL
- DM_EXEC_EXTERNAL_OPERATIONS
- SYS.DM_EXEC_EXTERNAL_OPERATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_external_operations management view
- dm_exec_external_operations management view
ms.assetid: d268217a-85b8-4b7f-9cd1-87865eba2be1
author: CarlRabeler
ms.author: carlrab
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 82d98e2086650c1ba22e2b0b7aa7e59976e22e5a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88398495"
---
# <a name="sysdm_exec_external_operations-transact-sql"></a>sys. dm_exec_external_operations (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Capture des informations sur les opérations Polybase externes.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|Identificateur de requête unique associé à la requête Polybase|Consultez ID dans [sys. dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|  
|step_index|**int**|Index de l’étape de la requête|Consultez step_index dans [sys. dm_exec_distributed_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md)|  
|type de operation_|**nvarchar(128)**|Décrit une opération Hadoop ou une autre opération externe|'Opération Hadoop externe'|  
|nom de l’operation_|**nvarchar(4000)**|Indique comment l’état du travail en pourcentage (combien est l’entrée consommée)|0-1-multiplié par le facteur 100 (terminé)|  
|progression de la map_|**float**|Indique comment l’état d’une tâche réduire en pourcentage, le cas échéant|0-1-multiplié par le facteur 100 (terminé)|  
  
## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](https://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  

---
description: sys.dm_exec_compute_node_errors (Transact-SQL)
title: sys.dm_exec_compute_node_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
- DM_EXEC_COMPUTE_NODE_ERRORS
- DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
- PolyBase, views
- dm_exec_compute_node_errors
- sys.dm_exec_compute_node_errors management view
ms.assetid: 9a03c039-70e4-4974-95d8-d3fa45984ffb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6f2de93073776522947a227f60ef9aa3fd894323
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094089"
---
# <a name="sysdm_exec_compute_node_errors-transact-sql"></a>sys.dm_exec_compute_node_errors (Transact-SQL)

[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Retourne les erreurs qui se produisent sur les nœuds de calcul Polybase.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|error_id|`nvarchar(36)`|ID numérique unique associé à l’erreur.|Unique pour toutes les erreurs de requête dans le système|  
|source|`nvarchar(255)`|Description du thread ou du processus source||  
|type|`nvarchar(255)`|Type d'erreur||  
|create_time|`datetime`|Heure de l’occurrence d’erreur||  
|compute_node_id|`int`|Identificateur du nœud de calcul spécifique|Consultez compute_node_id des [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)|  
|rexecution_id|`nvarchar(36)`|Identificateur de la requête Polybase, le cas échéant.||  
|spid|`int`|Identificateur de la session SQL Server||  
|thread_id|`int`|Identificateur numérique du thread sur lequel l’erreur s’est produite.||  
|details|nvarchar(4000)|Description complète des détails de l’erreur.||
|compute_pool_id|`int`|Identificateur unique du pool.|

  
## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

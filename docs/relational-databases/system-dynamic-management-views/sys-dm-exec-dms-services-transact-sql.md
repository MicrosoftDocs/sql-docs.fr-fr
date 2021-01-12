---
description: sys.dm_exec_dms_services (Transact-SQL)
title: sys.dm_exec_dms_services (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_DMS_SERVICES_TSQL
- SYS.DM_EXEC_DMS_SERVICES_TSQL
- DM_EXEC_DMS_SERVICES
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_dms_services management view
- sys.dm_exec_dms_services management view
ms.assetid: 6ac47eef-4293-46b8-8555-07a614837504
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d9376dcef8a436a387f1a1b37e636d1719a43f46
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098947"
---
# <a name="sysdm_exec_dms_services-transact-sql"></a>sys.dm_exec_dms_services (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Contient des informations sur tous les services DMS s’exécutant sur les nœuds de calcul Polybase. Elle répertorie une ligne par instance de service.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|dms_core_id|`int`|ID numérique unique associé au noyau DMS. Clé pour cette vue.|ID unique.|  
|compute_node_id|`int`|ID du nœud sur lequel ce service DMS s’exécute|Consultez *compute_node_id* dans [sys.dm_exec_compute_nodes &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|status|`nvarchar(32)`|État actuel du service DMS||
|compute_pool_id|`int`|Identificateur unique du pool.|

## <a name="see-also"></a>Voir aussi  
 [Résolution des problèmes de Polybase avec les vues de gestion dynamique](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

---
description: sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
title: sys.dm_exec_cached_plan_dependent_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cached_plan_dependent_objects
- dm_exec_cached_plan_dependent_objects_TSQL
- sys.dm_exec_cached_plan_dependent_objects_TSQL
- dm_exec_cached_plan_dependent_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plan_dependent_objects dynamic management function
ms.assetid: 9b6cf5f7-b267-44fb-aac8-f49c9aa10cc1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f26611fba5e00acd67a865e713807724b61bc64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160124"
---
# <a name="sysdm_exec_cached_plan_dependent_objects-transact-sql"></a>sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque plan d'exécution [!INCLUDE[tsql](../../includes/tsql-md.md)], plan d'exécution CLR (Common Language Runtime) et curseur associé à un plan.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_cached_plan_dependent_objects(plan_handle)  
```  
  
## <a name="arguments"></a>Arguments  
*plan_handle*  
Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan. *plan_handle* est **de type varbinary (64)**.   

Le *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**usecounts**|**int**|Nombre d'utilisations du curseur ou contexte d'utilisation.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**memory_object_address**|**varbinary (8)**|Adresse mémoire du curseur ou contexte d'utilisation.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**cacheobjtype**|**nvarchar(50)**|Type d’objet du cache du plan. Colonne n'acceptant pas la valeur NULL. Les valeurs possibles sont<br /><br /> Plan exécutable<br /><br /> Fonction compilée par le CLR<br /><br /> Procédure compilée par le CLR<br /><br /> Curseur|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Diagramme des relations](../../relational-databases/system-dynamic-management-views/media/dm-dependent-objects.gif "Diagramme des relations")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Il en va|Relation|  
|----------|--------|--------|------------------|  
|**dm_exec_cached_plan_dependent_objects**|**dm_os_memory_objects**|**memory_object_address**|Un-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.syscacheobjects &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md)  
  
  

---
description: sys.dm_db_task_space_usage (Transact-SQL)
title: sys.dm_db_task_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_task_space_usage_TSQL
- sys.dm_db_task_space_usage_TSQL
- dm_db_task_space_usage
- sys.dm_db_task_space_usage
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_task_space_usage dynamic management view
ms.assetid: fb0c87e5-43b9-466a-a8df-11b3851dc6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a35bbdb72e5efde46e29233d6378bb2fbadd30d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204838"
---
# <a name="sysdm_db_task_space_usage-transact-sql"></a>sys.dm_db_task_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie l'activité d'allocation/désallocation des pages par tâche pour la base de données.  
  
> [!NOTE]  
>  Cette vue s’applique uniquement à la [base de données tempdb](../../relational-databases/databases/tempdb-database.md).  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_db_task_space_usage**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|ID de la session.|  
|**request_id**|**int**|ID de la demande dans la session.<br /><br /> Une demande porte également le nom de traitement et peut contenir une ou plusieurs requêtes. Plusieurs demandes peuvent être simultanément actives dans une session. Chaque requête dans la demande peut démarrer plusieurs threads (tâches), si un plan d'exécution parallèle est utilisé.|  
|**exec_context_id**|**int**|ID du contexte d'exécution de la tâche. Pour plus d’informations, consultez [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|**database_id**|**smallint**|ID de la base de données.|  
|**user_objects_alloc_page_count**|**bigint**|Nombre de pages réservées ou allouées aux objets utilisateur par cette tâche.|  
|**user_objects_dealloc_page_count**|**bigint**|Nombre de pages désallouées et qui ne sont plus réservées aux objets utilisateur par cette tâche.|  
|**internal_objects_alloc_page_count**|**bigint**|Nombre de pages réservées ou allouées aux objets internes par cette tâche.|  
|**internal_objects_dealloc_page_count**|**bigint**|Nombre de pages désallouées et qui ne sont plus réservées aux objets internes par cette tâche.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="remarks"></a>Notes  
 Les pages IAM ne sont pas incluses dans les nombres de pages indiqués dans cette vue.  
  
 Les compteurs de pages sont initialisés à zéro (0) au début d'une demande. Ces valeurs sont agrégées au niveau de la session lorsque la demande est terminée. Pour plus d’informations, consultez [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 La mise en cache de la table de travail et de la table temporaire, ainsi que les suppressions différées ont une incidence sur le nombre de pages allouées et désallouées dans une tâche particulière.  
  
## <a name="user-objects"></a>Objets utilisateur  
 Les objets suivants sont compris dans les compteurs de pages des objets utilisateurs :  
  
-   les tables et les index définis par l'utilisateur ;  
  
-   les tables et les index système ;  
  
-   les tables temporaires globales et les index ;  
  
-   les tables temporaires locales et les index ;  
  
-   Variables de table  
  
-   les tables renvoyées dans les fonctions table.  
  
## <a name="internal-objects"></a>Objets internes  
 Les objets internes se trouvent uniquement dans **tempdb**. Les objets suivants sont compris dans les compteurs de pages des objets internes :  
  
-   les tables de travail des opérations de curseur ou de mise en attente et le stockage temporaire d'objets LOB ;  
  
-   les fichiers de travail des opérations telles que les jointures de hachage ;  
  
-   Tris  
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures physiques pour sys.dm_db_session_task_usage](../../relational-databases/system-dynamic-management-views/media/join-dm-db-task-space-usage-1.gif "Jointures physiques pour sys.dm_db_session_task_usage")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relation|  
|----------|--------|------------------|  
|dm_db_task_space_usage.request_id|dm_exec_requests.request_id|Un-à-un|  
|dm_db_task_space_usage.session_id|dm_exec_requests.session_id|Un-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)   
 [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)  
  
  



---
description: sys.dm_db_file_space_usage (Transact-SQL)
title: sys.dm_db_file_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_file_space_usage
- sys.dm_db_file_space_usage_TSQL
- sys.dm_db_file_space_usage
- dm_db_file_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_file_space_usage dynamic management view
ms.assetid: 148a5276-a8d5-49d2-8146-3c63d24c2144
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bca14dfbb84763a15791aa4bdefc8b857df9e87e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095166"
---
# <a name="sysdm_db_file_space_usage-transact-sql"></a>sys.dm_db_file_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations sur l’utilisation de l’espace pour chaque fichier de données de la base de données.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_db_file_space_usage**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|ID de la base de données.|  
|file_id|**smallint**|ID de fichier.<br /><br /> file_id est mappé à file_id dans [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) et à fileid dans les [ fichierssys.sys](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md).|  
|filegroup_id|**smallint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> ID de groupe de fichiers.|  
|total_page_count|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Nombre total de pages dans le fichier de données.|  
|allocated_extent_page_count|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Nombre total de pages dans les étendues allouées dans le fichier de données.|  
|unallocated_extent_page_count|**bigint**|Nombre total de pages dans les étendues non allouées dans le fichier de données.<br /><br /> Cette valeur ne comprend pas les pages inutilisées dans les étendues allouées.|  
|version_store_reserved_page_count|**bigint**|Nombre total de pages dans les étendues uniformes allouées pour le magasin de versions. Les pages du magasin de versions ne sont jamais allouées à partir d'étendues mixtes.<br /><br /> Les pages IAM ne sont pas incluses parce qu'elles sont toujours allouées à partir d'étendues mixtes. Les pages PFS sont incluses si elles sont allouées à partir d'une étendue uniforme.<br /><br /> Pour plus d’informations, consultez [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).|  
|user_object_reserved_page_count|**bigint**|Nombre total de pages allouées à partir d'étendues uniformes pour les objets utilisateur de la base de données. Ce nombre inclut les pages non utilisées provenant d'une étendue allouée.<br /><br /> Les pages IAM ne sont pas incluses parce qu'elles sont toujours allouées à partir d'étendues mixtes. Les pages PFS sont incluses si elles sont allouées à partir d'une étendue uniforme.<br /><br /> Vous pouvez utiliser la total_pages colonne dans l’affichage catalogue [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) pour retourner le nombre de pages réservées de chaque unité d’allocation dans l’objet utilisateur. Notez toutefois que la colonne total_pages inclut les pages IAM.|  
|internal_object_reserved_page_count|**bigint**|Nombre total de pages d'étendues uniformes allouées pour des objets internes dans le fichier. Ce nombre inclut les pages non utilisées provenant d'une étendue allouée.<br /><br /> Les pages IAM ne sont pas incluses parce qu'elles sont toujours allouées à partir d'étendues mixtes. Les pages PFS sont incluses si elles sont allouées à partir d'une étendue uniforme.<br /><br /> Il n'existe pas d'affichage catalogue ni d'objet de gestion dynamique qui retourne le nombre de pages de chaque objet interne.|  
|mixed_extent_page_count|**bigint**|Nombre total de pages allouées et non allouées dans les étendues mixtes allouées du fichier. Les étendues mixtes contiennent des pages allouées à différents objets. Ce nombre comprend toutes les pages IAM du fichier.|
|modified_extent_page_count|**bigint**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et versions ultérieures.<br /><br />Nombre total de pages modifiées dans les étendues allouées du fichier depuis la dernière sauvegarde complète de la base de données. Le nombre de pages modifiées peut être utilisé pour effectuer le suivi de la quantité de modifications différentielles dans la base de données depuis la dernière sauvegarde complète, afin de déterminer si une sauvegarde différentielle est nécessaire.|
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
|distribution_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> ID numérique unique associé à la distribution.|  
  
## <a name="remarks"></a>Notes  
 Le nombre de pages se situe toujours au niveau des étendues. Les nombres de pages sont donc toujours des multiples de huit. Les étendues qui contiennent des pages d'allocation GAM (Global Allocation Map) et SGAM (Shared Global Allocation Map) sont des étendues uniformes allouées. Elles ne sont pas incluses dans les nombres de pages décrits précédemment. Pour plus d’informations sur les pages et les étendues, consultez le [Guide d’architecture des pages et des extensions](../../relational-databases/pages-and-extents-architecture-guide.md). 
  
 Le contenu de la Banque des versions actuelle se trouve dans [sys.dm_tran_version_store](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md). Le suivi des pages du magasin de versions n'est pas effectué au niveau des sessions et des tâches, mais au niveau des fichiers. En effet, il s'agit de ressources globales. Une session peut générer des versions, mais ces dernières ne peuvent pas être supprimées lorsque la session se termine. Le nettoyage du magasin de versions doit considérer la transaction la plus longue ayant besoin d'accéder à une version particulière. La transaction la plus longue relative au nettoyage de la Banque des versions peut être détectée en affichant la colonne elapsed_time_seconds dans [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md).  
  
 Des modifications fréquentes dans la colonne mixed_extent_page_count peuvent indiquer une utilisation importante de pages SGAM. Le cas échéant, vous risquez d'observer un grand nombre d'attentes PAGELATCH_UP dans lesquelles la ressource d'attente est une page SGAM. Pour plus d’informations, consultez [sys.dm_os_waiting_tasks &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md), [sys.dm_os_wait_stats &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)et sys.DM_OS_LATCH_STATS &#40;[Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md).  
  
## <a name="user-objects"></a>Objets utilisateur  
 Les objets suivants sont compris dans les compteurs de pages des objets utilisateurs :  
  
-   les tables et les index définis par l'utilisateur ;  
  
-   les tables et les index système ;  
  
-   les tables temporaires globales et les index ;  
  
-   les tables temporaires locales et les index ;  
  
-   Variables de table  
  
-   les tables renvoyées dans les fonctions table.  
  
## <a name="internal-objects"></a>Objets internes  
 Les objets internes se trouvent uniquement dans tempdb. Les objets suivants sont compris dans les compteurs de pages des objets internes :  
  
-   les tables de travail des opérations de curseur ou de mise en attente et le stockage temporaire d'objets LOB ;  
  
-   les fichiers de travail des opérations telles que les jointures de hachage ;  
  
-   Tris  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|sys.dm_db_file_space_usage.database_id, file_id|sys.dm_io_virtual_file_stats.database_id, file_id|Un-à-un|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="determing-the-amount-of-free-space-in-tempdb"></a>Détermination de la quantité d'espace disponible dans tempdb  
 La requête suivante renvoie le nombre total de pages disponibles et l’espace disponible total en mégaoctets (Mo) disponibles dans tous les fichiers de données de **tempdb**.  
  
```sql
USE tempdb;  
GO  
SELECT SUM(unallocated_extent_page_count) AS [free pages],   
(SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]  
FROM sys.dm_db_file_space_usage;  
```  

### <a name="determining-the-amount-of-space-used-by-user-objects"></a>Détermination de la quantité d'espace utilisée par les objets utilisateur  
 La requête ci-dessous retourne le nombre total de pages utilisées par les objets utilisateur et l'espace total en Mo utilisé par les objets utilisateur dans tempdb.  
  
```sql  
USE tempdb;  
GO  
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],  
(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  

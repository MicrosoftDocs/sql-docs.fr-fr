---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
ms.assetid: 1d556c67-5c12-46d5-aa8c-7ec1bb858df7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f6942f95ff8d0f229d52921a692ae7ec74c18f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101535"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie l'ensemble de tous les régisseurs de mémoire actuellement actifs dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_memory_clerks**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary (8)**|Spécifie l'adresse mémoire unique du régisseur de mémoire. Il s'agit de la colonne clé primaire. N'accepte pas la valeur NULL.|  
|**type**|**nvarchar(60)**|Spécifie le type de régisseur de mémoire. Chaque régisseur de mémoire a un type spécifique, par exemple les régisseurs de mémoire CLR MEMORYCLERK_SQLCLR. N'accepte pas la valeur NULL.|  
|**name**|**nvarchar (256)**|Spécifie le nom affecté en interne au régisseur de mémoire. Un composant peut avoir plusieurs régisseurs de mémoire d'un type particulier. Un composant peut choisir d'utiliser des noms spécifiques pour identifier les régisseurs de mémoire du même type. N'accepte pas la valeur NULL.|  
|**memory_node_id**|**smallint**|Spécifie l'identificateur du nœud de mémoire. N'accepte pas la valeur NULL.|  
|**single_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].|  
|**pages_kb**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Spécifie la quantité de mémoire de page allouée, en kilo-octets (Ko), pour ce régisseur de mémoire. N'accepte pas la valeur NULL.|  
|**multi_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantité de mémoire multipage allouée, en Ko. Il s'agit de la quantité de mémoire allouée à l'aide de l'allocateur de pages multiples des nœuds de mémoire. Cette mémoire est allouée en dehors du pool de mémoires tampons ; elle tire parti de l'allocateur virtuel des nœuds mémoire. N'accepte pas la valeur NULL.|  
|**virtual_memory_reserved_kb**|**bigint**|Spécifie la quantité de mémoire virtuelle qui est réservée par un régisseur de mémoire. N'accepte pas la valeur NULL.|  
|**virtual_memory_committed_kb**|**bigint**|Spécifie la quantité de mémoire virtuelle qui est validée par un régisseur de mémoire. La quantité de mémoire validée doit toujours être inférieure à la quantité de mémoire réservée. N'accepte pas la valeur NULL.|  
|**awe_allocated_kb**|**bigint**|Spécifie la quantité de mémoire, en kilo-octets (Ko), verrouillée en mémoire physique et non paginée par le système d'exploitation. N'accepte pas la valeur NULL.|  
|**shared_memory_reserved_kb**|**bigint**|Spécifie la quantité de mémoire partagée qui est réservée par un régisseur de mémoire. Il s'agit de la quantité de mémoire réservée à l'usage de la mémoire partagée et du mappage de fichiers. N'accepte pas la valeur NULL.|  
|**shared_memory_committed_kb**|**bigint**|Spécifie la quantité de mémoire partagée qui est validée par le régisseur de mémoire. N'accepte pas la valeur NULL.|  
|**page_size_in_bytes**|**bigint**|Spécifie la granularité de l'allocation de page pour ce régisseur de mémoire. N'accepte pas la valeur NULL.|  
|**page_allocator_address**|**varbinary (8)**|Spécifie l'adresse de l'allocateur de page mémoire. Cette adresse est unique pour un régisseur de mémoire et peut être utilisée dans **sys.dm_os_memory_objects** pour localiser des objets mémoire qui sont liés à ce Clerk. N'accepte pas la valeur NULL.|  
|**host_address**|**varbinary (8)**|Spécifie l'adresse mémoire de l'hôte associé à ce régisseur de mémoire. Pour plus d’informations, consultez [sys.dm_os_hosts &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). Les composants, tels que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, accèdent aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ressources mémoire par le biais de l’interface hôte.<br /><br /> 0x00000000 = Le régisseur de mémoire appartient à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> N'accepte pas la valeur NULL.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations 

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="remarks"></a>Notes  
 Le gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se compose d'une hiérarchie à trois niveaux. La base de la hiérarchie est constituée par les nœuds de mémoire. Le niveau intermédiaire est constitué de régisseurs de mémoire, de caches mémoire et de pools de mémoires. Le niveau supérieur comprend les objets de mémoire. Ces objets sont généralement utilisés pour allouer de la mémoire dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les nœuds de mémoire fournissent l'interface et assurent l'implémentation des allocateurs de niveau inférieur. Au sein de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seuls les régisseurs de mémoire ont accès aux nœuds de mémoire. Les régisseurs de mémoire accèdent aux interfaces des nœuds de mémoire pour allouer de la mémoire. Les nœuds de mémoire assurent également le suivi de la mémoire allouée en utilisant le Clerk pour les diagnostics. Chaque composant allouant une quantité importante de mémoire doit créer son propre régisseur de mémoire et allouer toute sa mémoire à l'aide des interfaces du régisseur de mémoire. Souvent, les composants créent leurs régisseurs de mémoire lors du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  

 [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
  



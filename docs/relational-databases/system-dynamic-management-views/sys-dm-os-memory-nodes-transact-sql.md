---
description: sys.dm_os_memory_nodes (Transact-SQL)
title: sys.dm_os_memory_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_nodes_TSQL
- sys.dm_os_memory_nodes
- sys.dm_os_memory_nodes_TSQL
- dm_os_memory_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_nodes dynamic management view
ms.assetid: bf4032fe-7db1-40e9-a62e-d69cebff4b44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7bec1e868a8531c8a007b94c69a925a05711bde2
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836830"
---
# <a name="sysdm_os_memory_nodes-transact-sql"></a>sys.dm_os_memory_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les allocations qui sont internes à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le suivi de la différence entre les compteurs de mémoire de processus **sys.dm_os_process_memory** et les compteurs internes peut indiquer l’utilisation de la mémoire par des composants externes dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espace mémoire.  
  
 Les nœuds sont créés en fonction des nœuds de mémoire NUMA physiques. Celles-ci peuvent être différentes des nœuds de l’UC dans **sys.dm_os_nodes**.  
  
 Aucune allocation effectuée directement par le biais de routines d'allocations de mémoire Windows ne fait l'objet d'un suivi. Le tableau suivant fournit des informations sur les allocations de mémoire effectuées uniquement en utilisant des interfaces du gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_memory_nodes**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**memory_node_id**|**smallint**|Spécifie l'identificateur du nœud de mémoire. Relatif à **memory_node_id** de **sys.dm_os_memory_clerks**. N'accepte pas la valeur NULL.|  
|**virtual_address_space_reserved_kb**|**bigint**|Indique le nombre de réservations d'adresses virtuelles, en kilo-octets (Ko), qui ne sont ni validées ni mappées à des pages physiques. N'accepte pas la valeur NULL.|  
|**virtual_address_space_committed_kb**|**bigint**|Spécifie la quantité d'adresse virtuelle, en Ko, qui a été validée ou mappée à des pages physiques. N'accepte pas la valeur NULL.|  
|**locked_page_allocations_kb**|**bigint**|Spécifie la quantité de mémoire physique, en Ko, qui a été verrouillée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. N'accepte pas la valeur NULL.|  
|**single_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantité de mémoire allouée, en Ko, en utilisant l'allocateur de page unique par les threads en cours d'exécution sur ce nœud. Cette mémoire est allouée à partir du pool de mémoires tampons. Cette valeur indique le nœud où la demande d'allocation s'est produite, et non l'emplacement physique où la demande d'allocation a été satisfaite.|  
|**pages_kb**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Spécifie la quantité de mémoire validée, en Ko, allouée de ce nœud NUMA par l'allocateur de pages du gestionnaire de mémoire. N'accepte pas la valeur NULL.|  
|**multi_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Quantité de mémoire allouée, en Ko, en utilisant l'allocateur de plusieurs pages par les threads en cours d'exécution sur ce nœud. Cette mémoire provient de l'extérieur du pool de mémoires tampons. Cette valeur indique le nœud où les demandes d'allocations se sont produites, et non l'emplacement physique où la demande d'allocation a été satisfaite.|  
|**shared_memory_reserved_kb**|**bigint**|Spécifie la quantité de mémoire partagée, en Ko, qui a été réservée à partir de ce nœud. N'accepte pas la valeur NULL.|  
|**shared_memory_committed_kb**|**bigint**|Spécifie la quantité de mémoire partagée, en Ko, qui a été validée sur ce nœud. N'accepte pas la valeur NULL.|  
|**cpu_affinity_mask**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> À usage interne uniquement N'accepte pas la valeur NULL.|  
|**online_scheduler_mask**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> À usage interne uniquement N'accepte pas la valeur NULL.|  
|**processor_group**|**smallint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> À usage interne uniquement N'accepte pas la valeur NULL.|  
|**foreign_committed_kb**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Spécifie la quantité de mémoire validée, en Ko, d'autres nœuds de mémoire. N'accepte pas la valeur NULL.|  
|**target_kb** |**bigint** |**S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Spécifie l’objectif de mémoire pour le nœud de mémoire, en Ko. |   
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="see-also"></a>Voir aussi  
  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  

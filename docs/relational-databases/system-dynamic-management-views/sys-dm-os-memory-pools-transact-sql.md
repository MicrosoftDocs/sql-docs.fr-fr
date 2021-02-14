---
description: sys.dm_os_memory_pools (Transact-SQL)
title: sys.dm_os_memory_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_pools_TSQL
- dm_os_memory_pools
- dm_os_memory_pools_TSQL
- sys.dm_os_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_pools dynamic management view
ms.assetid: 1ef053f3-c6f3-456e-82b6-26e4bd630d46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5ea86721a9a9b0543bf32f0c2aba17ac5be66368
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100339453"
---
# <a name="sysdm_os_memory_pools-transact-sql"></a>sys.dm_os_memory_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque objet stocké dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser cet vue pour surveiller l'utilisation de la mémoire cache et pour identifier les comportements de mise en cache incorrects.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_memory_pools**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**memory_pool_address**|**varbinary (8)**|Adresse mémoire de l'entrée qui représente le pool de mémoire. N'accepte pas la valeur NULL.|  
|**pool_id**|**int**|Identificateur d'un pool spécifique au sein d'un ensemble de pools. N'accepte pas la valeur NULL.|  
|**type**|**nvarchar(60)**|Type de pool d'objets. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**name**|**nvarchar (256)**|Nom affecté par le système à cet objet de mémoire. N'accepte pas la valeur NULL.|  
|**max_free_entries_count**|**bigint**|Nombre maximum d'entrées libres possibles dans un pool. N'accepte pas la valeur NULL.|  
|**free_entries_count**|**bigint**|Nombre d'entrées actuellement stockées dans le pool. N'accepte pas la valeur NULL.|  
|**removed_in_all_rounds_count**|**bigint**|Nombre d'entrées supprimées du pool depuis le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. N'accepte pas la valeur NULL.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="remarks"></a>Notes  
 Les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent parfois une structure de pools commune pour mettre en mémoire cache des types de données homogènes sans état (stateless). La structure des pools de mémoire est plus simple que celle des mémoires cache. Toutes les entrées des pools sont considérées égales. En interne, les pools sont des régisseurs de mémoire et peuvent être utilisés dans les mêmes situations.  
  
## <a name="see-also"></a>Voir aussi  
 
  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  



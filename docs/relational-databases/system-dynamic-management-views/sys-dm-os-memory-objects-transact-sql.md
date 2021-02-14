---
description: sys.dm_os_memory_objects (Transact-SQL)
title: sys.dm_os_memory_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_objects
- sys.dm_os_memory_objects
- sys.dm_os_memory_objects_TSQL
- dm_os_memory_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_objects dynamic management view
ms.assetid: 5688bcf8-5da9-4ff9-960b-742b671d7096
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd0f5daa6ba1b167aa1f56dce70dabe5e92e4247
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338895"
---
# <a name="sysdm_os_memory_objects-transact-sql"></a>sys.dm_os_memory_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des objets mémoire qui sont actuellement alloués par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser **sys.dm_os_memory_objects** pour analyser l’utilisation de la mémoire et identifier d’éventuelles fuites de mémoire.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**memory_object_address**|**varbinary (8)**|Adresse de l'objet mémoire. N'accepte pas la valeur NULL.|  
|**parent_address**|**varbinary (8)**|Adresse de l'objet mémoire parent. Autorise la valeur NULL.|  
|**pages_allocated_count**|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre de pages allouées par cet objet. N'accepte pas la valeur NULL.|  
|**pages_in_bytes**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Quantité de mémoire, en octets, allouée par cette instance de l'objet mémoire. N'accepte pas la valeur NULL.|  
|**creation_options**|**int**|À usage interne uniquement Autorise la valeur NULL.|  
|**bytes_used**|**bigint**|À usage interne uniquement Autorise la valeur NULL.|  
|**type**|**nvarchar(60)**|Type d'objet mémoire.<br /><br /> Indique le composant auquel cet objet mémoire appartient ou la fonction de l'objet mémoire. Autorise la valeur NULL.|  
|**name**|**varchar(128)**|À usage interne uniquement Autorise la valeur Null.|  
|**memory_node_id**|**smallint**|ID d'un nœud de mémoire utilisé par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**creation_time**|**datetime**|À usage interne uniquement Autorise la valeur NULL.|  
|**max_pages_allocated_count**|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre maximum de pages allouées par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**page_size_in_bytes**|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Taille des pages, en octets, allouée par cet objet. N'accepte pas la valeur NULL.|  
|**max_pages_in_bytes**|**bigint**|Quantité de mémoire maximale utilisée par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**page_allocator_address**|**varbinary (8)**|Adresse mémoire de l'allocateur de page. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**creation_stack_address**|**varbinary (8)**|À usage interne uniquement Autorise la valeur NULL.|  
|**sequence_num**|**int**|À usage interne uniquement Autorise la valeur NULL.|  
|**partition_type**|**int**|**S’applique à** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Le type de partition :<br /><br /> 0-objet mémoire non partitionnée<br /><br /> 1-objet mémoire partitionné, actuellement non partitionné<br /><br /> 2-objet mémoire partitionné, partitionné par le nœud NUMA. Dans un environnement avec un seul nœud NUMA, cette valeur est équivalente à 1.<br /><br /> 3-objet mémoire partitionné, partitionné par UC.|  
|**contention_factor**|**real**|**S’applique à** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Valeur qui spécifie la contention sur cet objet mémoire, 0 signifiant aucun conflit. La valeur est mise à jour chaque fois qu’un nombre spécifié d’allocations de mémoire reflétant une contention a été effectué pendant cette période. S’applique uniquement aux objets de mémoire thread-safe.|  
|**waiting_tasks_count**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Nombre d’attentes sur cet objet mémoire. Ce compteur est incrémenté chaque fois que la mémoire est allouée à partir de cet objet mémoire. L’incrément est le nombre de tâches actuellement en attente d’accès à cet objet mémoire. S’applique uniquement aux objets de mémoire thread-safe. Il s’agit d’une valeur d’effort optimale sans garantie d’exactitude.|  
|**exclusive_access_count**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Spécifie la fréquence à laquelle cet objet mémoire a fait l’objet d’un accès exclusif. S’applique uniquement aux objets de mémoire thread-safe.  Il s’agit d’une valeur d’effort optimale sans garantie d’exactitude.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
 **partition_type**, **contention_factor**, **waiting_tasks_count** et **exclusive_access_count** ne sont pas encore implémentées dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] .  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="remarks"></a>Notes  
 Les objets mémoire sont des segments. Ils fournissent des allocations qui ont une granularité plus fine que celles fournies par les régisseurs de mémoire. Les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des objets mémoire au lieu de régisseurs de mémoire. Les objets mémoire utilisent l'interface d'allocation de page du régisseur de mémoire pour allouer les pages. Ils n'utilisent pas les interfaces de mémoire virtuelle ou partagée. Selon les modèles d'allocation, les composants peuvent créer différents types d'objets mémoire pour allouer des régions de taille arbitraire.  
  
 La taille de page type pour un objet mémoire est de 8 Ko. Toutefois, les objets mémoire incrémentiels peuvent avoir des tailles de page allant de 512 octets à 8 Ko.  
  
> [!NOTE]  
>  La taille de page n'est pas une allocation maximale. Il s'agit en fait de granularité d'allocation prise en charge par un allocateur de page et mise en œuvre par un régisseur de mémoire. Vous pouvez demander des allocations supérieures à 8 Ko d'objets mémoire.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la quantité de mémoire allouée par chaque type d'objet mémoire.  
  
```  
SELECT SUM (pages_in_bytes) as 'Bytes Used', type   
FROM sys.dm_os_memory_objects  
GROUP BY type   
ORDER BY 'Bytes Used' DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)  
  
  



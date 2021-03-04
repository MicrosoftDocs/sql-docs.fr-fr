---
description: sys.dm_os_memory_cache_entries (Transact-SQL)
title: sys.dm_os_memory_cache_entries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7566f26bde72b0b4503d9a3f6499bd967208c68e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839470"
---
# <a name="sysdm_os_memory_cache_entries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur toutes les entrées en mémoire cache dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette vue pour suivre les entrées en mémoire cache en fonction des objets qui leur sont associés. Vous pouvez également utiliser cette vue pour obtenir des statistiques sur les entrées en mémoire cache.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_memory_cache_entries**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary (8)**|Adresse du cache. N'accepte pas la valeur NULL.|  
|**name**|**nvarchar (256)**|Nom du cache. N'accepte pas la valeur NULL.|  
|**type**|**varchar(60)**|Type de cache. N'accepte pas la valeur NULL.|  
|**entry_address**|**varbinary (8)**|Adresse du descripteur de l'entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**entry_data_address**|**varbinary (8)**|Adresse des données utilisateur dans l'entrée en mémoire cache.<br /><br /> 0x00000000 = L'adresse des données d'entrée n'est pas disponible.<br /><br /> N'accepte pas la valeur NULL.|  
|**in_use_count**|**int**|Nombre d'utilisateurs simultanés de cette entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**is_dirty**|**bit**|Indique si cette entrée du cache est marquée en vue d'une suppression. 1 = marquée pour la suppression. N'accepte pas la valeur NULL.|  
|**disk_ios_count**|**int**|Nombre d'E/S qui ont eu lieu pendant la création de cette entrée. N'accepte pas la valeur NULL.|  
|**context_switches_count**|**int**|Nombre de changements de contexte subis pendant la création de cette entrée. N'accepte pas la valeur NULL.|  
|**original_cost**|**int**|Coût initial de l'entrée. Cette valeur est une approximation du nombre d'entrées/sorties engagées, du coût des instructions processeur et de la quantité de mémoire utilisée par l'entrée Plus le coût est élevé, moins il y a de probabilités que l'élément soit supprimé de la mémoire cache. N'accepte pas la valeur NULL.|  
|**current_cost**|**int**|Coût actuel de l'entrée en mémoire cache. Cette valeur est mise à jour lors de la purge des entrées. Le coût actuel est réinitialisé à sa valeur d'origine lors de la réutilisation de l'entrée. N'accepte pas la valeur NULL.|  
|**memory_object_address**|**varbinary (8)**|Adresse de l'objet mémoire associé. Autorise la valeur NULL.|  
|**pages_allocated_count**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre de pages de 8 Ko pour stocker cette entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**pages_kb**|**bigint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Quantité de mémoire, en kilo-octets (Ko), utilisée par cette entrée du cache.  N'accepte pas la valeur NULL.|  
|**entry_data**|**nvarchar(2048)**|Représentation en série de l'entrée en cache. Ces informations sont dépendantes du magasin du cache. Autorise la valeur NULL.|  
|**pool_id**|**int**|**S’applique à** : [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] et versions ultérieures.<br /><br /> ID de pool de ressources associé à l'entrée. Autorise la valeur NULL.<br /><br /> not katmai|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations 

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 
  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  

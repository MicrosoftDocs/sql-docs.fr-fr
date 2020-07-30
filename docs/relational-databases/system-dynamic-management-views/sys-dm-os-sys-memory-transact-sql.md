---
title: sys. dm_os_sys_memory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_sys_memory
- sys.dm_os_sys_memory_TSQL
- sys.dm_os_sys_memory
- dm_os_sys_memory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_memory dynamic management view
ms.assetid: 1ca58814-1caa-44c1-b307-ff0bdcbbef62
author: CarlRabeler
ms.author: carlrab
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6e79399d5483b84d893a2b4d3943dfd51aec7de6
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87396759"
---
# <a name="sysdm_os_sys_memory-transact-sql"></a>sys.dm_os_sys_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Retourne des informations de mémoire du système d'exploitation.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est lié par et répond à des conditions de mémoire externe au niveau du système d'exploitation et des limites physiques du matériel sous-jacent. La détermination de l'état du système global est une partie importante de l'évaluation de l'utilisation de la mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys. dm_pdw_nodes_os_sys_memory**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**total_physical_memory_kb**|**bigint**|Taille totale de mémoire physique disponible pour le système d'exploitation, en kilo-octets (Ko).|  
|**available_physical_memory_kb**|**bigint**|Taille de mémoire physique disponible, en Ko.|  
|**total_page_file_kb**|**bigint**|Taille de la limite de validation signalée par le système d'exploitation, en Ko.|  
|**available_page_file_kb**|**bigint**|Quantité totale de fichier d’échange qui n’est pas utilisée, en Ko.|  
|**system_cache_kb**|**bigint**|Quantité totale de mémoire du cache du système, en Ko.|  
|**kernel_paged_pool_kb**|**bigint**|Quantité totale du pool de noyaux paginés, en Ko.|  
|**kernel_nonpaged_pool_kb**|**bigint**|Quantité totale du pool de noyaux non paginés, en Ko.|  
|**system_high_memory_signal_state**|**bit**|État de la notification de ressource de mémoire supérieure système. La valeur 1 indique que le signal de mémoire supérieure a été défini par Windows. Pour plus d’informations, consultez [CreateMemoryResourceNotification](https://go.microsoft.com/fwlink/?LinkId=82427) dans MSDN Library.|  
|**system_low_memory_signal_state**|**bit**|État de la notification de ressource de mémoire inférieure système. La valeur 1 indique que le signal de mémoire inférieure a été défini par Windows. Pour plus d’informations, consultez [CreateMemoryResourceNotification](https://go.microsoft.com/fwlink/?LinkId=82427) dans MSDN Library.|  
|**system_memory_state_desc**|**nvarchar (256)**|Description de l'état de la mémoire. Consultez le tableau ci-dessous.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ,[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
|Condition|Valeur|  
|---------------|-----------|  
|system_high_memory_signal_state = 1<br /><br /> et<br /><br /> system_low_memory_signal_state = 0|La mémoire physique disponible est élevée|  
|system_high_memory_signal_state = 0<br /><br /> et<br /><br /> system_low_memory_signal_state = 1|La mémoire physique disponible est faible|  
|system_high_memory_signal_state = 0<br /><br /> et<br /><br /> system_low_memory_signal_state = 0|L'utilisation de la mémoire physique est constante|  
|system_high_memory_signal_state = 1<br /><br /> et<br /><br /> system_low_memory_signal_state = 1|L'état de la mémoire physique est en cours de transition<br /><br /> Les signaux de mémoire supérieure et inférieure ne doivent jamais être activés en même temps. Toutefois, il peut arriver que les deux valeurs semblent être activées sur une application en mode utilisateur à la suite de modifications rapides au niveau du système d'exploitation. L'affichage des deux signaux activés sera interprété comme un état de transition.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  



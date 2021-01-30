---
description: sys.dm_io_pending_io_requests (Transact-SQL)
title: sys.dm_io_pending_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_pending_io_requests
- dm_io_pending_io_requests
- dm_io_pending_io_requests_TSQL
- sys.dm_io_pending_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_pending_io_requests dynamic management view
ms.assetid: d1fb46dd-5c74-4c04-9ecf-8934b1bedb5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a13fe62558b85ab0ba9448793fba797f3e388b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185032"
---
# <a name="sysdm_io_pending_io_requests-transact-sql"></a>sys.dm_io_pending_io_requests (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque requête d'entrée/sortie en attente dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_io_pending_io_requests**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**io_completion_request_address**|**varbinary (8)**|Adresse mémoire de la requête d'entrée/sortie. N'accepte pas la valeur NULL.|  
|**io_type**|**nvarchar(60)**|Type de requête d'entrée/sortie en attente. N'accepte pas la valeur NULL.|  
|**io_pending_ms_ticks**|**bigint**|À usage interne uniquement N'accepte pas la valeur NULL.| 
|**io_pending**|**int**|Indique si la requête d'entrée/sortie est en attente ou a été effectuée par Windows. Une requête d'entrée/sortie peut toujours être en attente même si Windows l'a effectuée mais que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas encore effectué un changement de contexte dans lequel il doit traiter la requête d'entrée/sortie et la supprimer de la liste. N'accepte pas la valeur NULL.|  
|**io_completion_routine_address**|**varbinary (8)**|Fonction interne à appeler lorsque la requête d'entrée/sortie est terminée. Autorise la valeur NULL.|  
|**io_user_data_address**|**varbinary (8)**|À usage interne uniquement Autorise la valeur NULL.|  
|**scheduler_address**|**varbinary (8)**|Planificateur sur lequel a été émis la requête d'entrée/sortie. Cette requête s'affiche dans la liste des entrées/sorties en attente du planificateur. Pour plus d’informations, consultez [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). N'accepte pas la valeur NULL.|  
|**io_handle**|**varbinary (8)**|Descripteur du fichier utilisé dans la requête d'entrée/sortie. Autorise la valeur NULL.|  
|**io_offset**|**bigint**|Déplacement de la requête d'entrée/sortie. N'accepte pas la valeur NULL.|  
|**io_handle_path**|**nvarchar (256)**| Chemin d’accès du fichier utilisé dans la requête d’e/s. Autorise la valeur NULL.|
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [I O fonctions et vues de gestion dynamique associées &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



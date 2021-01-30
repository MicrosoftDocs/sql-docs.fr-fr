---
description: sys.dm_os_child_instances (Transact-SQL)
title: sys.dm_os_child_instances (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_child_instances
- sys.dm_os_child_instances_TSQL
- dm_os_child_instances
- dm_os_child_instances_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server state information [SQL Server]
- sys.dm_os_child_instances dynamic management view
- monitoring server health
ms.assetid: 1bef3074-0ccc-48fa-8f3d-14f3d99df86b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 67e13b08aa1feddf49d91933942f93140c43dc44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184934"
---
# <a name="sysdm_os_child_instances-transact-sql"></a>sys.dm_os_child_instances (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque instance utilisateur qui a été créée à partir de l'instance de serveur parente.  
  
> **IMPORTANT !** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Les informations retournées par **sys.dm_os_child_instances** peuvent être utilisées pour déterminer l’état de chaque instance utilisateur (heart_beat) et pour obtenir le nom du canal (instance_pipe_name) qui peut être utilisé pour créer une connexion à l’instance utilisateur à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de sqlcmd. Vous pouvez vous connecter à une instance utilisateur uniquement lorsque cette dernière a été démarrée par un processus externe, tel qu'une application cliente. Les outils de gestion de SQL Server ne peuvent pas démarrer une instance utilisateur.  
  
> **Remarque :** Les instances utilisateur sont une fonctionnalité de [!INCLUDE[ssExpressEd11](../../includes/ssexpressed11-md.md)] uniquement.  
> 
> **Remarque** Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_child_instances**.  
  
|Colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|**owning_principal_name**|**nvarchar (256)**|Nom de l'utilisateur pour lequel cette instance utilisateur a été créée.|  
|owning_principal_sid|nvarchar(256)|SID (Security-Identifier) du principal propriétaire de cette instance utilisateur. Elle correspond au SID Windows.|  
|owning_principal_sid_binary|varbinary(85)|Version binaire du SID de l'utilisateur propriétaire de l'instance utilisateur.|  
|**instance_name**|**nvarchar(128)**|Nom de cette instance utilisateur.|  
|**instance_pipe_name**|**nvarchar(260)**|Lors de la création d'une instance utilisateur, un canal nommé est créé auquel les applications peuvent se connecter. Ce nom peut s'utiliser dans une chaîne de connexion pour se connecter à cette instance utilisateur.|  
|**os_process_id**|**Int**|Numéro du processus Windows pour cette instance utilisateur.|  
|**os_process_creation_date**|**Datetime**|Date et heure du dernier démarrage du processus de cette instance utilisateur.|  
|**heart_beat**|**nvarchar(5**|État actuel de cette instance utilisateur ; ALIVE ou DEAD.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur la vue de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
## <a name="see-also"></a>Voir aussi  
 [Instances d'utilisateur pour les non administrateurs](/previous-versions/sql/)  
  

---
description: sys.dm_os_cluster_nodes (Transact-SQL)
title: sys.dm_os_cluster_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes
- sys.dm_os_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_cluster_nodes dynamic management view
ms.assetid: 92fa804e-2d08-42c6-a36f-9791544b1d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5276063293d8356902f0afcbf741281bedc24f6d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101520"
---
# <a name="sysdm_os_cluster_nodes-transact-sql"></a>sys.dm_os_cluster_nodes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque nœud de la configuration de l'instance de cluster de basculement. Si l’instance actuelle est une instance de cluster de basculement, elle retourne la liste des nœuds sur lesquels cette instance de cluster de basculement (anciennement « Virtual Server ») a été définie. Si l'instance de serveur active n'est pas une instance de cluster de basculement, elle retourne un ensemble de lignes vides.  
  
> **Remarque :** Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_cluster_nodes**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**NodeName**|**sysname**|Nom d'un nœud de la configuration de l'instance de cluster de basculement (serveur virtuel) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|status|**int**|État du nœud dans une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance de cluster de basculement : 0, 1, 2, 3,-1. Pour plus d’informations, consultez [fonction GetClusterNodeState](/windows/win32/api/clusapi/nf-clusapi-getclusternodestate).|  
|status_description|**nvarchar(20**|Description de l'état du nœud de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = actif<br /><br /> 1 = inactif<br /><br /> 2 = en pause<br /><br /> 3 = en cours de jointure<br /><br /> -1 = inconnu|  
|is_current_owner|bit|1 signifie que ce nœud est le propriétaire actuel de la ressource du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="remarks"></a>Notes  
 Lorsque le clustering de basculement est activé, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être exécutée sur n'importe quel nœud du cluster de basculement faisant partie de la configuration de l'instance de cluster de basculement (serveur virtuel) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> **Remarque :** Cette vue remplace la fonction fn_virtualservernodes, qui sera dépréciée dans une version ultérieure.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise sys. dm_os_cluster_nodes pour retourner les nœuds d'une instance de serveur cluster.  
  
```  
SELECT NodeName, status, status_description, is_current_owner   
FROM sys.dm_os_cluster_nodes;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|NodeName|status|status_description|is_current_owner|  
|--------------|------------|-------------------------|------------------------|  
|node1|0|up|1|  
|node2|0|up|0|  
|Node3|1|Éteindre|0|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_cluster_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-properties-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [sys.fn_virtualservernodes &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-virtualservernodes-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  

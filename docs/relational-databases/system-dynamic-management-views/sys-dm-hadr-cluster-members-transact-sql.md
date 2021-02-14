---
description: sys.dm_hadr_cluster_members (Transact-SQL)
title: sys.dm_hadr_cluster_members (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster_members_TSQL
- sys.dm_hadr_cluster_members
- dm_hadr_cluster_members_TSQL
- dm_hadr_cluster_members
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_members catalog view
ms.assetid: feb20b3a-8835-41d3-9a1c-91d3117bc170
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb47ace512b9db4983a9b61ccbdb6db490545937
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354174"
---
# <a name="sysdm_hadr_cluster_members-transact-sql"></a>sys.dm_hadr_cluster_members (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Si le nœud WSFC qui héberge une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activée pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] possède un quorum WSFC, retourne une ligne pour chacun des membres qui constituent le quorum et l'état de chacun d'eux. Cela comprend tous les nœuds du cluster (retournés avec CLUSTER_ENUM_NODE type par la fonction **Clusterenum** ) et le disque ou le témoin de partage de fichiers, le cas échéant. La ligne retournée pour un membre donné contient des informations sur l'état de ce membre. Par exemple, pour un cluster à cinq nœuds avec quorum de nœud majoritaire dans lequel un nœud est défaillant, lorsque **sys.dm_hadr_cluster_members** est interrogé à partir d’une instance de serveur qui est activée pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] qui réside sur un nœud avec quorum, **sys.dm_hadr_cluster_members** reflète l’état du nœud descendant comme « NODE_DOWN ».  
  
 Si le nœud WSFC n'a aucun quorum, aucune ligne n'est retournée.  
  
 Utilisez cette vue de gestion dynamique pour répondre aux questions suivantes :  
  
-   Quels nœuds sont en cours d'exécution sur le cluster WSFC ?  
  
-   Combien d'échecs de plus le cluster WSFC peut-il tolérer avant de perdre le quorum dans le cas d'un nœud majoritaire ?  

 > [!TIP]
 > À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , cette vue de gestion dynamique prend en charge les instances de cluster de basculement Always on en plus des groupes de disponibilité Always on.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|Nom de membre, qui peut être un nom d'ordinateur, une lettre de lecteur ou un chemin d'accès de partage de fichiers.|  
|**member_type**|**tinyint**|Type du membre. Peut prendre une des valeurs suivantes :<br /><br /> 0 = Nœud WSFC<br /><br /> 1 = Disque témoin<br /><br /> 2 = Témoin de partage de fichiers<br /><br /> 3 = témoin Cloud|  
|**member_type_desc**|**nvarchar(50)**|Description de **MEMBER_TYPE**, parmi :<br /><br /> CLUSTER_NODE<br /><br /> DISK_WITNESS<br /><br /> FILE_SHARE_WITNESS<br /><br /> CLOUD_WITNESS|  
|**member_state**|**tinyint**|État du membre. Peut prendre une des valeurs suivantes :<br /><br /> 0 = Hors connexion<br /><br /> 1 = En ligne|  
|**member_state_desc**|**nvarchar(60)**|Description de **member_state**, parmi :<br /><br /> UP<br /><br /> INACTIF|  
|**number_of_quorum_votes**|**tinyint**|Nombre de votes de quorum détenus par ce membre de quorum. Pour les quorums Non majoritaire - Disque uniquement, cette valeur est par défaut 0. Pour les autres types de quorum, cette valeur est par défaut 1.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

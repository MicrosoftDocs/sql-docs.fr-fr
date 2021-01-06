---
title: Référence sur les objets système de groupe de disponibilité
description: Document de référence sur les différents objets système que vous pouvez utiliser quand vous travaillez avec des groupes de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 340fcfa12f3432a09821132f44e8d58672be8198
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640927"
---
# <a name="always-on-availability-group-system-object-reference"></a>Référence d’objet système du groupe de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
Cette rubrique sert de page de référence sur tous les différents objets système qui peuvent être utilisés lorsque vous travaillez avec des groupes de disponibilité Always On. 

## <a name="system-catalog-views"></a>Vues de catalogue système

| Vue de catalogue système | Description|
| :------ | :----------------------------- |
| [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | Contient une ligne pour chaque base de données de disponibilité sur l'instance de SQL Server qui héberge un réplica de disponibilité pour tout groupe de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC), que la base de données de copie locale ait déjà été attachée au groupe de disponibilité ou non. |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | Retourne une ligne pour chaque adresse IP associée à un écouteur du groupe de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC). |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | Pour chaque groupe de disponibilité Always On, retourne aucune ligne, ce qui indique qu'aucun nom réseau n'est associé au groupe de disponibilité, ou retourne une ligne pour chaque configuration d'écouteur de groupe de disponibilité dans le cluster de clustering de basculement Windows Server (WSFC).  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | Retourne une ligne pour chaque groupe de disponibilité pour lequel l'instance locale de SQL Server héberge un réplica de disponibilité. Chaque ligne contient une copie mise en cache des métadonnées du groupe de disponibilité. |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | Retourne une ligne pour chaque groupe de disponibilité Always On dans le clustering de basculement Windows Server (WSFC). Chaque ligne contient les métadonnées du groupe de disponibilité du cluster WSFC. |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | Retourne une ligne pour la liste de routage en lecture seule de chaque réplica de disponibilité d’un groupe de disponibilité Always On dans le cluster de basculement WSFC. |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | Retourne une ligne pour chacun des réplicas de disponibilité qui appartiennent à un groupe de disponibilité Always On dans le cluster de basculement WSFC. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Vues de gestion dynamique système


| Vue de gestion dynamique système | Description|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | Retourne une ligne pour chaque tentative de réparation de page automatique sur une base de données de disponibilité sur un réplica de disponibilité hébergé pour un groupe de disponibilité quelconque par l'instance de serveur.  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | Retourne une ligne pour chaque groupe de disponibilité Always On qui possède un réplica de disponibilité sur l'instance locale de SQL Server. Chaque ligne affiche les états qui définissent l'intégrité d'un groupe de disponibilité donné. |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | Retourne une ligne pour chaque réplica de disponibilité (indépendamment de l’état de jointure) des groupes de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC). |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | Retourne une ligne pour chaque réplica de disponibilité Always On (indépendamment de son état de jointure) de tous les groupes de disponibilité Always On (indépendamment de l'emplacement du réplica) dans le cluster de basculement Windows Server (WSFC). |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | Retourne une ligne pour chaque réplica local et une ligne pour chaque réplica distant dans le même groupe de disponibilité Always On qu'un réplica local. Chaque ligne contient des informations sur l'état d'un réplica donné. |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | Retourne une ligne qui expose le nom du cluster et les informations sur le quorum. |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | Retourne une ligne pour chacun des membres qui constituent le quorum et l’état de chacun d’eux. |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | Retourne une ligne pour chaque membre de cluster WSFC qui participe à la configuration du sous-réseau d'un groupe de disponibilité.  |
| [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | Retourne une ligne contenant les informations destinées à vous fournir l'analyse de l'intégrité des bases de données de disponibilité dans chaque groupe de disponibilité Always On sur le cluster de basculement Windows Server (WSFC).  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | Retourne une ligne pour chaque base de données qui participe à un groupe de disponibilité Always On pour lequel l'instance locale de SQL Server héberge un réplica de disponibilité. |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | Pour chaque instance de SQL Server qui héberge un réplica de disponibilité joint à son groupe de disponibilité Always On, retourne le nom du nœud WSFC (cluster de basculement Windows Server) qui héberge l’instance de serveur. |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | Affiche le mappage des groupes de disponibilité Always On auxquels l’instance active de SQL Server a été jointe à trois identificateurs uniques : un ID de groupe de disponibilité, un ID de ressource WSFC et un ID de groupe WSFC. |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | Retourne une ligne contenant les informations dynamiques d'état pour chaque écouteur TCP. |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>Fonctions système


| Fonction système | Description|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | Utilisé pour déterminer si le réplica actuel est le réplica principal par défaut. |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | Utilisé pour déterminer si le réplica actuel est le réplica de sauvegarde par défaut. |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | Utilisé pour mapper un réplica dans un groupe de disponibilité distribué au groupe de disponibilité local. |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  

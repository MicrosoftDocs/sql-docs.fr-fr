---
title: Superviser les groupes de disponibilité à l’aide de Transact-SQL (T-SQL)
description: Utilisez des instructions Transact-SQL SELECT pour superviser les groupes de disponibilité Always On ainsi que leurs réplicas et bases de données.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- dynamic management views [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], databases
- catalog views [SQL Server], AlwaysOn Availability Groups
ms.assetid: 881a34de-8461-4811-8c62-322bf7226bed
author: cawrites
ms.author: chadam
ms.openlocfilehash: d2bccd9808ad397d7cc83285577aea3483b253c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352666"
---
# <a name="monitor-availability-groups-transact-sql"></a>Surveiller des groupes de disponibilité (Transact-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Pour surveiller les groupes de disponibilité et les réplicas, ainsi que les bases de données associées à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fournit un ensemble d'affichages catalogue et de vues de gestion dynamique, et des propriétés de serveur. Au moyen d'instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] SELECT, vous pouvez utiliser les vues pour surveiller les groupes de disponibilité, ainsi que leurs réplicas et bases de données. Les informations retournées pour un groupe de disponibilité donné varient selon que vous êtes connecté à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica principal ou un réplica secondaire.  
  
> [!TIP]  
>  La plupart de ces vues peuvent être jointes à l'aide de leurs colonnes ID afin de retourner des informations émanant de plusieurs vues à l'aide d'une requête unique.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nécessitent l'autorisation VIEW ANY DEFINITION sur l'instance de serveur. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
##  <a name="monitoring-the-always-on-availability-groups-feature-on-a-server-instance"></a><a name="AoAgFeatureOnSI"></a> Surveillance de la fonctionnalité de groupes de disponibilité Always On sur une instance de serveur  
 Pour surveiller la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur une instance de serveur, utilisez la fonction intégrée suivante :  
  
 [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) (fonction)  
 Retourne des informations de propriété de serveur indiquant si [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est activé et, si tel est le cas, s'il a démarré sur l'instance de serveur.  
  
 **Noms des colonnes :** IsHadrEnabled, HadrManagerStatus  
  
##  <a name="monitoring-availability-groups-on-the-wsfc-cluster"></a><a name="WSFC"></a> Surveillance des groupes de disponibilité sur le cluster WSFC  
 Pour surveiller le cluster WSFC (clustering de basculement Windows Server) qui héberge une instance de serveur locale activée pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], utilisez les vues suivantes :  
  
 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
 Si le nœud de clustering de basculement Windows Server (WSFC) qui héberge une instance de SQL Server avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] activé dispose d’un quorum WSFC, **sys.dm_hadr_cluster** retourne une ligne qui expose le nom et les informations de cluster sur le quorum. Si le nœud WSFC n'a aucun quorum, aucune ligne n'est retournée.  
  
 **Noms de colonne :** cluster_name, quorum_type, quorum_type_desc, quorum_state, quorum_state_desc  
  
 [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
 Si le nœud WSFC qui héberge l’instance locale Always On de SQL Server possède un quorum WSFC, retourne une ligne pour chacun des membres qui constituent le quorum et l’état de chacun d’eux.  
  
 **Noms de colonne :** member_name, member_type, member_type_desc, member_state, member_state_desc, number_of_quorum_votes  
  
 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
 Retourne une ligne pour chaque membre qui participe à la configuration de sous-réseau d'un groupe de disponibilité. Vous pouvez utiliser cette vue de gestion dynamique pour valider l'adresse IP virtuelle de réseau qui est configurée pour chaque réplica de disponibilité.  
  
 **Noms de colonne :** member_name, network_subnet_ip, network_subnet_ipv4_mask, network_subnet_prefix_length, is_public, is_ipv4  
  
 **Clé primaire :** member_name + network_subnet_IP + network_subnet_prefix_length  
  
 [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
 Pour chaque instance de SQL Server qui héberge un réplica de disponibilité joint à son groupe de disponibilité Always On, retourne le nom du nœud WSFC (clustering de basculement Windows Server) qui héberge l’instance de serveur. Cette vue de gestion dynamique permet les utilisations suivantes :  
  
-   Cette vue de gestion dynamique est utile pour détecter un groupe de disponibilité avec plusieurs réplicas de disponibilité hébergés sur le même nœud WSFC, ce qui correspond à une configuration non prise en charge qui peut se produire après un basculement FCI si le groupe de disponibilité n'est pas correctement configuré.  
  
-   Lorsque plusieurs instances de SQL Server sont hébergées sur le même nœud WSFC, la DLL de ressource utilise cette vue de gestion dynamique pour déterminer l'instance de SQL Server à laquelle se connecter.  
  
 **Noms de colonne :** ag_resource_id, instance_name, node_name  
  
 [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
 Affiche le mappage des groupes de disponibilité Always On auxquels l’instance active de SQL Server a été jointe à trois identificateurs uniques : un ID de groupe de disponibilité, un ID de ressource WSFC et un ID de groupe WSFC. L'objectif de ce mappage est de gérer le scénario dans lequel la ressource/le groupe WSFC est renommé.  
  
 **Noms de colonne :** ag_name, ag_id, ag_resource_id, ag_group_id  
  
> [!NOTE]  
>  Consultez également **sys.dm_hadr_availability_replica_cluster_nodes** et **sys.dm_hadr_availability_replica_cluster_states** dans la section [Surveillance de réplicas de disponibilité](#AvReplicas) et **sys.availability_databases_cluster** et **sys.dm_hadr_database_replica_cluster_states** dans la section [Surveillance des bases de données de disponibilité](#AvDbs) dans la suite de cette rubrique.  
  
 Pour plus d’informations sur les clusters WSFC et [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md) et [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
##  <a name="monitoring-availability-groups"></a><a name="AvGroups"></a> Monitoring Availability Groups  
 Pour surveiller les groupes de disponibilité pour lesquels l'instance de serveur héberge un réplica de disponibilité, utilisez les vues suivantes :  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Retourne une ligne pour chaque groupe de disponibilité pour lequel l'instance locale d' [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] héberge un réplica de disponibilité. Chaque ligne contient une copie mise en cache des métadonnées du groupe de disponibilité.  
  
 **Noms de colonne :** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Retourne une ligne pour chaque groupe de disponibilité du cluster WSFC. Chaque ligne contient les métadonnées du groupe de disponibilité du cluster de clustering de basculement Windows Server (WSFC).  
  
 **Noms de colonne :** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Retourne une ligne pour chaque groupe de disponibilité qui possède un réplica de disponibilité sur l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Chaque ligne affiche les états qui définissent l'intégrité d'un groupe de disponibilité donné.  
  
 **Noms de colonne :** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health, synchronization_health_desc  
  
##  <a name="monitoring-availability-replicas"></a><a name="AvReplicas"></a> Surveillance de réplicas de disponibilité  
 Pour surveiller les réplicas de disponibilité, utilisez les vues et fonction système suivantes :  
  
 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
 Retourne une ligne pour chaque réplica de disponibilité dans chaque groupe de disponibilité pour lequel l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] héberge un réplica de disponibilité.  
  
 **Noms de colonne :** replica_id, group_id, replica_metadata_id, replica_server_name, owner_sid, endpoint_url, availability_mode, availability_mode_desc, failover_mode, failover_mode_desc, session_timeout, primary_role_allow_connections, primary_role_allow_connections_desc, secondary_role_allow_connections, secondary_role_allow_connections_desc, create_date, modify_date, backup_priority, read_only_routing_url  
  
 [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
 Retourne une ligne pour la liste de routage en lecture seule de chaque réplica de disponibilité d’un groupe de disponibilité Always On dans le cluster de basculement WSFC.  
  
 **Noms de colonnes :** replica_id, routing_priority, read_only_replica_id  
  
 [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
 Retourne une ligne pour chaque réplica de disponibilité (indépendamment de l’état de jointure) des groupes de disponibilité Always On dans le cluster de clustering de basculement Windows Server (WSFC).  
  
 **Noms de colonnes :** group_name, replica_server_name, node_name  
  
 [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
 Retourne une ligne pour chaque réplica (indépendamment de l’état de jointure) de tous les groupes de disponibilité Always On (indépendamment de l’emplacement du réplica) dans le cluster WSFC (clustering de basculement Windows Server).  
  
 **Noms de colonnes :** replica_id, replica_server_name, group_id, join_state, join_state_desc  
  
 [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
 Retourne une ligne montrant l'état de chaque réplica de disponibilité local et une ligne pour chaque réplica de disponibilité distant au sein du même groupe de disponibilité.  
  
 **Noms de colonnes :** replica_id, group_id, is_local, role, role_desc, operational_state, operational_state_desc, connected_state, connected_state_desc, recovery_health, recovery_health_desc, synchronization_health, synchronization_health_desc, last_connect_error_number, last_connect_error_description et last_connect_error_timestamp  
  
 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
 Détermine si le réplica actuel est le réplica de sauvegarde par défaut.  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs de performances pour les réplicas de disponibilité (l’objet de performances **SQLServer:Availability Replica**  ), consultez [SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="monitoring-availability-databases"></a><a name="AvDbs"></a> Surveillance des bases de données de disponibilité  
 Pour surveiller les bases de données de disponibilité, utilisez les vues suivantes :  
  
 [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
 Contient une seule ligne pour chaque base de données sur l’instance de SQL Server qui fait partie de tous les groupes de disponibilité Always On du cluster, que la base de données de copie locale ait déjà été jointe au groupe de disponibilité ou non.  
  
> [!NOTE]  
>  Lorsqu'une base de données est ajoutée à un groupe de disponibilité, la base de données primaire est automatiquement jointe au groupe. Les bases de données secondaires doivent être préparées sur chaque réplica secondaire avant de pouvoir être jointes au groupe de disponibilité.  
  
 **Noms de colonne :** group_id, group_database_id, database_name  
  
 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
 Contient une ligne par base de données dans l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si une base de données appartient à un réplica de disponibilité, la ligne de cette base de données affiche l'identifiant GUID du réplica et l'identificateur unique de la base de données au sein de son groupe de disponibilité.  
  
 **[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] Noms de colonne :** replica_id, group_database_id  
  
 [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
 Retourne une ligne pour chaque tentative de réparation de page automatique sur une base de données de disponibilité sur un réplica de disponibilité hébergé pour un groupe de disponibilité quelconque par l'instance de serveur. Cette vue contient des lignes pour les tentatives de réparation de page automatique les plus récentes sur une base de données primaire ou secondaire donnée, avec un maximum de 100 lignes par base de données. Dès qu'une base de données atteint le maximum, la ligne pour sa tentative de réparation de page automatique suivante remplace l'une des entrées existantes.  
  
 **Noms de colonne :** database_id, file_id, page_id, error_type, page_status, modification_time  
  
 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
 Retourne une ligne pour chaque base de données qui participe à un groupe de disponibilité pour lequel l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] héberge un réplica de disponibilité.  
  
 **Noms de colonne :** database_id, group_id, replica_id, group_database_id, is_local, synchronization_state, synchronization_state_desc, is_commit_participant, synchronization_health, synchronization_health_desc, database_state, database_state_desc, is_suspended, suspend_reason, suspend_reason_desc, recovery_lsn, truncation_lsn, last_sent_lsn, last_sent_time, last_received_lsn, last_received_time, last_hardened_lsn, last_hardened_time, last_redone_lsn, last_redone_time, log_send_queue_size, log_send_rate, redo_queue_size, redo_rate, filestream_send_rate, end_of_log_lsn, last_commit_lsn, last_commit_time, low_water_mark_for_ghosts  
  
 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
 Retourne une ligne qui contient des informations destinées à vous fournir un éclairage concernant l'intégrité des bases de données de disponibilité dans chaque groupe de disponibilité sur le cluster WSFC (clustering de basculement Windows Server). Cette vue de gestion dynamique est utile lors de la planification d'un basculement, ou en réponse à un basculement, ou encore pour découvrir quel réplica secondaire d'un groupe de disponibilité retarde la troncation du journal sur une base de données primaire particulière.  
  
 **Noms de colonne :** replica_id, group_database_id, database_name, is_failover_ready, is_pending_secondary_suspend, is_database_joined, recovery_lsn, truncation_lsn  
  
> [!NOTE]  
>  L'emplacement du réplica principal constitue la source d'autorité pour un groupe de disponibilité.  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs de performances [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour les bases de données de disponibilité (l’objet de performances **SQLServer:Database Replica** ), consultez [SQL Server, réplica de base de données](../../../relational-databases/performance-monitor/sql-server-database-replica.md). De plus, pour superviser l’activité des journaux des transactions sur des bases de données de disponibilité, utilisez les compteurs suivants de l’objet de performance **SQLServer:Databases** : **Temps d’attente de vidage du journal (ms)** , **Vidages du journal/s**, **Journaliser les absences dans le cache/s du pool**, **Journaliser les lectures du disque/s du pool** et **Journaliser les requêtes/s du pool**. Pour plus d’informations, voir [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md).  
  
##  <a name="monitoring-availability-group-listeners"></a><a name="AGlisteners"></a> Surveillance des écouteurs de groupe de disponibilité  
 Pour surveiller les écouteurs de groupe de disponibilité sur les sous-réseaux du cluster WSFC, utilisez les vues suivantes :  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Retourne une ligne pour chaque adresse IP virtuelle conforme actuellement en ligne pour un écouteur de groupe de disponibilité.  
  
 **Noms de colonne** : listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Pour un groupe de disponibilité donné, retourne soit zéro ligne pour indiquer qu'aucun nom réseau n'est associé au groupe de disponibilité, soit une ligne pour chaque configuration d'écouteur de groupe de disponibilité dans le cluster WSFC.  
  
 **Noms de colonne** : group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Retourne une ligne contenant les informations dynamiques d'état pour chaque écouteur TCP.  
  
 **Noms de colonne** : listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
 **Clé primaire :** listener_id  
  
 Pour plus d’informations sur les écouteurs de groupe de disponibilité, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Tâches de surveillance de groupes de disponibilité Always On :**  
  
-   [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)  
  
-   [Afficher les propriétés d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Afficher les propriétés d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Afficher les propriétés d’écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
 **Référence sur la surveillance de groupes de disponibilité Always On (Transact-SQL) :**  
  
-   [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
-   [sys.availability_group_listener_ip_addresses &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
  
-   [sys.availability_group_listeners &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
  
-   [sys.availability_databases_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
  
-   [sys.availability_groups &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_nodes &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
-   [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
-   [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_networks &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_instance_node_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
  
-   [sys.dm_hadr_name_id_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
  
-   [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
-   [sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
  
-   [sys.fn_hadr_backup_is_preferred_replica  &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Compteurs de performances Always On :**  
  
-   [SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)  
  
-   [SQL Server, réplica de base de données](../../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
-   [SQL Server, objet Databases](../../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 **Gestion basée sur des stratégies pour les groupes de disponibilité Always On**  
  
-   [Utiliser les stratégies Always On pour afficher l’intégrité d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Outils pour superviser les groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  

---
title: Voir les propriétés d’un écouteur de groupe de disponibilité
description: Explique comment afficher les propriétés d’un écouteur de groupe de disponibilité Always On à l’aide de SQL Server Management Studio, de Transact-SQL ou de PowerShell dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/11/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygrouplistenerproperties.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
ms.assetid: aca0d016-3228-40b8-bdc3-285ed6d9b280
author: cawrites
ms.author: chadam
ms.openlocfilehash: b3b77706425eae89bd74b4803859e4a5c029ad17
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641699"
---
# <a name="view-availability-group-listener-properties-sql-server"></a>Afficher les propriétés d’un écouteur de groupe de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher les propriétés d’un *écouteur de groupe de disponibilité* Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)] dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour afficher les propriétés d'un écouteur, procédez comme suit :**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance de serveur qui héberge un réplica de disponibilité du groupe de disponibilité dont vous souhaitez afficher l'écouteur. Cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Développez le nœud du groupe de disponibilité, puis développez le nœud **Écouteurs de groupe de disponibilité** .  
  
4.  Cliquez avec le bouton droit sur l’écouteur que vous voulez afficher, puis sélectionnez la commande **Propriétés** .  
  
5.  Cela ouvre la boîte de dialogue **Propriétés de l'écouteur du groupe disponibilité** . Pour plus d’informations, consultez [Propriétés de l’écouteur du groupe disponibilité (boîte de dialogue)](#AgListenerPropertiesDialog), plus loin dans cette rubrique.  
  
###  <a name="availability-group-listener-properties-dialog-box"></a><a name="AgListenerPropertiesDialog"></a> Propriétés de l’écouteur du groupe disponibilité (boîte de dialogue)  
 **Nom DNS de l'écouteur**  
 Nom réseau de l'écouteur du groupe de disponibilité.  
  
 **Port**  
 Port TCP utilisé par cet écouteur.  
  
> [!NOTE]  
>  Si vous êtes connecté au réplica principal, vous pouvez utiliser ce champ pour modifier le numéro de port de l'écouteur. Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
 **Mode réseau**  
 Indique le protocole TCP utilisé par l'écouteur, à savoir :  
  
 **DHCP**  
 L'écouteur utilise une adresse IP dynamique affectée par un serveur exécutant le protocole DHCP (Dynamic Host Configuration Protocol).  
  
 **Adresse IP statique**  
 L'écouteur utilise une ou plusieurs adresses IP statiques. Pour accéder aux différents sous-réseaux, un écouteur de groupe de disponibilité doit utiliser des adresses IP statiques.  
  
 La grille affiche chacun des sous-réseaux sur lesquels l'écouteur écoute, ainsi que l'adresse IP associée à chaque sous-réseau.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour afficher les propriétés d'un écouteur, procédez comme suit :**  
  
 Pour surveiller les écouteurs de groupe de disponibilité, utilisez les vues suivantes :  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Retourne une ligne pour chaque adresse IP virtuelle conforme actuellement en ligne pour un écouteur de groupe de disponibilité.  
  
 **Noms de colonne** : listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Pour un groupe de disponibilité donné, retourne soit zéro ligne pour indiquer qu'aucun nom réseau n'est associé au groupe de disponibilité, soit une ligne pour chaque configuration d'écouteur de groupe de disponibilité dans le cluster WSFC.  
  
 **Noms de colonne** : group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Retourne une ligne contenant les informations dynamiques d'état pour chaque écouteur TCP.  
  
 **Noms de colonne** : listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation de [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour surveiller votre environnement [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , consultez [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  

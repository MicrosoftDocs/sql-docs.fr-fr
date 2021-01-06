---
title: Utiliser le volet Détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité | Microsoft Docs
description: Découvrez comment utiliser SQL Server Management Studio pour surveiller et gérer des groupes de disponibilité Always On existants, des réplicas de disponibilité et des bases de données de disponibilité.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.OEdetails.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: 84affc47-40e0-43d9-855e-468967068c35
author: cawrites
ms.author: chadam
ms.openlocfilehash: e6b0c8547718c1a7580108427314a4a58f4ba0b5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641822"
---
# <a name="use-object-explorer-details-to-monitor-availability-groups"></a>Utiliser le volet Détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment utiliser le volet **Détails de l’Explorateur d’objets** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] pour surveiller et gérer des groupes de disponibilité Always On existants, des réplicas de disponibilité et des bases de données de disponibilité.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation du volet Détails de l’Explorateur d’objets, consultez [Volet Détails de l’Explorateur d’objets](../../../ssms/object/object-explorer-details-pane.md).  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Vous devez être connecté à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (instance de serveur) qui héberge le réplica principal ou un réplica secondaire.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour surveiller des groupes de disponibilité, des réplicas de disponibilité et des bases de données de disponibilité**  
  
1.  Dans le menu Affichage, cliquez sur **Détails de l'Explorateur d'objets** ou appuyez sur la touche **F7** .  
  
2.  Dans l'Explorateur d'objets, connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur laquelle vous voulez surveiller un groupe de disponibilité, puis cliquez sur le nom du serveur pour développer son arborescence.  
  
3.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
4.  Le volet **Détails de l'Explorateur d'objets** affiche chaque groupe de disponibilité pour lequel l'instance de serveur connectée héberge un réplica. Pour chaque groupe de disponibilité, la colonne **Instance de serveur (principal)** affiche le nom de l’instance de serveur qui héberge actuellement le réplica principal.  Pour afficher plus d'informations sur un groupe de disponibilité donné, sélectionnez-le dans l'Explorateur d'objets.  
  
5.  Le volet **Détails de l'Explorateur d'objets** affiche les nœuds **Réplicas de disponibilité** et **Bases de données de disponibilité** pour ce groupe de disponibilité :  
  
    -   Lorsque vous développez le nœud **Groupe de disponibilité** dans l'Explorateur d'objets et sélectionnez le nœud **Réplicas de disponibilité** , le volet **Détails de l'Explorateur d'objets** affiche des informations sur chacun des réplicas de disponibilité dans le groupe. Pour plus d'informations, consultez [Détails du réplica de disponibilité](#AvReplicaDetails), plus loin dans cette rubrique.  
  
         Pour effectuer des opérations sur plusieurs réplicas de disponibilité, sélectionnez-les et cliquez dessus avec le bouton droit pour ouvrir un menu contextuel qui répertorie les commandes disponibles.  
  
    -   Lorsque vous développez le nœud **Groupe de disponibilité** dans l'Explorateur d'objets et sélectionnez le nœud **Bases de données de disponibilité** , le volet **Détails de l'Explorateur d'objets** affiche des informations sur chacune des bases de données de disponibilité dans le groupe. Pour plus d'informations, consultez [Détails de la base de données de disponibilité](#AvDbDetails), plus loin dans cette rubrique.  
  
         Pour effectuer des opérations sur plusieurs bases de données de disponibilité, sélectionnez-les, et cliquez dessus avec le bouton droit pour ouvrir un menu contextuel qui répertorie les commandes disponibles.  
  
##  <a name="availability-groups-details"></a><a name="AvGroupsDetails"></a> Détails du groupe de disponibilité  
 L'écran détaillé **Groupes de disponibilité** affiche les colonnes suivantes :  
  
 **Nom**  
 Répertorie les dossiers des écouteurs **Réplicas de disponibilité**, **Bases de données de disponibilité** et **Groupe de disponibilité** du groupe de disponibilité sélectionné.  
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> Détails du réplica de disponibilité  
 L'écran détaillé **Réplica de disponibilité** affiche les colonnes suivantes :  
  
 **Instance de serveur**  
 Affiche le nom de l'instance de serveur qui héberge le réplica de disponibilité, avec une icône indiquant l'état actuel de la connexion de l'instance de serveur à l'instance de serveur locale.  
  
 **Rôle**  
 Indique le rôle actuel du réplica de disponibilité, à savoir **Principal** ou **Secondaire**. Pour plus d’informations sur les rôles des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 **Mode de connexion dans le rôle secondaire**  
 Indique si les bases de données d'un réplica de disponibilité donné qui joue le rôle secondaire (autrement dit, qui joue le rôle d'un réplica secondaire) peuvent accepter des connexions de clients.  
  
 Les valeurs possibles sont les suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Interdire les connexions**|Aucune connexion directe n'est autorisée aux bases de données de disponibilité lorsque ce réplica de disponibilité joue le rôle de réplica secondaire. Les bases de données secondaires ne sont pas disponibles pour l'accès en lecture.|  
|**Autoriser uniquement les connexions de tentatives de lecture**|Seules les connexions en lecture seule directes sont autorisées lorsque ce réplica agit comme réplica secondaire. Toutes les bases de données du réplica sont disponibles pour l'accès en lecture.|  
|**Autoriser toutes les connexions**|Toutes les connexions sont autorisées à ces bases de données pour l'accès en lecture seule lorsque ce réplica joue le rôle d'un réplica secondaire.|  
  
 **État de la connexion**  
 Indique si un réplica secondaire est actuellement connecté au réplica principal. Les valeurs possibles sont les suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Déconnecté**|Pour un réplica de disponibilité distant, indique qu'il est déconnecté du réplica de disponibilité local. La réponse du réplica local à l'état déconnecté dépend de son rôle, comme suit :<br /><br /> Sur le réplica principal, si un réplica secondaire est déconnecté, les bases de données secondaires sont marquées comme **Non synchronisé** sur le réplica principal, et le réplica principal attend que le réplica secondaire se reconnecte.<br /><br /> Une fois la déconnexion détectée, le réplica secondaire tente de se reconnecter au réplica principal.|  
|**Connecté**|Réplica de disponibilité distant qui est actuellement connecté au réplica local.|  
|**NULL**|Si le réplica local est un réplica secondaire, cette valeur est NULL pour les autres réplicas secondaires.|  
  
 **État de synchronisation**  
 Indique si un réplica secondaire est actuellement synchronisé avec le réplica principal. Les valeurs possibles sont les suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Non synchronisé**|La base de données n'est pas synchronisée ou n'a pas encore été jointe au groupe de disponibilité.|  
|**Synchronisée**|La base de données est synchronisée avec la base de données primaire sur le réplica principal actuel, le cas échéant, ou sur le dernier réplica principal.<br /><br /> Remarque : En mode de performances, la base de données n’est jamais à l’état Synchronisée.|  
|**NULL**|État inconnu. Cette valeur se produit lorsque l'instance de serveur locale ne peut pas communiquer avec le cluster de basculement WSFC (le nœud local ne fait pas partie du quorum WSFC).|  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs de performances pour les réplicas de disponibilité, consultez [SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="availability-database-details"></a><a name="AvDbDetails"></a> Détails de la base de données de disponibilité  
 L'écran détaillé **Base de données de disponibilité** affiche les propriétés suivantes des bases de données de disponibilité d'un groupe de disponibilité donné :  
  
 **Nom**  
 Nom de la base de données de disponibilité.  
  
 **État de synchronisation**  
 Indique si la base de données de disponibilité est actuellement synchronisée avec le réplica principal.  
  
 Les possibles états de synchronisation sont les suivants :  
  
|Valeur|Description|  
|-----------|-----------------|  
|Synchronisation|La base de données secondaire a reçu les enregistrements du journal des transactions de la base de données primaire qui ne sont pas encore écrits sur le disque (renforcé).<br /><br /> Remarque : En mode de validation asynchrone, l’état de synchronisation est toujours **Synchronisation**.|  
|||  
  
 **Suspendu**  
 Indique si la base de données de disponibilité est actuellement en ligne. Les valeurs possibles sont les suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Suspendu**|Cet état indique que la base de données est interrompue localement et doit être reprise manuellement.<br /><br /> Sur le réplica principal, la valeur est peu fiable pour une base de données secondaire. Pour déterminer de manière fiable si une base de données secondaire est interrompue, interrogez-la sur le réplica secondaire qui héberge la base de données.|  
|**Non jointe**|Indique que la base de données secondaire n'a pas été jointe au groupe de disponibilité ou a été supprimée du groupe.|  
|**En ligne**|Indique que la base de données n'est pas interrompue sur le réplica de disponibilité local et que la base de données est connectée.|  
|**Non connecté**|Indique que le réplica secondaire n'est pas actuellement en mesure de se connecter au réplica principal.|  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs de performances pour les bases de données de disponibilité, consultez [SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-database-replica.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Afficher les propriétés d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)   
 [Afficher les propriétés d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
  

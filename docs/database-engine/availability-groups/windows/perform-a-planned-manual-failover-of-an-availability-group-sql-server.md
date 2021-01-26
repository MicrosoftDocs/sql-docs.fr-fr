---
title: Effectuer un basculement manuel planifié d’un groupe de disponibilité
description: Cette rubrique explique comment effectuer un basculement manuel planifié d’un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 10/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.manualfailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 419f655d-3f9a-4e7d-90b9-f0bab47b3178
author: cawrites
ms.author: chadam
ms.openlocfilehash: 663861d552b619b199e6279353f5bbdcf6658f24
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783364"
---
# <a name="perform-a-planned-manual-failover-of-an-always-on-availability-group-sql-server"></a>Effectuer un basculement manuel planifié d’un groupe de disponibilité Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Cette rubrique explique comment effectuer un basculement manuel sans perte de données ( *basculement manuel planifié*) sur un groupe de disponibilité AlwaysOn à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Un groupe de disponibilité bascule au niveau d'un réplica de disponibilité. Un basculement manuel planifié, comme un basculement de groupe de disponibilité AlwaysOn, transfère le rôle principal à un réplica secondaire. En même temps, le basculement transfère l’ancien réplica principal sur le rôle secondaire.  
  
Un basculement manuel planifié est pris en charge seulement quand le réplica principal et le réplica secondaire cible s’exécutent en mode de validation synchrone et sont synchronisés. Il conserve toutes les données dans les bases de données secondaires qui sont jointes au groupe de disponibilité sur le réplica secondaire cible. Une fois que le réplica principal précédent transfère le rôle secondaire, ses bases de données deviennent des bases de données secondaires. Puis, elles commencent à se synchroniser avec les nouvelles bases de données primaires. Une fois que toutes ont passé à l'état SYNCHRONIZED, le nouveau réplica secondaire devient éligible pour servir de cible d'un futur basculement manuel planifié.  
  
> [!NOTE]  
>  Si les réplicas principal et secondaire sont tous les deux configurés pour le mode de basculement automatique, une fois que le réplica secondaire est synchronisé, il peut également servir de cible à un basculement automatique. Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer 

>[!IMPORTANT]
>Il existe des procédures spécifiques pour basculer un groupe de disponibilité avec échelle lecture sans gestionnaire de cluster. Quand un groupe de disponibilité a CLUSTER_TYPE = NONE, suivez les procédures sous [Basculer le réplica principal sur un groupe de disponibilité avec échelle lecture](#fail-over-the-primary-replica-on-a-read-scale-availability-group).

###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions 
  
- Une commande de basculement retourne dès que le réplica secondaire cible a accepté la commande. Toutefois, la récupération de la base de données est asynchrone après que le basculement du groupe de disponibilité est terminé. 
- La cohérence entre les bases de données sur plusieurs bases de données dans le groupe de disponibilité peut ne pas être conservée lors d’un basculement. 
  
    > [!NOTE] 
    >  La prise en charge des transactions distribuées et entre les bases de données varie selon les versions de système d’exploitation et SQL Server. Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
  
###  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prérequis et restrictions 
  
-   Le réplica secondaire cible et le réplica principal doivent tous les deux s’exécuter en mode de disponibilité avec validation synchrone. 
-   Le réplica secondaire cible doit être actuellement synchronisé avec le réplica principal. Toutes les bases de données secondaires de ce réplica secondaire doivent être jointes au groupe de disponibilité. Elles doivent également être synchronisées avec leurs bases de données primaires correspondantes (autrement dit, les bases de données secondaires locales doivent être SYNCHRONISÉES). 
  
    > [!TIP] 
    >  Pour déterminer si le basculement d’un réplica secondaire est prêt, interrogez la colonne **is_failover_ready** dans la vue de gestion dynamique [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md). Vous pouvez aussi consulter la colonne **Disponibilité du basculement** du [tableau de bord du groupe AlwaysOn](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md). 
-   Cette tâche est prise en charge uniquement sur le réplica secondaire cible. Vous devez être connecté à l'instance de serveur qui héberge le réplica secondaire cible. 
  
###  <a name="security"></a><a name="Security"></a> Sécurité 
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations 
 L’autorisation ALTER AVAILABILITY GROUP est obligatoire sur le groupe de disponibilité. L’autorisation CONTROL AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER sont également obligatoires. 
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utiliser SQL Server Management Studio 
 Pour basculer manuellement un groupe de disponibilité : 
  
1. Dans l’Explorateur d’objets, connectez-vous à une instance de serveur qui héberge un réplica secondaire du groupe de disponibilité qui doit être basculé. Développez l'arborescence du serveur. 
  
2. Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** . 
  
3. Cliquez avec le bouton droit sur le groupe de disponibilité à basculer et sélectionnez **Basculement**. 
  
4. L’Assistant Basculer le groupe de disponibilité commence. Pour plus d’informations, consultez [Utiliser l’Assistant Basculer le groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). 
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Utiliser Transact-SQL 
 Pour basculer manuellement un groupe de disponibilité : 
  
1. Connectez-vous à l'instance de serveur qui héberge le réplica secondaire cible. 
  
2. Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit : 
  
     ALTER AVAILABILITY GROUP *nom_groupe* FAILOVER 
  
     Dans l’instruction, *nom_groupe* correspond au nom du groupe de disponibilité. 
  
     L’exemple suivant montre un basculement manuel du groupe de disponibilité *MyAg* vers le réplica secondaire connecté : 
  
    ```  
    ALTER AVAILABILITY GROUP MyAg FAILOVER;  
    ```  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Utiliser PowerShell 
 Pour basculer manuellement un groupe de disponibilité : 
  
1. Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica secondaire cible. 
  
2. Utilisez l’applet de commande **Switch-SqlAvailabilityGroup** . 
  
    > [!NOTE] 
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] PowerShell. Pour plus d’informations, consultez [Obtenir de l’aide pour SQL Server PowerShell](../../../powershell/sql-server-powershell.md). 
  
     L’exemple suivant montre un basculement manuel du groupe de disponibilité *MyAg* vers le réplica secondaire dont le chemin est spécifié : 
  
    ```  
    Switch-SqlAvailabilityGroup -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg  
    ```  
  
    Pour configurer et utiliser le fournisseur SQL Server PowerShell : 
  
    -   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md) 
    -   [Obtenir de l’aide pour SQL Server PowerShell](../../../powershell/sql-server-powershell.md) 

##  <a name="follow-up-after-you-manually-fail-over-an-availability-group"></a><a name="FollowUp"></a> Suivi : Après avoir basculé manuellement un groupe de disponibilité 
 Si vous avez effectué le basculement en dehors de [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] du groupe de disponibilité, ajustez les votes de quorum des nœuds du clustering de basculement Windows Server afin de refléter la nouvelle configuration du groupe de disponibilité. Pour plus d’informations, consultez [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md). 

<a name = "ReadScaleOutOnly"><a/>

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Basculer le réplica principal sur un groupe de disponibilité avec échelle lecture

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="see-also"></a>Voir aussi 

 * [Vue d’ensemble des groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
 * [Basculement et modes de basculement &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 
 * [Effectuer un basculement manuel forcé d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) 
  

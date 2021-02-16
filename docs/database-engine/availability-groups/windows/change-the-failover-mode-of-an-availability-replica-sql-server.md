---
title: Changer le mode de basculement d’un réplica pour un groupe de disponibilité
description: Décrit comment changer le mode de basculement d’un réplica dans un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 06/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- failover modes [SQL Server]
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], failover modes
- Availability Groups [SQL Server], configuring
ms.assetid: 619a826f-8e65-48eb-8c34-39497d238279
author: cawrites
ms.author: chadam
ms.openlocfilehash: a5f75b9de94e5826035c047bf1e99d0b9655a131
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343809"
---
# <a name="change-the-failover-mode-for-a-replica-within-an-always-on-availability-group"></a>Changer le mode de basculement d’un réplica dans un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment modifier le mode de basculement d’un réplica de disponibilité dans un groupe de disponibilité Always On dans [!INCLUDE[ssnoverison](../../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell. Le mode de basculement est une propriété de réplica qui détermine le mode de basculement pour les réplicas qui s'exécutent en mode de disponibilité avec validation synchrone. Pour plus d’informations, consultez [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) et [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Conditions préalables requises et restrictions  
  
-   Cette tâche est prise en charge uniquement sur les réplicas principaux. Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
-   Les instances de cluster de basculement (FCI) SQL Server ne prennent pas en charge le basculement automatique par les groupes de disponibilité. Par conséquent, tout réplica de disponibilité hébergé par une instance de cluster de basculement ne peut être configuré que pour un basculement manuel.  
  

##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier le mode de basculement d'un réplica de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez modifier le réplica.  
  
4.  Cliquez avec le bouton droit sur le réplica, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du réplica de disponibilité** , utilisez la liste déroulante **Mode de basculement** pour modifier le mode de basculement de ce réplica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier le mode de basculement d'un réplica de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :

    ```syntaxsql
    ALTER AVAILABILITY GROUP *group_name* MODIFY REPLICA ON '*server_name*'  
       WITH ( {  
             AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }
                | FAILOVER_MODE = { AUTOMATIC | MANUAL }
             }  )
    ```
    
    Dans le script précédent :

    - *nom_groupe* correspond au nom du groupe de disponibilité.  
  
    - *server_name* est le nom d’ordinateur ou le nom réseau du cluster de basculement. Pour les instances nommées, ajoutez « \instance_name ». Utilisez le nom qui héberge le réplica que vous souhaitez modifier.
  
Pour plus d’informations sur ces paramètres, consultez [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
L’exemple suivant, entré sur le réplica principal du groupe de disponibilité *MyAG* , remplace le mode de basculement par le basculement automatique sur le réplica de disponibilité situé sur l’instance de serveur par défaut sur un ordinateur nommé *COMPUTER01*.  
  
```sql
ALTER AVAILABILITY GROUP MyAG MODIFY REPLICA ON 'COMPUTER01' WITH  
    (FAILOVER_MODE = AUTOMATIC);  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour modifier le mode de basculement d'un réplica de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Set-SqlAvailabilityReplica** avec le paramètre **FailoverMode** . En définissant un réplica en mode de basculement automatique, vous devrez peut-être utiliser le paramètre **AvailabilityMode** pour modifier le réplica afin qu’il utilise le mode de disponibilité avec validation synchrone.  
  
    Par exemple, la commande suivante modifie le réplica `MyReplica` dans le groupe de disponibilité `MyAg` afin qu'il utilise le mode de disponibilité avec validation synchrone et prenne en charge le basculement automatique.  
  
    ```powershell
    Set-SqlAvailabilityReplica -AvailabilityMode "SynchronousCommit" -FailoverMode "Automatic" `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\Replicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)  
  

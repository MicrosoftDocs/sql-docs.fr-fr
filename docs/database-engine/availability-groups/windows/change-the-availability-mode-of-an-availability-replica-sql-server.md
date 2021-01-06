---
title: Changer le mode de disponibilité d’un réplica pour un groupe de disponibilité
description: Explique comment changer le mode de disponibilité d’un réplica de disponibilité au sein d’un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), de PowerShell ou de SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], availability modes
ms.assetid: c4da8f25-fb1b-45a4-8bf2-195df6df634c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 295600860742757e5498a7cbf1fd18394282e87d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643125"
---
# <a name="change-availability-mode-of-a-replica-within-an-always-on-availability-group"></a>Changer le mode de disponibilité d’un réplica dans un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment modifier le mode de disponibilité d’un réplica de disponibilité dans un groupe de disponibilité Always On dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou PowerShell. Le mode de disponibilité est une propriété de réplica qui contrôle si le réplica effectue la validation de façon synchrone ou asynchrone. Le *mode de validation asynchrone* optimise les performances au détriment d’une haute disponibilité et prend en charge uniquement le basculement forcé manuel (avec une possible perte de données), qu’on appelle généralement *basculement forcé*. Le *mode de validation synchrone* privilégie la haute disponibilité plutôt que les performances et, une fois que le réplica secondaire est synchronisé, prend en charge le basculement manuel et, éventuellement, le basculement automatique.  
    
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  

##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier le mode de disponibilité d'un groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez modifier le réplica.  
  
4.  Cliquez avec le bouton droit sur le réplica, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du réplica de disponibilité** , utilisez la liste déroulante **Mode de disponibilité** pour modifier le mode de disponibilité de ce réplica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier le mode de disponibilité d'un groupe de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme l’exemple suivant :  
  
     ```sql
     ALTER AVAILABILITY GROUP *group_name* MODIFY REPLICA ON '*server_name*'  
     WITH ( AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT , FAILOVER_MODE = MANUAL );  
     ```
     
     où *group_name* est le nom du groupe de disponibilité et *server_name* est le nom de l’instance de serveur qui héberge le réplica à modifier.  
  
    > [!NOTE]  
    > `FAILOVER_MODE = AUTOMATIC` est pris en charge uniquement si vous spécifiez également `AVAILABILITY_MODE = SYNCHRONOUS_COMMIT`.  
  
     L'exemple suivant, écrit sur le réplica principal du groupe de disponibilité `AccountsAG` , modifie les modes de disponibilité et de basculement par une validation synchrone et un basculement automatique, pour le réplica hébergé par l'instance de serveur `INSTANCE09` .  
  
    ```sql
    ALTER AVAILABILITY GROUP AccountsAG MODIFY REPLICA ON 'INSTANCE09'  
       WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);  
    ALTER AVAILABILITY GROUP AccountsAG MODIFY REPLICA ON 'INSTANCE09'  
       WITH (FAILOVER_MODE = AUTOMATIC);  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour modifier le mode de disponibilité d'un groupe de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Set-SqlAvailabilityReplica** avec le paramètre **AvailabilityMode** et, éventuellement, le paramètre **FailoverMode** .  
  
     Par exemple, la commande suivante modifie le réplica `MyReplica` dans le groupe de disponibilité `MyAg` afin qu'il utilise le mode de disponibilité avec validation synchrone et prenne en charge le basculement automatique.  
  
    ```powershell  
    Set-SqlAvailabilityReplica -AvailabilityMode "SynchronousCommit" -FailoverMode "Automatic" `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    > Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)  
  

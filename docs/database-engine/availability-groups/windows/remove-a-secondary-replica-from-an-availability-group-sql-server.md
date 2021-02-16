---
title: Supprimer un réplica secondaire d’un groupe de disponibilité
description: 'Découvrez les étapes à suivre pour supprimer un réplica secondaire d’un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51211eafbc847a5ad7daae6fecc128a489116cd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344598"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>Supprimer un réplica secondaire d'un groupe de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment supprimer un réplica secondaire d’un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Cette tâche est prise en charge uniquement sur le réplica principal.    
-   Seul un réplica secondaire peut être supprimé d'un groupe de disponibilité.  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal du groupe de disponibilité.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour supprimer un réplica secondaire**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Sélectionnez le groupe de disponibilité, puis développez le nœud **Réplicas de disponibilité** .  
  
4.  Cette étape varie selon que vous souhaitez supprimer un seul ou plusieurs réplicas, comme suit :  
  
    -   Pour supprimer plusieurs réplicas, utilisez le volet **Détails de l'Explorateur d'objets** pour afficher et sélectionner tous les réplicas à supprimer. Pour plus d’informations, consultez [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Pour supprimer un seul réplica, sélectionnez-le dans le volet **Explorateur d'objets** ou le volet **Détails de l'Explorateur d'objets** .  
  
5.  Cliquez avec le bouton droit sur le réplica ou les réplicas secondaires sélectionnés, puis sélectionnez **Supprimer du groupe de disponibilité** dans le menu de commande.  
  
6.  Dans la boîte de dialogue **Supprimer les réplicas secondaires du groupe de disponibilité** , pour supprimer tous les réplicas secondaires répertoriés, cliquez sur **OK**. Si vous ne souhaitez pas supprimer tous les réplicas répertoriés, cliquez sur **Annuler**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour supprimer un réplica secondaire**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* REMOVE REPLICA ON '*nom_instance*' [,...*n*]  
  
     où *nom_groupe* est le nom du groupe de disponibilité et *nom_instance* est l'instance de serveur où se trouve le réplica secondaire.  
  
     L’exemple suivant supprime un réplica secondaire du groupe de disponibilité *MyAG* . Le réplica secondaire cible se trouve sur une instance de serveur nommée *HADR_INSTANCE* sur un ordinateur nommé *COMPUTER02*.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour supprimer un réplica secondaire**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Remove-SqlAvailabilityReplica** .  
  
     Par exemple, la commande suivante supprime le réplica de disponibilité sur le serveur `MyReplica` du groupe de disponibilité nommé `MyAg`.  Cette commande doit être exécutée sur l'instance de serveur qui héberge le réplica principal du groupe de disponibilité.  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> Suivi : Après avoir supprimé un réplica secondaire  
 Si vous spécifiez un réplica qui n'est pas disponible actuellement, lorsque le réplica est mis en ligne, on découvre qu'il a été supprimé.  
  
 La suppression d'un réplica provoque l'arrêt de la réception des données. Après qu'un réplica secondaire a confirmé qu'il a été supprimé du magasin global, le réplica supprime les paramètres de groupe de disponibilité de ses bases de données, lesquelles demeurent sur l'instance de serveur locale dans l'état RECOVERING.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [Supprimer un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  

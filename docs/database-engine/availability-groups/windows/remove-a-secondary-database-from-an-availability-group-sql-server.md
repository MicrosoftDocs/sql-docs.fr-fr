---
title: Supprimer une base de données secondaire d’un groupe de disponibilité
description: Découvrez les étapes à suivre pour supprimer une base de données secondaire d’un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.unjoindb.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], databases
ms.assetid: 4e51a570-58d7-4f01-9390-4198f3602576
author: cawrites
ms.author: chadam
ms.openlocfilehash: d6ad224d5c017fa8e8286e245af2e8ca3a96e4c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344562"
---
# <a name="remove-a-secondary-database-from-an-availability-group-sql-server"></a>Supprimer une base de données secondaire d'un groupe de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment supprimer une base de données secondaire d’un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
   
  
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Conditions préalables requises et restrictions  
  
-   Cette tâche est prise en charge sur les réplicas secondaires uniquement. Vous devez être connecté à l'instance de serveur qui héberge le réplica secondaire duquel la base de données doit être supprimée.  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour supprimer une base de données secondaire dans un groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica secondaire dont vous souhaitez supprimer une ou plusieurs bases de données secondaires, et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Sélectionnez le groupe de disponibilité, puis développez le nœud **Bases de données de disponibilité** .  
  
4.  Cette étape varie selon que vous souhaitez supprimer plusieurs groupes de bases de données ou une seule base de données, comme suit :  
  
    -   Pour supprimer plusieurs bases de données, utilisez le volet **Détails de l'Explorateur d'objets** pour afficher et sélectionner toutes les bases de données que vous souhaitez supprimer. Pour plus d’informations, consultez [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Pour supprimer une seule base de données, sélectionnez-la dans le volet **Explorateur d'objets** ou le volet **Détails de l'Explorateur d'objets** .  
  
5.  Cliquez avec le bouton droit sur les bases de données sélectionnées, puis sélectionnez **Supprimer la base de données secondaire** dans le menu de commande.  
  
6.  Dans la boîte de dialogue **Supprimer la base de données du groupe de disponibilité** , pour supprimer toutes les bases de données répertoriées, cliquez sur **OK**. Si vous ne souhaitez pas supprimer toutes les bases de données répertoriées, cliquez sur **Annuler**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour supprimer une base de données secondaire dans un groupe de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica secondaire.  
  
2.  Utilisez la [clause SET HADR de l'instruction ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) comme suit :  
  
     ALTER DATABASE *nom_base_de_données* SET HADR OFF  
  
     où *nom_base_de_données* est le nom d’une base de données secondaire à supprimer du groupe de disponibilité auquel elle appartient.  
  
     L’exemple suivant supprime la base de données secondaire locale *MyDb2* de son groupe de disponibilité.  
  
    ```  
    ALTER DATABASE MyDb2 SET HADR OFF;  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour supprimer une base de données secondaire dans un groupe de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica secondaire.  
  
2.  Utilisez l’applet de commande **Remove-SqlAvailabilityDatabase** , en spécifiant le nom de la base de données de disponibilité à supprimer du groupe de disponibilité. Lorsque vous êtes connecté à une instance de serveur qui héberge un réplica secondaire, seule la base de données secondaire locale est supprimée du groupe de disponibilité.  
  
     Par exemple, la commande suivante supprime la base de données secondaire `MyDb8` du réplica secondaire hébergé par l’instance de serveur nommée `SecondaryComputer\Instance`. La synchronisation de données avec les bases de données secondaires supprimées s'arrête. Cette commande n'affecte pas la base de données primaire ni aucune autre base de données secondaire.  
  
    ```  
    Remove-SqlAvailabilityDatabase `  
    -Path SQLSERVER:\Sql\SecondaryComputer\InstanceName\AvailabilityGroups\MyAg\Databases\MyDb8  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-database-from-an-availability-group"></a><a name="FollowUp"></a> Suivi : Après la suppression d'une base de données secondaire dans un groupe de disponibilité  
 Lorsqu'une base de données secondaire est supprimée, elle n'est plus jointe au groupe de disponibilité et toutes les informations relatives à la base de données secondaire supprimée sont ignorées par le groupe de disponibilité. La base de données secondaire supprimée est placée dans l'état RESTORING.  
  
> [!TIP]  
>  Pendant une courte période après la suppression d’une base de données secondaire, vous pouvez redémarrer la synchronisation des données Always On sur la base de données en la rejoignant au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 À ce stade, il existe d'autres méthodes pour traiter une base de données secondaire supprimée :  
  
-   Si vous n'avez plus besoin de la base de données secondaire, vous pouvez la supprimer.  
  
     Pour plus d’informations, consultez [DROP DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-transact-sql.md) ou [Supprimer une base de données](../../../relational-databases/databases/delete-a-database.md).  
  
-   Si vous souhaitez accéder à une base de données secondaire supprimée après sa suppression du groupe de disponibilité, vous pouvez récupérer la base de données. Toutefois, si vous récupérez une base de données secondaire supprimée, deux bases de données divergentes distinctes portant le même nom se trouvent alors en ligne. Vous devez vous assurer que les clients ne peuvent accéder qu'à la base de données primaire actuelle.  
  
     Pour plus d’informations, consultez [Récupérer une base de données sans restauration des données &#40;Transact-SQL&#41;](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Supprimer une base de données primaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  

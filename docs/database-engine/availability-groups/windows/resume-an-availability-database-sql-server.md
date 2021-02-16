---
title: Reprendre une base de données de groupe disponibilité
description: Reprenez une base de données de disponibilité interrompue dans les groupes de disponibilité Always On à l’aide de SQL Server Management Studio, de Transact-SQL ou de PowerShell dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.resumedatamove.f1
helpviewer_keywords:
- Availability Groups [SQL Server], resuming a database
- secondary databases [SQL Server], in availability group
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], databases
ms.assetid: 20e9147b-e985-4caa-910e-fc4b38dbf9a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: db200b1884b47ddc684646f529958095163d903a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344530"
---
# <a name="resume-an-availability-database-sql-server"></a>Reprendre une base de données de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Vous pouvez reprendre une base de données de disponibilité interrompue dans [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. La reprise d'une base de données interrompue met la base de données dans l'état SYNCHRONIZING. La reprise de la base de données primaire rétablit également toutes ses bases de données secondaires qui ont été interrompues suite à l'interruption de la base de données primaire. Si une base de données secondaire a été interrompue localement, depuis l'instance de serveur qui héberge le réplica secondaire, cette base de données secondaire doit être reprise localement. Une fois qu'une base de données secondaire particulière et que la base de données principale correspondante sont dans l'état SYNCHRONIZING, la synchronisation des données reprend sur la base de données secondaire.  
  
> [!NOTE]  
>  La suspension et la reprise d’une base de données secondaire Always On n’affectent pas directement la disponibilité de la base de données primaire. Toutefois, l'interruption d'une base de données secondaire peut avoir un impact sur la redondance et les fonctions de basculement de la base de données primaire, jusqu'à ce que la base de données secondaire interrompue reprenne. Ce comportement diffère de la mise en miroir de bases de données dans laquelle l'état de mise en miroir est interrompu à la fois sur la base de données miroir et la base de données principale tant que la mise en miroir n'a pas repris. L’interruption d’une base de données principale Always On interrompt le déplacement des données sur toutes les bases de données secondaires correspondantes, et les capacités de redondance et de basculement cessent pour cette base de données tant que la base de données principale n’a pas repris.  
  
  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Une commande RESUME retourne dès qu'elle est acceptée par le réplica qui héberge la base de données cible, mais en réalité, la reprise de la base de données se produit de façon asynchrone.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge la base de données à reprendre.    
-   Le groupe de disponibilité doit être en ligne.    
-   La base de données primaire doit être en ligne et disponible.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour reprendre une base de données secondaire**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica de disponibilité sur lequel vous souhaitez reprendre une base de données et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Développez le groupe de disponibilité.  
  
4.  Développez le nœud **Bases de données de disponibilité** , cliquez avec le bouton droit sur la base de données, puis sélectionnez **Reprendre le déplacement des données**.  
  
5.  Dans la boîte de dialogue **Reprendre le déplacement des données** , cliquez sur **OK**.  
  
> [!NOTE]  
>  Pour reprendre des bases de données supplémentaires sur cet emplacement de réplica, répétez les étapes 4 et 5 pour chaque base de données.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour reprendre une base de données secondaire qui a été suspendue localement**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica secondaire dont vous souhaitez reprendre la base de données.  
  
2.  Reprenez la base de données secondaire à l’aide de l’instruction [ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) suivante :  

     ALTER DATABASE *nom_base_de_données* SET HADR RESUME;
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour reprendre une base de données secondaire**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica dont vous souhaitez reprendre la base de données. Pour plus d'informations, consultez [Conditions préalables requises](#Prerequisites), plus haut dans cette rubrique.  
  
2.  Utilisez l’applet de commande **Resume-SqlAvailabilityDatabase** pour reprendre le groupe de disponibilité.  
  
     Par exemple, la commande suivante reprend la synchronisation des données pour la base de données de disponibilité `MyDb3` dans le groupe de disponibilité `MyAg`.  
  
    ```  
    Resume-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\Databases\MyDb3  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Interrompre une base de données de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  

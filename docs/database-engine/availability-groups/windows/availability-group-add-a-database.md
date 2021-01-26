---
title: Ajouter une base de données à un groupe de disponibilité
description: 'Ajoutez une base de données à un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 2a54eef8-9e8e-4e04-909c-6970112d55cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2f40156f8a7677de1c80fcc5094ee42b5527682
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766052"
---
# <a name="add-a-database-to-an-always-on-availability-group"></a>Ajouter une base de données à un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment ajouter une base de données à un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  

  
## <a name="prerequisites-and-restrictions"></a>Conditions préalables requises et restrictions  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
-   La base de données doit résider sur l'instance de serveur qui héberge le réplica principal et se conformer aux conditions préalables requises et aux restrictions applicables aux bases de données de disponibilité. Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utiliser SQL Server Management Studio  

  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez avec le bouton droit sur le groupe de disponibilité, puis sélectionnez l'une des commandes suivantes :  
  
    -   Pour lancer l'Assistant Ajouter une base de données au groupe de disponibilité, sélectionnez la commande **Ajouter une base de données** . Pour plus d’informations, consultez [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
    -   Pour ajouter une ou plusieurs bases de données en les spécifiant dans la boîte de dialogue **Propriétés du groupe de disponibilité** , sélectionnez la commande **Propriétés** . Les étapes d'ajout d'une base de données sont les suivantes :  
  
        1.  Dans le volet **Bases de données de disponibilité** , cliquez sur le bouton **Ajouter** . Cela permet de créer et de sélectionner un champ de base de données vide.  
  
        2.  Entrez le nom d'une base de données répondant aux exigences requises applicables aux bases de données de disponibilité.  
  
         Pour ajouter une autre base de données, répétez les étapes précédentes. Lorsque vous avez terminé la spécification des bases de données, cliquez sur **OK** pour terminer l'opération.  
  
         Après avoir utilisé la boîte de dialogue **Propriétés du groupe de disponibilité** pour ajouter une base de données à un groupe de disponibilité, vous devez configurer la base de données secondaire correspondante sur chaque instance de serveur qui héberge un réplica secondaire. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Utiliser Transact-SQL  

  
1.  Connectez-vous à l'instance de serveur qui héberge l'instance de serveur qui héberge le réplica principal.    
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* ADD DATABASE *nom_base_de_données* [,...*n*]  
  
     où *nom_groupe* est le nom du groupe de disponibilité et *nom_base_de_données* est le nom d’une base de données à ajouter au groupe.  
  
     L’exemple suivant ajoute la base de données *MyDb3* au groupe de disponibilité *MyAG* .  
  
    ```  
    -- Connect to the server instance that hosts the primary replica.  
    -- Add an existing database to the availability group.  
    ALTER AVAILABILITY GROUP MyAG ADD DATABASE MyDb3;  
    GO  
    ```  
  
3.  Après avoir ajouté une base de données à un groupe de disponibilité, vous devez configurer la base de données secondaire correspondante sur chaque instance de serveur qui héberge un réplica secondaire. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Utiliser PowerShell  

  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Add-SqlAvailabilityDatabase** .  
  
     Par exemple, la commande suivante ajoute la base de données secondaire `MyDd` au groupe de disponibilité `MyAG` , dont le réplica principal est hébergé par `PrimaryServer\InstanceName`.  
  
    ```  
  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "MyDb"  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
3.  Après avoir ajouté une base de données à un groupe de disponibilité, vous devez configurer la base de données secondaire correspondante sur chaque instance de serveur qui héberge un réplica secondaire. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
 Pour obtenir un exemple complet, consultez [Exemple (PowerShell)](#PSExample)ci-dessous.  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Exemple (PowerShell)  
 L'exemple suivant illustre le processus complet de préparation d'une base de données secondaire à partir d'une base de données présente sur l'instance de serveur qui héberge le réplica principal d'un groupe de disponibilité, qui consiste à ajouter la base de données à un groupe de disponibilité (en tant que base de données primaire) puis à joindre la base de données secondaire au groupe de disponibilité. Dans cet exemple, la base de données et son journal de transactions sont d'abord sauvegardés. Les sauvegardes de la base de données et du journal sont ensuite restaurées sur les instances de serveur qui hébergent un réplica secondaire.  
  
 Dans cet exemple, l’applet de commande **Add-SqlAvailabilityDatabase** est appelée deux fois : d’abord sur le réplica principal pour ajouter la base de données au groupe de disponibilité, puis sur le réplica secondaire pour joindre la base de données secondaire présente sur ce réplica au groupe de disponibilité. Si vous avez plusieurs réplicas secondaires, restaurez et joignez la base de données secondaire sur chacun d'eux.  
  
```  
$DatabaseBackupFile = "\\share\backups\MyDatabase.bak"  
$LogBackupFile = "\\share\backups\MyDatabase.trn"  
$MyAgPrimaryPath = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
$MyAgSecondaryPath = "SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAg"  
  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "PrimaryServer\InstanceName"  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "PrimaryServer\InstanceName" -BackupAction 'Log'  
  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "SecondaryServer\InstanceName" -NoRecovery  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "SecondaryServer\InstanceName" -RestoreAction 'Log' -NoRecovery  
  
Add-SqlAvailabilityDatabase -Path $MyAgPrimaryPath -Database "MyDatabase"  
Add-SqlAvailabilityDatabase -Path $MyAgSecondaryPath -Database "MyDatabase"  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)
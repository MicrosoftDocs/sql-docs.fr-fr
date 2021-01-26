---
title: Préparer une base de données secondaire pour un groupe de disponibilité
description: Décrit comment préparer manuellement une base de données secondaire pour la joindre à un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 07/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.preparedbs.f1
- sql13.swb.availabilitygroup.configsecondarydbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 9f2feb3c-ea9b-4992-8202-2aeed4f9a6dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3a87c7b4dc37a7b1a70b31b9d7b9191a3570db98
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764758"
---
# <a name="prepare-a-secondary-database-for-an-always-on-availability-group"></a>Préparer une base de données secondaire pour un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Cette rubrique explique comment préparer une base de données pour un groupe de disponibilité Always On dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell. La préparation d’une base de données comporte deux étapes : 

1. Restaurez une sauvegarde de base de données récente de la base de données principale ainsi que les sauvegardes de fichier journal ultérieures à l’aide de RESTORE WITH NORECOVERY sur chaque instance de serveur qui héberge le réplica secondaire
2. Joignez la base de données restaurée au groupe de disponibilité.  
  
> [!TIP]  
>  Si vous avez une configuration de copie des journaux de transaction, vous pouvez peut-être convertir la base de données principale de copie des journaux de transaction ainsi qu’une ou plusieurs de ses bases de données secondaires en réplica principal de groupe de disponibilité et en un ou plusieurs réplicas secondaires. Pour plus d'informations, consultez [Prérequis pour la migration de la copie des journaux de transaction vers les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  

##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Prérequis et restrictions  
  
-   Assurez-vous que le système dans lequel vous envisagez de placer la base de données possède un lecteur de disque avec suffisamment d'espace pour les bases de données secondaires.  
  
-   Le nom de la base de données secondaire doit être identique au nom de la base de données primaire.  
  
-   Utilisez RESTORE WITH NORECOVERY pour chaque opération de restauration.  
  
-   Si la base de données secondaire doit résider sur un chemin d'accès de fichier différent (lettre de lecteur incluse) de celui de la base de données primaire, la commande de restauration doit également utiliser l'option WITH MOVE pour chacun des fichiers de base de données afin de les spécifier au chemin d'accès de la base de données secondaire.  
  
-   Si vous restaurez le groupe de fichiers de base de données par groupe de fichiers, veillez à restaurer l'intégralité de la base de données.  
  
-   Après la restauration de la base de données, vous devez restaurer (WITH NORECOVERY) chaque sauvegarde de journal créée depuis la dernière sauvegarde de données restaurée.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Sur les instances autonomes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], nous recommandons, si possible, que le chemin d'accès au fichier (y compris la lettre de lecteur) d'une base de données secondaire particulière soit identique au chemin d'accès de la base de données primaire correspondante. Cela est dû au fait que, si vous déplacez les fichiers de base de données lors de la création d'une base de données secondaire, une opération ultérieure d'ajout de fichier peut échouer sur la base de données secondaire et provoquer l'interruption de la base de données secondaire.  
  
-   Avant de préparer vos bases de données secondaires, il est fortement recommandé d'interrompre les sauvegardes de fichiers journaux planifiées sur les bases de données dans le groupe de disponibilité jusqu'à ce que l'initialisation des réplicas secondaires soit terminée.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Lorsqu'une base de données est sauvegardée, la valeur OFF est attribuée à la propriété [TRUSTWORTHY](../../../relational-databases/security/trustworthy-database-property.md) de la base de données. Par conséquent, la propriété TRUSTWORTHY d'une base de données nouvellement restaurée a toujours la valeur OFF.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** . Pour plus d’informations, consultez [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md).  
  
 Lorsque la base de données en cours de restauration n'existe pas dans l'instance de serveur, l'instruction RESTORE nécessite des autorisations CREATE DATABASE. Pour plus d’informations, consultez [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md).  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utiliser SQL Server Management Studio  
  
> [!NOTE]  
>  Si les chemins de fichier de sauvegarde et de restauration sont identiques entre l’instance de serveur qui héberge le réplica principal et chaque instance qui héberge un réplica secondaire, vous devez pouvoir créer des bases de données de réplica secondaire à l’aide de l’[Assistant Nouveau groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), de l’[Assistant Ajouter un réplica au groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md) ou de l’[Assistant Ajouter une base de données au groupe de disponibilité](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
 **Pour préparer une base de données secondaire**  
  
1.  À moins d'avoir déjà une sauvegarde de base de données récente de la base de données primaire, créez une nouvelle sauvegarde complète ou différentielle de base de données. À titre de recommandation, placez cette sauvegarde et toutes les sauvegardes du journal réalisées ultérieurement sur le partage réseau recommandé.  
  
2.  Créez au moins une nouvelle sauvegarde du journal de la base de données primaire.

   >[!NOTE]
   >Une sauvegarde du journal des transactions peut ne pas être nécessaire si une sauvegarde du journal des transactions n’a pas été précédemment capturée sur la base de données dans le réplica principal. Microsoft vous recommande d’effectuer une sauvegarde du journal des transactions chaque fois qu’une nouvelle base de données est jointe au groupe de disponibilité. 
  
3.  Sur l'instance de serveur qui héberge le réplica secondaire, restaurez la sauvegarde complète de la base de données primaire (et éventuellement une sauvegarde différentielle) suivies de toutes les sauvegardes de journaux suivantes.  
  
     Dans la page **Options de RESTORE DATABASE**, sélectionnez **Laisser la base de données non opérationnelle, et ne pas restaurer les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. (RESTORE WITH NORECOVERY)** .  
  
     Si les chemins d'accès de fichier de la base de données primaire et de la base de données secondaire diffèrent, par exemple, si la base de données primaire se trouve sur le lecteur « F: », mais que l'instance de serveur qui héberge le réplica secondaire ne dispose pas de lecteur F:, incluez l'option MOVE dans votre clause WITH.  
  
4.  Pour terminer la configuration de la base de données secondaire, vous devez attacher la base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’exécution de ces opérations de sauvegarde et de restauration, consultez [Tâches connexes de sauvegarde et de restauration](#RelatedTasks), plus loin dans cette section.  
  
###  <a name="related-backup-and-restore-tasks"></a><a name="RelatedTasks"></a> Tâches connexes de sauvegarde et de restauration  
 **Pour créer une sauvegarde de base de données**  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 **Pour créer une sauvegarde du journal**  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **Pour restaurer des sauvegardes**  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour préparer une base de données secondaire**  
  
> [!NOTE]  
>  Pour obtenir un exemple de cette procédure, consultez [Exemple (Transact-SQL)](#ExampleTsql), plus haut dans cette rubrique.  
  
1.  À moins de disposer d'une sauvegarde complète récente de la base de données primaire, connectez-vous à l'instance de serveur qui héberge le réplica principal et créez une sauvegarde complète de base de données. À titre de recommandation, placez cette sauvegarde et toutes les sauvegardes du journal réalisées ultérieurement sur le partage réseau recommandé.  
  
2.  Sur l'instance de serveur qui héberge le réplica secondaire, restaurez la sauvegarde complète de la base de données primaire (et éventuellement une sauvegarde différentielle) suivies de toutes les sauvegardes de journaux suivantes. Utilisez WITH NORECOVERY pour chaque opération de restauration.  
  
     Si les chemins d'accès de fichier de la base de données primaire et de la base de données secondaire diffèrent, par exemple, si la base de données primaire se trouve sur le lecteur « F: », mais que l'instance de serveur qui héberge le réplica secondaire ne dispose pas de lecteur F:, incluez l'option MOVE dans votre clause WITH.  
  
3.  Si des sauvegardes de journal ont été effectuées sur la base de données primaire après la sauvegarde du journal requise, vous devez également les copier sur l'instance de serveur qui héberge le réplica secondaire et appliquer chacune de ces sauvegardes de journal à la base de données secondaire, en commençant par la première et en utilisant systématiquement RESTORE WITH NORECOVERY.  
  
    > [!NOTE]  
    >  Une sauvegarde du journal n'existerait pas si la base de données primaire venait d'être créée et qu'aucune sauvegarde du journal n'avait encore été réalisée ou que le mode de récupération venait d'être modifié de simple à complet.  
  
4.  Pour terminer la configuration de la base de données secondaire, vous devez attacher la base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’exécution de ces opérations de sauvegarde et de restauration, consultez [Tâches connexes de sauvegarde et de restauration](#RelatedTasks), plus loin dans cette rubrique.  
  
###  <a name="transact-sql-example"></a><a name="ExampleTsql"></a> Exemple Transact-SQL  
 L'exemple suivant prépare une base de données secondaire. L'exemple suivant utilise la base de données d'exemple [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] qui emploie par défaut le mode de récupération simple.  
  
1.  Pour utiliser la base de données [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] , modifiez-la afin qu'elle utilise le mode de récupération complète :  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE MyDB1   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Après avoir changé le mode de récupération de la base de données de SIMPLE à FULL, créez une sauvegarde complète qui pourra être utilisée pour créer la base de données secondaire. Dans la mesure où le mode de récupération vient d'être modifié, l'option WITH FORMAT est spécifiée pour créer un nouveau support de sauvegarde. Il peut se révéler utile de séparer les sauvegardes effectuées en mode de récupération complète des sauvegardes préalablement effectuées en mode de récupération simple. Pour les besoins de cet exemple, le fichier de sauvegarde (C:\\[!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].bak) est créé sur le même lecteur que la base de données.  
  
    > [!NOTE]  
    >  Dans le cas d'une base de données de production, il est conseillé de toujours effectuer les sauvegardes sur une unité distincte.  
  
     Sur l'instance de serveur qui héberge le réplica principal (`INSTANCE01`), créez une sauvegarde complète de la base de données primaire comme suit :  
  
    ```  
    BACKUP DATABASE MyDB1   
        TO DISK = 'C:\MyDB1.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copiez la sauvegarde complète sur l'instance de serveur qui héberge le réplica secondaire.  
  
4.  Restaurez la sauvegarde complète, à l'aide de RESTORE WITH NORECOVERY, sur l'instance de serveur qui héberge le réplica secondaire. La commande de restauration varie selon que les chemins d'accès des bases de données primaire et secondaire sont identiques ou non.  
  
    -   **Si les chemins d'accès sont identiques :**  
  
         Sur l'ordinateur qui héberge le réplica secondaire, restaurez la sauvegarde complète comme suit :  
  
        ```  
        RESTORE DATABASE MyDB1   
            FROM DISK = 'C:\MyDB1.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Si les chemins d'accès sont différents :**  
  
         Si le chemin d'accès de la base de données secondaire n'est pas le même que celui de la base de données primaire (lettres de lecteurs différentes, par exemple), la création de la base de données secondaire requiert l'intégration d'une clause MOVE dans l'opération de restauration.  
  
        > [!IMPORTANT]  
        >  Si les chemins d'accès des bases de données primaire et secondaire diffèrent, vous ne pouvez pas ajouter de fichier. La raison tient à la réception du journal pour l'opération d'ajout de fichier, puisque l'instance de serveur du réplica secondaire tente de placer le nouveau fichier dans le même chemin d'accès que celui utilisé par la base de données primaire.  
  
         Par exemple, la commande ci-dessous restaure une sauvegarde d'une base de données primaire qui réside dans le répertoire de données de l'instance par défaut de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA. L’opération de restauration de la base de données doit déplacer la base de données dans le répertoire de données d’une instance distante de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] nommée (*Always On1*), qui héberge le réplica secondaire sur un autre nœud de cluster. Là, les données et les fichiers journaux sont restaurés dans le répertoire *C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA* . L'opération de restauration utilise WITH NORECOVERY, afin de laisser la base de données secondaire dans la base de données de restauration.  
  
        ```  
        RESTORE DATABASE MyDB1  
          FROM DISK='C:\MyDB1.bak'  
         WITH NORECOVERY,   
            MOVE 'MyDB1_Data' TO   
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.mdf',   
            MOVE 'MyDB1_Log' TO  
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.ldf';  
        GO  
        ```  
  
5.  Après avoir restauré la sauvegarde complète, vous devez créer une sauvegarde du journal sur la base de données primaire. Par exemple, l’instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante sauvegarde le journal dans un fichier de sauvegarde nommé *E:\MyDB1_log.trn* :  
  
    ```  
    BACKUP LOG MyDB1   
      TO DISK = 'E:\MyDB1_log.trn'   
    GO  
    ```  
  
6.  Avant de pouvoir joindre la base de données au réplica secondaire, vous devez appliquer la sauvegarde du journal requise (et toutes les sauvegardes de journal ultérieures).  
  
     Par exemple, l’instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante restaure le premier journal à partir de *C:\MyDB1.trn* :  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Si des sauvegardes de journal supplémentaires se produisent avant la jointure de la base de données au réplica secondaire, vous devez également restaurer toutes ces sauvegardes de journal, dans l'ordre, dans l'instance de serveur qui héberge le réplica secondaire à l'aide de RESTORE WITH NORECOVERY.  
  
     Par exemple, l’instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante restaure deux journaux supplémentaires à partir de *E:\MyDB1_log.trn* :  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour préparer une base de données secondaire**  
  
1.  Si vous devez créer une sauvegarde récente de la base de données primaire, remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Backup-SqlDatabase** pour créer chacune des sauvegardes.  
  
3.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica secondaire.  
  
4.  Pour restaurer la base de données et les sauvegardes de journaux de chaque base de données primaire, utilisez l’applet de commande **restore-SqlDatabase** , en spécifiant le paramètre de restauration **NoRecovery** . Si les chemins de fichier diffèrent entre les ordinateurs qui hébergent le réplica principal et le réplica secondaire cible, utilisez également le paramètre de restauration **RelocateFile** .  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
5.  Pour terminer la configuration de la base de données secondaire, vous devez l'attacher au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="sample-backup-and-restore-script-and-command"></a><a name="ExamplePSscript"></a> Exemple de commande et de script de sauvegarde et restauration  
 Les commandes PowerShell suivantes effectuent une sauvegarde complète de base de données et de son journal des transactions dans un partage réseau et restaurent ces sauvegardes depuis ce partage. Dans cet exemple, on suppose que le chemin d'accès de fichier dans lequel la base de données est restaurée est identique au chemin d'accès de fichier dans lequel la base de données a été sauvegardée.  
  
```  
# Create database backup  
Backup-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -ServerInstance "SourceMachine\Instance"  
# Create log backup  
Backup-SqlDatabase -Database "MyDB1" -BackupAction "Log" -BackupFile "\\share\backups\MyDB1.trn" -ServerInstance "SourceMachine\Instance"  
# Restore database backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
# Restore log backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.trn" -RestoreAction "Log" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
  
```  
  
##  <a name="next-steps"></a><a name="FollowUp"></a>Étapes suivantes  
 Pour terminer la configuration de la base de données secondaire, attachez la base de données nouvellement restaurée au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md)   
 [Arguments RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md)   
 [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  

---
title: Préparer une base de données pour la mise en miroir
description: Découvrez comment préparer une base de données SQL Server pour la mise en miroir de bases de données à l’aide de SQL Server Management Studio ou de Transact-SQL dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 11/10/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], preparing for mirroring
- logins [SQL Server], database mirroring
- mirror database [SQL Server]
ms.assetid: 8676f9d8-c451-419b-b934-786997d46c2b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b55c6f886fb875c3091b8b2090a41bbe4cf832d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343709"
---
# <a name="prepare-a-mirror-database-for-mirroring-sql-server"></a>Préparer une base de données miroir pour la mise en miroir (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Avant qu'une session de mise en miroir de bases de données puisse commencer, le propriétaire de la base de données ou l'administrateur système doit s'assurer que la base de données miroir a été créée et qu'elle est prête pour la mise en miroir. La création d'une nouvelle base de données miroir requiert au minimum la réalisation d'une sauvegarde complète de la base de données principale puis d'une sauvegarde du journal, ainsi que la restauration de ces deux sauvegardes sur l'instance du serveur miroir, en utilisant WITH NORECOVERY.  
  
 Cette rubrique explique comment préparer une base de données miroir dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Avant de commencer :**  
  
     [Configuration requise](#Requirements)  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   [Pour préparer une base de données miroir existante pour redémarrer la mise en miroir](#PrepareToRestartMirroring)  
  
-   [Pour préparer une nouvelle base de données miroir](#CombinedProcedure)  
  
-   **Suivi :**  [Après avoir préparé une base de données miroir](#FollowUp)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="requirements"></a><a name="Requirements"></a> Spécifications  
  
-   Les instances du serveur miroir et du serveur principal doivent exécuter la même version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Bien qu'il soit possible que le serveur miroir possède une version ultérieure de SQL Server, cette configuration est recommandée uniquement lors d'une mise à niveau planifiée avec soin. Dans une telle configuration, vous courez le risque d'un basculement automatique, dans lequel le déplacement des données est automatiquement interrompu, car les données n'ont pas accès à une version antérieure de SQL Server. Pour plus d’informations, consultez [Mise à niveau des instances en miroir](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   Les instances du serveur miroir et du serveur principal doivent exécuter la même édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la prise en charge de la mise en miroir de bases de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](~/sql-server/editions-and-components-of-sql-server-2017.md).  
  
-   La base de données doit utiliser le mode de récupération complète.  
  
     Pour plus d’informations, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) ou [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   Le nom de la base de données miroir doit être le même que celui de la base de données principale.  
  
-   La base de données miroir doit être dans l'état RESTORING pour que la mise en miroir fonctionne. Lors de la préparation d'une base de données miroir, vous devez utiliser RESTORE WITH NORECOVERY pour chaque opération de restauration. Au minimum, vous devrez restaurer avec RESTORE WITH NORECOVERY une sauvegarde complète de la base de données principale, suivie de toutes les sauvegardes de journaux suivantes.  
  
-   Le système dans lequel vous envisagez de créer la base de données miroir doit posséder un lecteur de disque avec suffisamment d'espace pour contenir la base de données miroir.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez pas mettre en miroir les bases de données système **master**, **msdb**, **temp** ou **model** .  
  
-   Vous ne pouvez pas mettre en miroir une base de données qui appartient à un [groupe de disponibilité Always On](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Utilisez une sauvegarde complète très récente ou une sauvegarde différentielle récente de la base de données principale.  
  
-   Si un travail de sauvegarde du journal est planifié pour s'exécuter très fréquemment sur la base de données principale, vous pouvez être amené à désactiver le travail de sauvegarde tant que la mise en miroir n'a pas commencé.  
  
-   Si possible, le chemin d'accès (y compris la lettre de lecteur) de la base de données miroir doit être identique au chemin d'accès de la base de données principale.  
  
     Si les chemins d'accès des fichiers doivent différer, par exemple, si la base de données principale se trouve sur le lecteur « F: », mais que le système miroir n'a pas de lecteur F:, vous devez inclure l'option MOVE dans l'instruction RESTORE STATEMENT.  
  
    > [!IMPORTANT]  
    >  Pour pouvoir ajouter un fichier pendant une session de mise en miroir sans compromettre la session, il faut que le chemin d'accès au fichier existe sur les deux serveurs. Par conséquent, si vous déplacez des fichiers de base de données lors de la création de la base de données miroir, une opération d'ajout de fichier ultérieure peut échouer sur la base de données miroir et entraîner la suspension de la mise en miroir. Pour plus d’informations sur le traitement d’un échec d’opération de création de fichier, consultez [Résoudre des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md).  
  
-   Si la base de données principale comporte des catalogues de texte intégral, nous vous recommandons de consulter [Mise en miroir de bases de données et catalogues de texte intégral &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md).  
  
-   Dans le cas d'une base de données de production, effectuez toujours les sauvegardes sur une unité distincte.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 TRUSTWORTHY a la valeur OFF lorsqu'une base de données est sauvegardée. Par conséquent, la propriété TRUSTWORTHY d'une nouvelle base de données miroir a toujours la valeur OFF. Si la base de données doit être fiable après un basculement, des opérations de configuration supplémentaires sont requises. Pour plus d’informations, consultez [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
 Pour plus d’informations sur l’activation du déchiffrement automatique de la clé principale d’une base de données miroir, consultez [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Propriétaire de base de données ou administrateur système.  
  
##  <a name="to-prepare-an-existing-mirror-database-to-restart-mirroring"></a><a name="PrepareToRestartMirroring"></a> Pour préparer une base de données miroir existante pour redémarrer la mise en miroir  
 Si la mise en miroir a été supprimée et que la base de données miroir est toujours à l'état RECOVERING, vous pouvez redémarrer la mise en miroir.  
  
1.  Prenez au moins une sauvegarde du journal sur la base de données principale. Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
2.  Sur la base de données miroir, restaurez à l'aide de RESTORE WITH NORECOVERY toutes les sauvegardes des journaux effectuées sur la base de données principale depuis la suppression de la mise en miroir. Pour plus d’informations, consultez [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="to-prepare-a-new-mirror-database"></a><a name="CombinedProcedure"></a> Pour préparer une nouvelle base de données miroir  
 **Pour préparer une base de données miroir**  
  
> [!NOTE]  
>  Pour obtenir un exemple [!INCLUDE[tsql](../../includes/tsql-md.md)] de cette procédure, consultez [Exemple (Transact-SQL)](#TsqlExample), plus loin dans cette section.  
  
1.  Connectez-vous à l'instance de serveur principal.  
  
2.  Créez une sauvegarde complète ou une sauvegarde différentielle de la base de données principale.  
  
    -   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
    -   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).  
  
3.  En général, vous devez prendre au moins une sauvegarde du journal sur la base de données principale. Toutefois, une sauvegarde du journal peut s'avérer superflue, si la base de données vient d'être créée et qu'aucune sauvegarde du journal n'a été encore réalisée ou si le mode de récupération vient d'être modifié de SIMPLE à FULL.  
  
    -   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
4.  À moins que les sauvegardes se trouvent sur un lecteur réseau accessible à partir des deux systèmes, copiez les sauvegardes de la base de données et des journaux sur le système qui hébergera l'instance de serveur miroir.  
  
5.  Connectez-vous à l'instance de serveur miroir.  
  
6.  À l'aide de RESTORE WITH NORECOVERY, créez la base de données miroir en restaurant la sauvegarde complète de la base de données et, éventuellement, la sauvegarde différentielle de base de données la plus récente, sur l'instance de serveur miroir.  
  
    > [!NOTE]  
    >  Si vous restaurez le groupe de fichiers de base de données par groupe de fichiers, veillez à restaurer l'intégralité de la base de données.  
  
    -   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
    -   [RESTORE &amp;#40;Transact-SQL&amp;#41;](../../t-sql/statements/restore-statements-transact-sql.md) et [RESTORE Arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
7.  À l'aide de RESTORE WITH NORECOVERY, appliquez les autres sauvegardes de fichiers journaux ou sauvegardes en attente à la base de données miroir.  
  
    -   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Avant de démarrer une session de mise en miroir de bases de données, vous devez créer la base de données miroir. Vous devez procéder à cette opération avant de démarrer la session de mise en miroir.  
  
 L'exemple suivant utilise la base de données d'exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] qui emploie par défaut le mode de récupération simple.  
  
1.  Pour utiliser la mise en miroir de base de données sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , modifiez-la afin qu'elle utilise le mode de restauration complète :  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Après avoir changé le mode de récupération de SIMPLE à FULL, créez une sauvegarde complète qui pourra être utilisée pour créer la base de données miroir. Dans la mesure où le mode de récupération vient d'être modifié, l'option WITH FORMAT est spécifiée pour créer un nouveau support de sauvegarde. Il peut se révéler utile de séparer les sauvegardes effectuées en mode de récupération complète des sauvegardes préalablement effectuées en mode de récupération simple. Pour les besoins de cet exemple, le fichier de sauvegarde (`C:\AdventureWorks.bak`) est créé sur le même lecteur que la base de données.  
  
    > [!NOTE]  
    >  Dans le cas d'une base de données de production, il est conseillé de toujours effectuer les sauvegardes sur une unité distincte.  
  
     Sur l'instance de serveur principal (sur `PARTNERHOST1`), créez une sauvegarde complète de la base de données principale comme suit :  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copiez cette sauvegarde complète sur le serveur miroir.  
  
4.  À l'aide de RESTORE WITH NORECOVERY, restaurez la sauvegarde complète sur l'instance de serveur miroir. La commande de restauration dépend si les chemins d'accès aux bases de données principale et miroir sont identiques ou non.  
  
    -   **Si les chemins d'accès sont identiques :**  
  
         Sur l'instance de serveur miroir (sur `PARTNERHOST5`), restaurez la sauvegarde complète comme suit :  
  
        ```  
        RESTORE DATABASE AdventureWorks   
            FROM DISK = 'C:\AdventureWorks.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Si les chemins d'accès sont différents :**  
  
         Si le chemin d'accès de la base de données miroir n'est pas le même que celui de la base de données principale (lettres de lecteurs distinctes, par exemple), la création de la base de données miroir requiert l'intégration d'une clause MOVE dans l'opération de restauration.  
  
        > [!IMPORTANT]  
        >  Si les chemins d'accès des bases de données principale et miroir diffèrent, vous ne pouvez pas ajouter de fichier. La raison tient à la réception du journal pour l'opération d'ajout de fichier puisque l'instance de serveur miroir tente de placer le nouveau fichier dans l'emplacement utilisé par la base de données principale.  
  
         Par exemple, la commande ci-dessous restaure une sauvegarde d’une base de données principale qui réside dans C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\ dans un emplacement différent, D:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Dat`a\`, où la base de données miroir doit résider.  
  
        ```  
        RESTORE DATABASE AdventureWorks  
           FROM DISK='C:\AdventureWorks.bak'  
           WITH NORECOVERY,   
              MOVE 'AdventureWorks_Data' TO   
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Data.mdf',   
              MOVE 'AdventureWorks_Log' TO  
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Log.ldf';  
        GO  
        ```  
  
5.  Après avoir créé la sauvegarde complète, vous devez créer une sauvegarde du journal sur la base de données principale. Par exemple, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sauvegarde le journal dans le même fichier utilisé par la sauvegarde complète antérieure :  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Avant de démarrer la mise en miroir, vous devez appliquer la sauvegarde du journal requise (et toutes les sauvegardes de journal ultérieures).  
  
     Par exemple, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous restaure le premier journal à partir de `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Si des sauvegardes de journal supplémentaires se produisent avant le démarrage de la mise en miroir, vous devez également restaurer toutes ces sauvegardes de journal, dans l'ordre, sur le serveur miroir, en utilisant WITH NORECOVERY.  
  
     Par exemple, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-dessous restaure deux journaux supplémentaires à partir de `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
 Pour voir un exemple de configuration de la mise en miroir d’une base de données illustrant la configuration de la sécurité, la préparation de la base de données miroir, la configuration des serveurs partenaires et l’ajout d’un témoin, consultez [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
##  <a name="follow-up-after-preparing-a-mirror-database"></a><a name="FollowUp"></a> Suivi : Après avoir préparé une base de données miroir  
  
1.  Si des sauvegardes de fichier journal supplémentaires ont été effectuées depuis votre opération RESTORE LOG la plus récente, vous devez appliquer manuellement chaque sauvegarde de journal supplémentaire, à l'aide de RESTORE WITH NORECOVERY.  
  
2.  Démarrez la session de mise en miroir. Pour plus d’informations, consultez [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) ou de [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md).  
  
3.  Si vous avez désactivé le travail de sauvegarde sur la base de données principale, réactivez le travail.  
  
4.  Si la base de données doit être fiable après un basculement, des opérations de configuration supplémentaires sont requises après le début de la mise en miroir. Pour plus d’informations, consultez [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Mise en miroir de bases de données et catalogues de texte intégral &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)   
 [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)  
  
  


---
title: 'Restauration de base de données : différentielle'
description: Cet article explique comment restaurer une sauvegarde différentielle de base de données dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- restoring databases [SQL Server], full differential backups
- database backups [SQL Server], full differential backups
- database restores [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
ms.assetid: 0dd971a4-ee38-4dd3-9f30-ef77fc58dd11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 028e5b2d1f26da698edae325b003fb402f721ee1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075841"
---
# <a name="restore-a-differential-database-backup-sql-server"></a>Restaurer une sauvegarde différentielle de base de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment restaurer une sauvegarde différentielle de base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Composants requis](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour restaurer une sauvegarde différentielle de base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   La commande RESTORE n'est pas autorisée dans une transaction explicite ou implicite.  
  
-   Les sauvegardes créées avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vous pouvez restaurer une base de données utilisateur à partir d'une sauvegarde de base de données créée à l'aide de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Que vous soyez en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, pour pouvoir restaurer une base de données, vous devez d'abord sauvegarder le journal des transactions actif (appelé fin du journal). Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-restore-a-differential-database-backup"></a>Pour restaurer une sauvegarde différentielle de base de données  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**. Selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, sur **Restaurer**, puis cliquez sur **Base de données**.  
  
4.  Dans la page **Général** , utilisez la section **Source** pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer. Sélectionnez l’une des options suivantes :  
  
    -   **Sauvegarde de la base de données**  
  
         Sélectionnez la base de données à restaurer dans la liste déroulante. La liste contient uniquement les bases de données qui ont été sauvegardées selon l'historique de sauvegarde **msdb** .  
  
    > [!NOTE]  
    >  Si la sauvegarde est prise à partir d'un serveur différent, le serveur de destination ne disposera pas des informations d'historique de sauvegarde pour la base de données spécifiée. Dans ce cas, sélectionnez **Unité** pour spécifier manuellement le fichier ou l'unité à restaurer.  
  
    -   **Appareil**  
  
         Cliquez sur le bouton Parcourir ( **...** ) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Dans la zone **Type du média de sauvegarde** , sélectionnez l'un des types d'unités proposés. Pour sélectionner une ou plusieurs unités pour la zone **Support de sauvegarde** , cliquez sur **Ajouter**.  
  
         Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .  
  
         Dans la zone de liste **Source : Appareil : Base de données**, sélectionnez le nom de la base de données à restaurer.  
  
         **Remarque** Cette liste n'est disponible que lorsque **Unité** est sélectionné. Seules les bases de données qui ont des copies de sauvegarde sur l'unité sélectionnée seront disponibles.  
  
5.  Dans la section **Destination** , la zone **Base de données** est automatiquement renseignée avec le nom de la base de données à restaurer. Pour changer le nom de la base de données, entrez le nouveau nom dans la zone **Base de données** .  
  
    > [!NOTE]  
    >  Pour arrêter la restauration à un moment précis, cliquez sur **Chronologie** pour accéder à la boîte de dialogue **Chronologie de sauvegarde** . Pour obtenir de l’aide concernant l’arrêt d’une restauration de base de données à un moment précis dans le temps, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
6.  Dans la grille **Jeux de sauvegarde à restaurer** , sélectionnez les sauvegardes par la sauvegarde différentielle que vous souhaitez restaurer.  
  
     Pour plus d’informations sur les colonnes de la grille **Jeux de sauvegarde à restaurer** , consultez [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md).  
  
7.  Dans la page **Options** , dans le volet **Options de restauration** , vous pouvez choisir les options suivantes si elles s'appliquent à votre situation :  
  
    -   **Remplacer la base de données existante (WITH REPLACE)**  
  
    -   **Conserver les paramètres de la réplication (WITH KEEP_REPLICATION)**  
  
    -   **Demander confirmation avant chaque restauration de sauvegarde**  
  
    -   **Restreindre l'accès à la base de données restaurée (WITH RESTRICTED_USER)**  
  
     Pour plus d’informations sur ces options, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
8.  Sélectionnez une option pour la zone **État de récupération** . Cette zone détermine l'état de la base de données à l'issue de l'opération de restauration.  
  
    -   **RESTORE WITH RECOVERY** est le comportement par défaut qui laisse la base de données opérationnelle en annulant les transactions non validées. Les journaux des transactions supplémentaires ne peuvent pas être restaurés. Choisissez cette option si vous restaurez toutes les sauvegardes nécessaires maintenant.  
  
    -   **RESTORE WITH NORECOVERY** qui laisse la base de données non opérationnelle et n’annule pas les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. La base de données ne peut pas être utilisée tant qu'elle n'est pas récupérée.  
  
    -   **RESTORE WITH STANDBY** qui laisse la base de données en lecture seule. Elle annule les transactions non validées, mais enregistre les actions d'annulation dans un fichier afin de rendre réversibles les effets de la récupération.  
  
     Pour obtenir des descriptions des options, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
9. Les opérations de restauration échoueront s'il existe des connexions actives à la base de données. Activez l'option **Fermer les connexions existantes** pour garantir que toutes les connexions actives entre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et la base de données sont fermées.  
  
10. Sélectionnez **Demander confirmation avant chaque restauration de sauvegarde** si vous souhaitez être invité entre chaque opération de restauration. Cela n'est généralement pas nécessaire à moins que la base de données ne soit volumineuse et que vous ne souhaitiez surveiller l'état de l'opération de restauration.  
  
11. Utilisez éventuellement la page **Fichiers** pour restaurer la base de données à un nouvel emplacement. Pour obtenir de l’aide concernant le déplacement d’une base de données, consultez [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-restore-a-differential-database-backup"></a>Pour restaurer une sauvegarde différentielle de base de données  
  
1.  Exécutez l'instruction RESTORE DATABASE, en spécifiant la clause NORECOVERY, pour restaurer la sauvegarde complète de la base de données précédant la sauvegarde différentielle. Pour plus d’informations, consultez [Procédure : Restaurer une sauvegarde complète](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md).  
  
2.  Exécutez l'instruction RESTORE DATABASE pour restaurer la sauvegarde différentielle de la base de données, en spécifiant :  
  
    -   le nom de la base de données à laquelle s'applique la sauvegarde différentielle ;  
  
    -   l’unité de sauvegarde à partir de laquelle la sauvegarde différentielle est restaurée ;  
  
    -   la clause NORECOVERY si vous devez appliquer des sauvegardes du journal des transactions après restauration de la sauvegarde différentielle. Dans le cas contraire, spécifiez la clause RECOVERY.  
  
3.  Avec le mode de restauration complète ou de récupération utilisant les journaux de transactions, la restauration d'une sauvegarde différentielle restaure la base de données au point où la sauvegarde différentielle a été effectuée. Pour restaurer jusqu'au point de défaillance, vous devez appliquer toutes les sauvegardes du journal des transactions créées après la dernière sauvegarde différentielle. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
  
#### <a name="a-restoring-a-differential-database-backup"></a>R. Restauration d'une sauvegarde différentielle de base de données  
 Cet exemple illustre la restauration et la sauvegarde différentielle de la base de données `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost, and restore full database,   
-- specifying the original full database backup and NORECOVERY,   
-- which allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   RECOVERY;  
GO  
```  
  
#### <a name="b-restoring-a-database-differential-database-and-transaction-log-backup"></a>B. Restauration d'une base de données, d'une base de données différentielle et d'une sauvegarde du journal des transactions  
 Cet exemple illustre la restauration d'une base de données, la sauvegarde différentielle d'une base de données et la sauvegarde du journal des transactions de la base de données `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost at this point. Now restore the full   
-- database. Specify the original full database backup and NORECOVERY.  
-- NORECOVERY allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
-- Now restore each transaction log backup created after  
-- the differential database backup.  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log1  
   WITH NORECOVERY;  
GO  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log2  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  

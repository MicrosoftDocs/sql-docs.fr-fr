---
description: Restaurer une base de données SQL Server jusqu'à une limite dans le temps (mode de récupération complète)
title: Restaurer une base de données SQL Server jusqu’à une limite dans le temps (mode de récupération complète) | Microsoft Docs
decription: Learn how to restore a database to a point in time in SQL Server using SQL Server Management Studio or Transact-SQL in the full or bulk-logged recovery models.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- STOPAT clause [RESTORE LOG statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
ms.assetid: 3a5daefd-08a8-4565-b54f-28ad01a47d32
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4038ba678af8635f3ac2bd12f68c818a61e2c87f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075632"
---
# <a name="restore-a-sql-server-database-to-a-point-in-time-full-recovery-model"></a>Restaurer une base de données SQL Server jusqu'à une limite dans le temps (mode de récupération complète)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment restaurer une base de données à un point précis dans le temps dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cette rubrique s'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisant le mode de restauration complète ou de récupération utilisant les journaux de transactions.  
  
> [!IMPORTANT]  
>  Dans le mode de récupération utilisant les journaux de transactions, si la sauvegarde du journal contient des modifications journalisées en bloc, la récupération jusqu'à une date et heure à un point de cette sauvegarde est impossible. La base de données doit être récupérée à la fin de la sauvegarde du journal des transactions.  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour restaurer une base de données SQL Server à un point précis dans le temps, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Utilisation de STANDBY pour retrouver un moment inconnu dans le temps.  
  
-   Spécification du point d'arrêt suffisamment tôt dans la séquence de restauration  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour restaurer une base de données à un point dans le temps**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis développez l'arborescence du serveur.  
  
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
  
6.  Cliquez sur **Chronologie** pour accéder à la boîte de dialogue **Chronologie de sauvegarde**.  
  
7.  Dans la section **Restaurer sur** , cliquez sur **Date et heure spécifiques**.  
  
8.  Utilisez les zones **Date** et **Heure** ou la barre du curseur pour spécifier une date et une heure spécifiques auxquelles la restauration doit s'arrêter. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Utilisez la zone **Chronologie - Intervalle** pour modifier la durée affichée dans la chronologie.  
  
9. Après avoir défini un point spécifique dans le temps, l'assistant de récupération de base de données garantit que seules les sauvegardes nécessaires à la restauration à un point précis dans le temps sont sélectionnées dans la colonne **Restaurer** de la grille **Jeux de sauvegarde à restaurer**. Ces sauvegardes sélectionnées constituent le plan de restauration recommandé pour votre limite de restauration dans le temps. Utilisez uniquement les sauvegardes sélectionnées pour votre opération de restauration jusqu'à une date et heure.  
  
     Pour plus d’informations sur les colonnes de la grille **Jeux de sauvegarde à restaurer** , consultez [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md). Pour plus d’informations sur l’Assistant de récupération de base de données, consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
10. Dans la page **Options** , dans le volet **Options de restauration** , vous pouvez choisir les options suivantes si elles s'appliquent à votre situation :  
  
    -   **Remplacer la base de données existante (WITH REPLACE)**  
  
    -   **Conserver les paramètres de la réplication (WITH KEEP_REPLICATION)**  
  
    -   **Restreindre l'accès à la base de données restaurée (WITH RESTRICTED_USER)**  
  
     Pour plus d’informations sur ces options, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
11. Sélectionnez une option pour la zone **État de récupération** . Cette zone détermine l'état de la base de données à l'issue de l'opération de restauration.  
  
    -   **RESTORE WITH RECOVERY** est le comportement par défaut qui laisse la base de données opérationnelle en annulant les transactions non validées. Les journaux des transactions supplémentaires ne peuvent pas être restaurés. Choisissez cette option si vous restaurez toutes les sauvegardes nécessaires maintenant.  
  
    -   **RESTORE WITH NORECOVERY** qui laisse la base de données non opérationnelle et n’annule pas les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. La base de données ne peut pas être utilisée tant qu'elle n'est pas récupérée.  
  
    -   **RESTORE WITH STANDBY** qui laisse la base de données en lecture seule. Elle annule les transactions non validées, mais enregistre les actions d'annulation dans un fichier afin de rendre réversibles les effets de la récupération.  
  
     Pour obtenir des descriptions des options, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
12. L’option **Effectuer la sauvegarde de la fin du journal avant la restauration** est sélectionnée si elle s’avère nécessaire pour le moment sélectionné. Vous n'avez pas besoin de modifier ce paramètre, mais vous pouvez choisir de sauvegarder la fin du journal même si ce n'est pas obligatoire.  
  
13. Les opérations de restauration peuvent échouer s'il existe des connexions actives à la base de données. Activez l'option **Fermer les connexions existantes** pour garantir que toutes les connexions actives entre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et la base de données sont fermées. Cette case à cocher définit la base de données en mode mono-utilisateur avant d'effectuer les opérations de restauration, et définit la base de données en mode multi-utilisateur une fois l'opération terminée.  
  
14. Sélectionnez **Demander confirmation avant chaque restauration de sauvegarde** si vous souhaitez être invité entre chaque opération de restauration. Cela n'est généralement pas nécessaire à moins que la base de données ne soit volumineuse et que vous ne souhaitiez surveiller l'état de l'opération de restauration.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Avant de commencer**  
  
 La restauration d'une heure spécifiée est toujours effectuée à partir d'une sauvegarde de fichier journal. Dans chaque instruction RESTORE LOG de la séquence de restauration, vous devez spécifier votre heure cible ou votre transaction dans une clause STOPAT identique. Comme condition préalable dans une restauration limitée dans le temps, vous devez restaurer une sauvegarde complète de base de données dont le point d'arrêt est antérieur à votre heure de restauration cible. Cette sauvegarde complète de base de données peut être plus ancienne que la sauvegarde complète de base de données la plus récente dans la mesure où vous restaurez ensuite chaque sauvegarde de fichier journal suivante jusqu'à la sauvegarde de fichier journal (comprise) contenant votre limite dans le temps cible.  
  
 Pour vous aider à identifier la sauvegarde de base à restaurer, vous pouvez éventuellement spécifier votre clause WITH STOPAT dans l'instruction RESTORE DATABASE afin de générer une erreur lorsqu'une sauvegarde de données est trop récente pour l'heure cible spécifiée. La sauvegarde de données complète est toujours restaurée, même si elle contient l'heure cible.  
  
 **Syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] de base**  
  
 RESTORE LOG *nom_base_de_données* FROM <unité_sauvegarde> WITH STOPAT **=** _heure_ **,** RECOVERY...  
  
 Le point de récupération est la dernière validation de transaction qui s’est produite au plus tard à l’heure **datetime** spécifiée par *time*.  
  
 Pour restaurer uniquement les modifications apportées avant un moment spécifique dans le temps, spécifiez WITH STOPAT **=** _time_ pour chaque sauvegarde que vous restaurez. De cette manière, vous êtes certain de ne pas dépasser le moment cible.  
  
 **Pour restaurer une base de données à un point dans le temps**  
  
> [!NOTE]  
>  Pour obtenir un exemple de cette procédure, consultez [Exemple (Transact-SQL)](#TsqlExample)plus loin dans cette section.  
  
1.  Connectez -vous à l'instance de serveur sur laquelle vous souhaitez restaurer la base de données.  
  
2.  Exécutez l'instruction RESTORE DATABASE à l'aide de l'option NORECOVERY.  
  
    > [!NOTE]  
    >  Si une séquence de restauration partielle exclut tout groupe de fichiers [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) , la limite de restauration dans le temps n’est pas prise en charge. Vous pouvez forcer la séquence de restauration à continuer. Cependant, les groupes de fichiers FILESTREAM omis de l'instruction RESTORE ne peuvent jamais être restaurés. Pour forcer une limite de restauration dans le temps, spécifiez l'option CONTINUE_AFTER_ERROR avec l'option STOPAT, STOPATMARK ou STOPBEFOREMARKx, que vous devez également spécifier dans vos instructions RESTORE LOG suivantes. Si vous spécifiez l'option CONTINUE_AFTER_ERROR, la séquence de restauration partielle réussit et le groupe de fichiers FILESTREAM devient irrécupérable.  
  
3.  Restaurez la dernière sauvegarde différentielle de base de données, si elle existe, sans récupérer la base de données (RESTORE DATABASE *database_name* FROM *backup_device* WITH NORECOVERY).  
  
4.  Appliquez chaque sauvegarde du journal des transactions dans l’ordre de sa création, en spécifiant l’heure à laquelle vous avez l’intention d’arrêter la restauration du journal (RESTORE DATABASE *database_name* FROM <backup_device> WITH STOPAT **=**_time_**,** RECOVERY).  
  
    > [!NOTE]  
    >  Options RECOVERY et STOPAT. Si la sauvegarde du journal des transactions ne contient pas l'heure demandée (par exemple, si l'heure spécifiée dépasse la dernière heure figurant dans le journal des transactions), un avertissement est émis et la base de données n'est pas récupérée.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant restaure une base de données dans l'état où elle se trouvait le `12:00 AM` à `April 15, 2020` et décrit une opération de restauration impliquant plusieurs sauvegardes de fichiers journaux. Sur l'unité de sauvegarde `AdventureWorksBackups`, la sauvegarde de base de données complète à restaurer correspond au troisième jeu de sauvegarde (`FILE = 3`), la première sauvegarde de fichier journal correspond au quatrième jeu de sauvegarde (`FILE = 4`) et la seconde sauvegarde de fichier journal correspond au cinquième jeu de sauvegarde (`FILE = 5`).  
  
> [!IMPORTANT]  
>  La base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilise le mode de récupération simple. Pour autoriser les sauvegardes de fichier journal, avant d'effectuer une sauvegarde complète de base de données, la base de données a été configurée pour utiliser le mode de récupération complète par le biais de `ALTER DATABASE AdventureWorks SET RECOVERY FULL`.  
  
```  
RESTORE DATABASE AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks WITH RECOVERY;   
GO  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restaurer une base de données jusqu’au point d’échec en mode de récupération complète &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurer une base de données jusqu’à une transaction marquée &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Récupérer un numéro séquentiel dans le journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ToPointInTime%2A> (SMO)  
  
## <a name="see-also"></a>Voir aussi  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
  

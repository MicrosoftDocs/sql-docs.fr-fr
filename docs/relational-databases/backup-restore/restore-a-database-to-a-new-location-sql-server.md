---
title: Restaurer une base de données à un nouvel emplacement (SQL Server) | Microsoft Docs
description: Cet article explique comment restaurer une base de données SQL Server vers un nouvel emplacement, et comment la renommer à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring databases [SQL Server], moving
- database restores [SQL Server], creating new databases
- restoring [SQL Server], with move
- restoring databases [SQL Server], creating new databases
- database backups [SQL Server], creating new database from
- backing up databases [SQL Server], creating new database from
- restoring databases [SQL Server], renaming
- database creation [SQL Server], restoring with move
ms.assetid: 4da76d61-5e11-4bee-84f5-b305240d9f42
author: cawrites
ms.author: chadam
ms.openlocfilehash: c0b65303e9c23977f658e3efef0d87a6713d55e4
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075821"
---
# <a name="restore-a-database-to-a-new-location-sql-server"></a>Restaurer une base de données à un nouvel emplacement (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment restaurer une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un nouvel emplacement, et éventuellement renommer la base de données, dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de SQL Server Management Studio(SSMS) ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous pouvez déplacer une base de données vers un nouveau chemin d'accès au répertoire ou créer une copie d'une base de données sur la même instance de serveur ou sur une instance différente.  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'administrateur système qui restaure une sauvegarde complète de base de données doit être la seule personne à utiliser la base de données à restaurer.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Que vous soyez en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, pour pouvoir restaurer une base de données, vous devez d'abord sauvegarder le journal des transactions actif. Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  

-   Pour restaurer une base de données chiffrée, **vous devez avoir accès au certificat ou à la clé asymétrique utilisé(e) pour chiffrer la base de données.** Sans ce certificat ou cette clé asymétrique, vous ne pouvez pas restaurer la base de données. Vous devez conserver le certificat utilisé pour chiffrer la clé de chiffrement de base de données tant que vous en avez besoin pour la sauvegarde. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Pour connaître les autres points à prendre en considération pour déplacer une base de données, consultez [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
-   Si vous restaurez une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou de versions ultérieures vers [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la base de données est automatiquement mise à niveau. En général, la base de données est immédiatement disponible. Toutefois, si une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comprend des index de recherche en texte intégral, le processus de mise à niveau les importe, les réinitialise ou les reconstruit, selon la valeur de la propriété de serveur  **upgrade_option** . Si l’option de mise à niveau a la valeur Importer (**upgrade_option** = 2) ou Reconstruire (**upgrade_option** = 0), les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, les index de recherche en texte intégral associés sont reconstruits si aucun catalogue de texte intégral n'est disponible. Pour modifier le paramètre de la propriété de serveur **upgrade_option** , utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Pour des raisons de sécurité, nous vous recommandons de ne pas attacher ni restaurer des bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixes **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données.  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
  
## <a name="restore-a-database-to-a-new-location-optionally-rename-the-database-using-ssms"></a>Restaurer une base de données à un nouvel emplacement et renommer éventuellement la base de données à l’aide de SSMS 

  
1.  Connectez-vous à l'instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]puis, dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Cliquez avec le bouton droit sur **Bases de données**, puis cliquez sur **Restaurer la base de données**. La boîte de dialogue **Restaurer la base de données** s'ouvre.  
  
3.  Dans la page **Général** , utilisez la section **Source** pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer. Sélectionnez l’une des options suivantes :  
  
    -   **Sauvegarde de la base de données**  
  
         Sélectionnez la base de données à restaurer dans la liste déroulante. La liste contient uniquement les bases de données qui ont été sauvegardées selon l'historique de sauvegarde **msdb** .  
  
    > **REMARQUE :** Si la sauvegarde est prise à partir d'un serveur différent, le serveur de destination ne disposera pas des informations d'historique de sauvegarde pour la base de données spécifiée. Dans ce cas, sélectionnez **Unité** pour spécifier manuellement le fichier ou l'unité à restaurer.  
  
    1.  **Appareil**  
  
         Cliquez sur le bouton Parcourir ( **...** ) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Dans la zone **Type du média de sauvegarde** , sélectionnez l'un des types d'unités proposés. Pour sélectionner une ou plusieurs unités pour la zone **Support de sauvegarde** , cliquez sur **Ajouter**.  
  
         Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .  
  
         Dans la zone de liste **Source : Appareil : Base de données**, sélectionnez le nom de la base de données à restaurer.  
  
         **Remarque** Cette liste n'est disponible que lorsque **Unité** est sélectionné. Seules les bases de données qui ont des copies de sauvegarde sur l'unité sélectionnée seront disponibles.  
  
4.  Dans la section **Destination** , la zone **Base de données** est automatiquement renseignée avec le nom de la base de données à restaurer. Pour changer le nom de la base de données, entrez le nouveau nom dans la zone **Base de données** .  
  
5.  Dans la zone **Restaurer sur** , laissez la valeur par défaut **Vers la dernière sauvegarde prise** ou cliquez sur **Chronologie** pour accéder à la boîte de dialogue **Chronologie de sauvegarde** afin de sélectionner manuellement une limite spécifique pour arrêter l'action de récupération. Pour plus d'informations sur la façon de désigner une limite spécifique, consultez [Backup Timeline](../../relational-databases/backup-restore/backup-timeline.md) .  
  
6.  Dans la grille **Jeux de sauvegarde à restaurer** , sélectionnez les sauvegardes à restaurer. Cette grille affiche les sauvegardes disponibles pour l'emplacement spécifié. Par défaut, un plan de récupération est suggéré. Pour ignorer le plan de récupération suggéré, vous pouvez modifier les sélections dans la grille. Les sauvegardes qui dépendent de la restauration d'une sauvegarde antérieure sont automatiquement désélectionnées dès lors que la sauvegarde antérieure est désélectionnée.  
  
     Pour plus d’informations sur les colonnes de la grille **Jeux de sauvegarde à restaurer** , consultez [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md).  
  
7.  Pour spécifier le nouvel emplacement des fichiers de base de données, sélectionnez la page **Fichiers** , puis cliquez sur **Déplacer tous les fichiers dans le dossier**. Fournissez un nouvel emplacement pour les dossiers **Fichier de données** et **Fichier journal**. Pour plus d’informations sur cette grille, consultez [Restaurer la base de données &#40;page Fichiers&#41;](../../relational-databases/backup-restore/restore-database-files-page.md).  
  
8.  Dans la page **Options** , ajustez les options si vous le souhaitez. Pour plus d’informations sur ces options, consultez [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  

 ## <a name="restore-database-to-a-new-location-optionally-rename-the-database-using-t-sql"></a>Restaurer une base de données à un nouvel emplacement et renommer éventuellement la base de données à l’aide de T-SQL
 
 
1.  Déterminez éventuellement les noms logiques et physiques des fichiers dans le jeu de sauvegarde qui contient la sauvegarde complète de la base de données que vous souhaitez restaurer. Cette instruction retourne une liste des fichiers journaux et des fichiers de base de données contenus dans le jeu de sauvegarde. La syntaxe de base est la suivante :  
  
     RESTORE FILELISTONLY FROM *<unité_sauvegarde>* WITH FILE = *numéro_fichier_jeu_sauvegarde* 
  
     Ici, *numéro_fichier_jeu_sauvegarde* indique la position de la sauvegarde dans le support de sauvegarde. Vous pouvez obtenir la position d'un jeu de sauvegarde en utilisant l'instruction [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) . Pour plus d’informations, consultez « Spécification d’un jeu de sauvegarde » dans [RESTORE, arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
     Cette instruction prend également en charge plusieurs options WITH. Pour plus d’informations, consultez [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
2.  Utilisez l'instruction [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md) pour restaurer la sauvegarde complète de la base de données. Par défaut, les fichiers de données et les fichiers journaux sont restaurés à leur emplacement d'origine. Pour déplacer une base de données, utilisez l'option MOVE pour déplacer chacun des fichiers de la base de données et éviter des collisions avec les fichiers existants.  

  La syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] de base pour restaurer la base de données en utilisant un nouvel emplacement et un nouveau nom est :  
  ```sql
  RESTORE DATABASE *new_database_name*  
  
  FROM *backup_device* [ ,...*n* ]  
  
  [ WITH  
 
   {  
  
      [ **RECOVERY** | NORECOVERY ]  
  
      [ , ] [ FILE ={ *backup_set_file_number* | @*backup_set_file_number* } ]  
  
      [ , ] MOVE '*logical_file_name_in_backup*' TO '*operating_system_file_name*' [ ,...*n* ]  
  
  }  
  
  ;  
  ```
  > [!NOTE] 
  > Lorsque vous préparez le déplacement d'une base de données vers un autre disque, vous devez vérifier que l'espace y est suffisant et identifier les collisions potentielles avec des fichiers existants. Cela suppose d'utiliser une instruction [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md) spécifiant les mêmes paramètres MOVE que ceux que vous envisagez d'utiliser dans votre instruction RESTORE DATABASE.  
  
  Le tableau suivant décrit les arguments de cette instruction RESTORE en termes de restauration d'une base de données à un nouvel emplacement. Pour plus d’informations sur ces arguments, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
  *nouveau_nom_base_de_données*  
  Nouveau nom de la base de données.  
  
  > [!NOTE]
  > Si vous restaurez la base de données vers une autre instance de serveur, vous pouvez conserver son nom d'origine au lieu d'en utiliser un nouveau.  
  
  *unité_sauvegarde* [ **,** ...*n* ]  
  Spécifie une liste séparée par des virgules de 1 à 64 unités de sauvegarde à partir desquelles la sauvegarde de la base de données sera restaurée. Vous pouvez spécifier une unité de sauvegarde physique ou une unité de sauvegarde logique correspondante, si celle-ci est définie. Pour spécifier une unité de sauvegarde physique, utilisez l'option DISK ou TAPE :  
  
  {DISQUE | TAPE} **=** _nom_unité_sauvegarde_physique_  
  
  Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
  { **RECOVERY** | NORECOVERY }  
  Si la base de données utilise le mode de récupération complète, vous devrez peut-être appliquer des sauvegardes du journal des transactions après avoir restauré la base de données. Dans ce cas, spécifiez l'option NORECOVERY.  
  
  Sinon, utilisez l'option RECOVERY, qui est la valeur par défaut.  
  
  FILE = { *numéro_fichier_jeu_sauvegarde* | @*numéro_fichier_jeu_sauvegarde_* }  
  Identifie le jeu de sauvegarde à restaurer. Ainsi, une valeur *numéro_fichier_jeu_sauvegarde* égale à **1** indique le premier jeu de sauvegarde sur le support de sauvegarde, et une valeur *numéro_fichier_jeu_sauvegarde* égale à **2** indique le second jeu. Vous pouvez obtenir le *numéro_fichier_jeu_sauvegarde* d’un jeu de sauvegarde en utilisant l’instruction [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
  Lorsque cette option n'est pas spécifiée, le comportement par défaut consiste à utiliser le premier jeu de sauvegarde de l'unité de sauvegarde.  
  
  Pour plus d’informations, consultez « Spécification d’un jeu de sauvegarde » dans [RESTORE, arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
  MOVE **’** _nom_fichier_logique_dans_sauvegarde_ **’** TO **’** _nom_fichier_système_d’exploitation_ **’** [ **,** ...*n* ]  
  Spécifie que le fichier de données ou le fichier journal spécifié par *nom_fichier_logique_dans_sauvegarde* doit être restauré à l’emplacement spécifié par *nom_fichier_système_d’exploitation*. Spécifiez une instruction MOVE pour chaque fichier logique du jeu de sauvegarde que vous voulez restaurer à un nouvel emplacement.  
  
  |Option|Description|  
  |------------|-----------------|  
  |*nom_fichier_logique_dans_sauvegarde*|Indique le nom logique d'un fichier de données ou d'un fichier journal du jeu de sauvegarde. Le nom de fichier logique d'un fichier de données ou journal dans un jeu de sauvegarde correspond au nom logique qu'il portait dans la base de données au moment de la création du jeu de sauvegarde.<br /><br /> <br /><br /> Remarque : Utilisez [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md) pour obtenir une liste des fichiers logiques contenus dans le jeu de sauvegarde.|  
  |*nom_fichier_système_d’exploitation*|Spécifie un nouvel emplacement pour le fichier spécifié par *nom_fichier_logique_dans_sauvegarde*. Le fichier sera restauré à cet emplacement.<br /><br /> Éventuellement, *nom_fichier_système_d’exploitation* spécifie un nouveau nom de fichier pour le fichier restauré. Cette option est nécessaire si vous créez une copie d'une base de données existante sur la même instance de serveur.|  
  |*n*|Est un espace réservé indiquant que vous pouvez spécifier des instructions MOVE supplémentaires.|  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple crée une base de données nommée `MyAdvWorks` en restaurant une sauvegarde de l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , qui comprend deux fichiers : [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]_Data et [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]_Log. Cette base de données utilise le mode de récupération simple. La base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] existe déjà sur l'instance de serveur, de sorte que les fichiers de la sauvegarde doivent être restaurés à un nouvel emplacement. L'instruction RESTORE FILELISTONLY permet de déterminer le nombre et le nom des fichiers de la base de données en cours de restauration. La sauvegarde de la base de données est la première sauvegarde définie sur l'unité de sauvegarde.  
  
> **REMARQUE :** Les exemples de sauvegarde et de restauration du journal des transactions, notamment les restaurations dans le temps, utilisent la base de données `MyAdvWorks_FullRM` qui est créée à partir de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , comme dans l'exemple `MyAdvWorks` suivant. Toutefois, la base de données `MyAdvWorks_FullRM` ainsi obtenue doit être modifiée pour utiliser le mode de récupération complète à l’aide de l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante : ALTER DATABASE <nom_base_de_données> SET RECOVERY FULL.  
  
```sql  
USE master;  
GO  
-- First determine the number and names of the files in the backup.  
-- AdventureWorks2012_Backup is the name of the backup device.  
RESTORE FILELISTONLY  
   FROM AdventureWorks2012_Backup;  
-- Restore the files for MyAdvWorks.  
RESTORE DATABASE MyAdvWorks  
   FROM AdventureWorks2012_Backup  
   WITH RECOVERY,  
   MOVE 'AdventureWorks2012_Data' TO 'D:\MyData\MyAdvWorks_Data.mdf',   
   MOVE 'AdventureWorks2012_Log' TO 'F:\MyLog\MyAdvWorks_Log.ldf';  
GO  
  
```  
  
 Pour voir un exemple de création d’une sauvegarde complète de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)  
  
  

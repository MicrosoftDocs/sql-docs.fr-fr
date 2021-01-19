---
title: Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure | Microsoft Docs
description: La sauvegarde d’instantanés de fichiers SQL Server utilise des instantanés Azure afin de permettre des sauvegardes rapides et des restaurations encore plus rapides pour les fichiers de base de données qui sont stockés dans le service Stockage Blob Azure.
ms.custom: ''
ms.date: 05/23/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 17a81fcd-8dbd-458d-a9c7-2b5209062f45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 63e2f722305c2af448be7d49868eef72d1a9fb8d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172691"
---
# <a name="file-snapshot-backups-for-database-files-in-azure"></a>Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sauvegarde d’instantanés de fichiers utilise des instantanés Azure pour obtenir des sauvegardes quasi instantanées et des restaurations plus rapides pour les fichiers de base de données stockés avec le service de stockage d’objets blob Azure. Cette fonctionnalité vous permet de simplifier vos stratégies de sauvegarde et de restauration. Pour une démonstration en situation réelle, consultez [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Démonstration de la restauration à un point dans le temps). Pour plus d’informations sur le stockage de fichiers de base de données à l’aide du service Stockage Blob Azure, consultez [Fichiers de données SQL Server dans Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md).  
  
 ![diagramme architectural de sauvegardes d’instantanés](../../relational-databases/backup-restore/media/snapshotbackups.PNG "diagramme architectural de sauvegardes d’instantanés")  
  
 **Télécharger**  
  
-   Pour télécharger [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], accédez au  **[Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   Vous avez un compte Azure ?  Cliquez **[ici](https://azure.microsoft.com/services/virtual-machines/sql-server/)** pour lancer une machine virtuelle déjà équipée de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
## <a name="using-azure-snapshots-to-back-up-database-files-stored-in-azure"></a>Utilisation d’instantanés Azure pour sauvegarder des fichiers de base de données stockés dans Azure  
  
### <a name="what-is-a-ssnoversion-file-snapshot-backup"></a>Qu’est-ce qu’une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ?  
 Une sauvegarde d’instantanés de fichiers se compose d’un ensemble d’instantanés Azure d’objets blob contenant les fichiers de base de données et un fichier de sauvegarde renfermant des pointeurs vers ces instantanés de fichiers. Chaque instantané de fichier est stocké dans le conteneur avec l’objet blob de base. Vous pouvez spécifier que le fichier de sauvegarde lui-même soit écrit sur un périphérique URL, sur disque ou sur bande. La sauvegarde sur un périphérique URL est recommandée. Pour plus d’informations sur la sauvegarde, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md) et sur la sauvegarde des URL, consultez [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
 ![architecture de la fonctionnalité d’instantané](../../relational-databases/backup-restore/media/snapshotbackups-flat.png "architecture de la fonctionnalité d’instantané")  
  
 En cas de suppression de l’objet blob de base, le jeu de sauvegarde est invalidé et vous ne pouvez pas supprimer un objet blob qui contient des instantanés de fichiers (sauf si vous choisissez expressément de supprimer un objet blob avec tous ses instantanés de fichiers). En outre, la suppression d’une base de données ou d’un fichier de données ne supprime pas l’objet blob de base ou l’un de ses instantanés de fichiers. De plus, la suppression du fichier de sauvegarde ne supprime pas les instantanés de fichiers dans le jeu de sauvegarde. Pour supprimer un jeu de sauvegarde de capture instantanée de fichier, utilisez la procédure stockée système **sys.sp_delete_backup** .  
  
 **Sauvegarde complète de base de données :** une sauvegarde complète de base de données à l’aide de la sauvegarde de capture instantanée de fichier crée un instantané Azure de chaque fichier de données et de journal constituant la base de données, établit la chaîne de sauvegarde du journal des transactions et écrit l’emplacement des captures instantanées de fichiers dans le fichier de sauvegarde.  
  
 **Sauvegarde du journal des transactions :** une sauvegarde du journal des transactions à l’aide de la sauvegarde de capture instantanée de fichier crée une capture instantanée de fichier de chaque fichier de base de données (pas seulement du journal des transactions), enregistre les informations d’emplacement des instantanés de fichiers dans le fichier de sauvegarde et tronque le fichier journal des transactions.  
  
> [!IMPORTANT]  
>  Après la sauvegarde complète initiale nécessaire pour établir la chaîne de sauvegarde du journal des transactions (qui peut être une sauvegarde d’instantanés de fichiers), vous devez uniquement effectuer des sauvegardes du journal des transactions, car chaque jeu de sauvegarde d’instantanés de fichiers du journal des transactions contient des instantanés de fichiers de tous les fichiers de base de données et peut être utilisé pour effectuer une restauration de base de données ou de journal. Après la sauvegarde complète de base de données initiale, aucune sauvegarde complète ou différentielle supplémentaire n’est nécessaire, car le service de stockage d’objets blob Azure gère les différences entre chaque instantané de fichier et l’état actuel de l’objet blob de base pour chaque fichier de base de données.  
  
> [!NOTE]  
>  Pour obtenir un tutoriel sur l’utilisation de SQL Server 2016 avec le service Stockage Blob Microsoft Azure, consultez [Tutoriel : Utilisation du service Stockage Blob Microsoft Azure avec des bases de données SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="restore-using-file-snapshot-backups"></a>Effectuer une restauration à l’aide de sauvegardes d’instantanés de fichiers  
 Chaque jeu de sauvegarde d’instantanés de fichiers contenant un instantané de fichier de chaque fichier de base de données, un processus de restauration requiert au plus deux jeux de sauvegarde d’instantanés de fichiers adjacents. Cela est vrai, que le jeu de sauvegarde provienne d’une sauvegarde de complète base de données ou d’une sauvegarde de fichier journal. Ce processus de restauration diffère nettement de celui qui fait appel à des fichiers de sauvegarde en continu classiques. Avec la sauvegarde en continu classique, le processus de restauration requiert l’utilisation d’une chaîne entière de jeux de sauvegarde : la sauvegarde complète, une sauvegarde différentielle et une ou plusieurs sauvegardes de journaux de transactions. La partie récupération du processus de restauration reste la même, que la restauration utilise une sauvegarde d’instantanés de fichiers ou un jeu de sauvegarde en continu.  
  
 **Au point dans le temps où a été effectué un jeu de sauvegarde :** pour exécuter une opération RESTORE DATABASE pour restaurer une base de données au point dans le temps où a été effectué un jeu de sauvegarde de capture instantanée de fichier spécifique, seul le jeu de sauvegarde spécifique est nécessaire ainsi que les objets blob de base eux-mêmes. Étant donné que vous pouvez utiliser un jeu de sauvegarde d’instantanés de fichiers du journal des transactions pour effectuer une opération RESTORE DATABASE, vous utilisez généralement un jeu de sauvegarde du journal des transactions pour exécuter ce type d’opération RESTORE DATABASE et recourez rarement à un jeu de sauvegarde complète de base de données. Un exemple illustrant cette technique est proposé à la fin de cette rubrique.  
  
 **À un point dans le temps entre deux jeux de sauvegarde de capture instantanée de fichier :** pour effectuer une opération RESTORE DATABASE pour restaurer une base de données à un point dans le temps situé entre deux jeux de sauvegarde du journal des transactions adjacents, seuls deux jeux de sauvegarde du journal des transactions sont nécessaires (un avant et un après le point dans le temps auquel vous souhaitez restaurer la base de données). Pour ce faire, vous exécutez une opération RESTORE DATABASE WITH NORECOVERY à l’aide du jeu de sauvegarde d’instantanés de fichiers du journal des transactions défini à partir du point antérieur dans le temps et effectuez une opération RESTORE LOG WITH RECOVERY à l’aide du jeu de sauvegarde d’instantanés de fichiers du journal des transactions à partir du point postérieur dans le temps et en utilisant l’argument STOPAT pour spécifier le point dans le temps auquel arrêter la récupération à partir de la sauvegarde du journal des transactions. Un exemple illustrant cette technique est proposé à la fin de cette rubrique. Pour une démonstration en situation réelle, consultez [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Démonstration de la restauration à un point dans le temps).  
  
### <a name="file-backup-set-maintenance"></a>Maintenance du jeu de sauvegarde de fichiers  
 **Suppression d’un jeu de sauvegarde de capture instantanée de fichier :** vous ne pouvez pas remplacer un jeu de sauvegarde de capture instantanée de fichier à l’aide de l’argument FORMAT. L’argument FORMAT n’est pas autorisé afin d’éviter que des instantanés de fichier qui ont été créés avec la sauvegarde de fichiers d’instantanés d’origine deviennent orphelins. Pour supprimer un jeu de sauvegarde de capture instantanée de fichier, utilisez la procédure stockée système **sys.sp_delete_backup** . Cette procédure stockée supprime le fichier de sauvegarde et les instantanés de fichiers qui composent le jeu de sauvegarde. Utiliser une autre méthode pour supprimer un jeu de sauvegarde d’instantanés de fichiers peut supprimer le fichier de sauvegarde sans supprimer les instantanés de fichiers dans le jeu de sauvegarde.  
  
 **Suppression de captures instantanées de fichiers de sauvegarde orphelines :** vous pouvez avoir des captures instantanées de fichiers orphelines si le fichier de sauvegarde a été supprimé sans utiliser la procédure stockée système **sys.sp_delete_backup** ou si une base de données ou un fichier de base de données a été supprimé alors que des captures instantanées de fichiers de sauvegarde étaient associées aux objets blob contenant cette base de données ou ce fichier de base de données. Pour identifier les captures instantanées de fichiers pouvant être orphelines, utilisez la fonction système **sys.fn_db_backup_file_snapshots** pour répertorier toutes les captures instantanées des fichiers de base de données. Pour identifier les instantanés de fichiers qui font partie d’un jeu de sauvegarde d’instantanés de fichiers spécifique, utilisez la procédure stockée système RESTORE FILELISTONLY. Vous pouvez ensuite utiliser la procédure stockée système **sys.sp_delete_backup_file_snapshot** pour supprimer une capture instantanée de fichier de sauvegarde devenue orpheline. Vous trouverez des exemples d’utilisation de cette fonction système et de ces procédures stockées système à la fin de cette rubrique. Pour plus d’informations, consultez [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md), [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md), [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) et [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
### <a name="considerations-and-limitations"></a>Observations et limitations  
 **Stockage premium :** quand vous utilisez le stockage Premium, les limitations suivantes s’appliquent :  
  
-   Le fichier de sauvegarde ne peut pas être stocké à l’aide du stockage Premium.  
  
-   La fréquence des sauvegardes ne peut être inférieure à 10 minutes.  
  
-   Le nombre maximal d’instantanés que vous pouvez stocker est 100.  
  
-   RESTORE WITH MOVE est requis.  
  
-   Pour plus d’informations sur le stockage Premium, consultez [stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](/azure/virtual-machines/disks-types).  
  
 **Compte de stockage unique :** l’instantané de fichier et les objets blob de destination doivent utiliser le même compte de stockage.  
  
 **Mode de récupération utilisant les journaux de transactions :** quand vous utilisez le mode de récupération utilisant les journaux de transactions et une sauvegarde du journal des transactions contenant des transactions à consignation minimale, vous ne pouvez pas effectuer une restauration du journal (avec récupération à un point dans le temps) à l’aide de la sauvegarde du journal des transactions. Au lieu de cela, vous exécutez une restauration de base de données au point dans le temps où a été effectué le jeu de sauvegarde d’instantanés de fichiers. Cette limitation est identique à celle qui affecte la sauvegarde en continu.  
  
 **Restauration en ligne :** quand vous utilisez des sauvegardes de captures instantanées de fichiers, vous ne pouvez pas effectuer une restauration en ligne. Pour plus d’informations sur les restaurations en ligne, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
 **Facturation :** quand vous utilisez la sauvegarde de captures instantanées de fichiers SQL Server, des frais supplémentaires s’appliquent du fait de la modification des données. Pour plus d’informations, consultez [Understanding How Snapshots Accrue Charges](/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges)(Comprendre comment les captures instantanées contribuent à l’augmentation des coûts).  
  
 **Archivage :** si vous souhaitez archiver une sauvegarde de capture instantanée de fichier, vous pouvez effectuer l’opération dans le stockage d’objets blob ou dans la sauvegarde en continu. Pour effectuer l’opération d’archivage dans le stockage d’objets blob, copiez les captures instantanées du jeu de sauvegarde de captures instantanées de fichiers dans des objets blob distincts. Pour l’exécuter dans la sauvegarde en continu, restaurez la sauvegarde d’instantanés de fichiers en tant que nouvelle base de données, puis effectuez une sauvegarde en continu normale avec compression et/ou chiffrement et archivez-la aussi longtemps que vous le souhaitez, indépendamment des objets blob de base.  
  
> [!IMPORTANT]  
>  La gestion de plusieurs sauvegardes d’instantanés de fichiers a un faible impact sur les performances. Cependant, gérer un nombre excessif de sauvegardes d’instantanés de fichiers peut affecter les performances des opérations d’E/S sur la base de données. Nous vous recommandons de ne gérer que les sauvegardes d’instantanés de fichiers nécessaires à la prise en charge de votre objectif de point de restauration.  
  
## <a name="backing-up-the-database-and-log-using-a-file-snapshot-backup"></a>Sauvegarde de la base de données et du journal à l’aide d’une sauvegarde d’instantanés de fichiers  
 L’exemple ci-dessous utilise une sauvegarde d’instantanés de fichiers pour sauvegarder l’exemple de base de données AdventureWorks2016 sur un périphérique URL.  
  
```  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2016  
   SET RECOVERY FULL;  
GO  
-- Back up the full AdventureWorks2016 database.  
BACKUP DATABASE AdventureWorks2016   
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
WITH FILE_SNAPSHOT;  
GO  
-- Back up the AdventureWorks2016 log using a time stamp in the backup file name.  
DECLARE @Log_Filename AS VARCHAR (300);  
SET @Log_Filename = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_Log_'+   
REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
BACKUP LOG AdventureWorks2016  
 TO URL = @Log_Filename WITH FILE_SNAPSHOT;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup"></a>Restauration à partir d’une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 L’exemple suivant restaure la base de données AdventureWorks2016 à l’aide d’un jeu de sauvegarde d’instantanés de fichiers du journal des transactions et illustre une opération de récupération. Notez que vous pouvez restaurer une base de données à partir d’un seul jeu de sauvegarde d’instantanés de fichiers du journal des transactions.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH RECOVERY, REPLACE;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup-to-a-point-in-time"></a>Restauration à partir d’une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un point dans le temps  
 L’exemple suivant restaure la base de données AdventureWorks2016 dans l’état qui était le sien à un point spécifique dans le temps, à l’aide de deux jeux de sauvegarde d’instantanés de fichiers du journal des transactions et illustre une opération de récupération.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH NORECOVERY,REPLACE;  
GO   
  
RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
WITH RECOVERY,STOPAT = 'May 18, 2015 5:35 PM';  
GO  
```  
  
## <a name="deleting-a-database-file-snapshot-backup-set"></a>Suppression d’un jeu de sauvegarde d’instantanés de fichiers de base de données  
 Pour supprimer un jeu de sauvegarde de capture instantanée de fichier, utilisez la procédure stockée système **sys.sp_delete_backup** . Spécifiez le nom de la base de données pour que le système vérifie que le jeu de sauvegarde d’instantanés de fichiers indiqué est effectivement une sauvegarde pour la base de données spécifiée. Si vous ne spécifiez aucun nom de base de données, le jeu de sauvegarde indiqué et ses instantanés de fichiers sont supprimés sans validation de ce type. Pour plus d’informations, consultez [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md).  
  
> [!WARNING]  
>  Si vous tentez de supprimer un jeu de sauvegarde d’instantanés de fichiers à l’aide d’une autre méthode, telle que le Portail de gestion Microsoft Azure ou la visionneuse Azure Storage dans SQL Server Management Studio, les instantanés de fichiers dans le jeu de sauvegarde ne sont pas supprimés. Ces outils suppriment uniquement le fichier de sauvegarde qui contient les pointeurs vers les instantanés de fichiers dans le jeu de sauvegarde d’instantanés de fichiers. Pour identifier les captures instantanées de fichiers de sauvegarde qui restent après la suppression incorrecte d’un fichier de sauvegarde, utilisez la fonction système **sys.fn_db_backup_file_snapshots** , puis la procédure stockée système **sys.sp_delete_backup_file_snapshot** pour supprimer une capture instantanée de fichier de sauvegarde spécifique.  
  
 L’exemple suivant supprime le jeu de sauvegarde d’instantanés de fichiers spécifié, y compris le fichier de sauvegarde et les instantanés de fichiers composant le jeu de sauvegarde indiqué.  
  
```  
sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak', 'adventureworks2016' ;  
GO  
  
```  
  
## <a name="viewing-database-backup-file-snapshots"></a>Affichage des instantanés de fichiers de sauvegarde de base de données  
 Pour afficher les captures instantanées de fichiers de l’objet blob de base pour chaque fichier de base de données, utilisez la fonction système **sys.fn_db_backup_file_snapshots** . Cette fonction système vous permet d’afficher tous les instantanés de fichiers de sauvegarde de chaque objet blob de base pour une base de données stockée à l’aide du service de stockage d’objets blob Azure. Cette fonction sert notamment à identifier les captures instantanées de fichiers de sauvegarde d’une base de données qui restent quand le fichier de sauvegarde pour un jeu de sauvegarde de captures instantanées de fichiers est supprimé à l’aide d’un mécanisme autre que la procédure stockée système **sys.sp_delete_backup** . Pour déterminer les captures instantanées de fichiers de sauvegarde qui font partie de jeux de sauvegarde intacts et celles qui n’en font pas partie, utilisez la procédure stockée système **RESTORE FILELISTONLY**  pour répertorier les captures instantanées de fichiers appartenant à chaque fichier de sauvegarde. Pour plus d’informations, consultez [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) et [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 L’exemple suivant retourne la liste de tous les instantanés de fichiers de sauvegarde pour la base de données spécifiée.  
  
```  
--Either specify the database name or set the database context  
USE AdventureWorks2016  
select * from sys.fn_db_backup_file_snapshots (null) ;  
GO  
select * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016') ;  
GO  
  
```  
  
## <a name="deleting-an-individual-database-backup-file-snapshot"></a>Suppression d’un instantané de fichier de sauvegarde de base de données  
 Pour supprimer une capture instantanée de fichier de sauvegarde d’un objet blob de base de base de données, utilisez la procédure stockée système **sys.sp_delete_backup_file_snapshot** . Cette procédure stockée système sert notamment à supprimer les fichiers de captures instantanées de fichiers orphelines qui restent après la suppression d’un fichier de sauvegarde à l’aide d’une méthode autre que la procédure stockée système **sys.sp_delete_backup** . Pour plus d’informations, consultez [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md).  
  
> [!WARNING]  
>  La suppression d’un instantané de fichier qui fait partie d’un jeu de sauvegarde d’instantanés de fichiers invalide le jeu de sauvegarde.  
  
 L’exemple suivant supprime l’instantané de fichiers de sauvegardes spécifié. L’URL de la sauvegarde spécifiée a été obtenue à l’aide de la fonction système **sys.fn_db_backup_file_snapshots** .  
  
```  
sys.sp_delete_backup_file_snapshot N'adventureworks2016', N'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016Data.mdf?snapshot=2015-05-29T21:31:31.6502195Z';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Tutoriel : Utilisation du service Stockage Blob Microsoft Azure avec des bases de données SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  

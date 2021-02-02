---
title: Restaurations de fichiers (mode de récupération complète) | Microsoft Docs
description: Une restauration de fichiers dans SQL Server est une séquence de restauration unique qui copie, restaure par progression et récupère un ou plusieurs fichiers de données sans restaurer l’intégralité de la base de données.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- full recovery model [SQL Server], performing restores
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- file restores [SQL Server], full recovery model
- restoring files [SQL Server], full recovery model
- Transact-SQL restore sequence
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: d2236a2a-4cf1-4c3f-b542-f73f6096e15c
author: cawrites
ms.author: chadam
ms.openlocfilehash: fd5fa04b32d9c00f810b61bbca81963e21bd5697
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076877"
---
# <a name="file-restores-full-recovery-model"></a>Restaurations de fichiers (mode de récupération complète)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique concerne uniquement les bases de données contenant plusieurs fichiers ou groupes de fichiers en modes de restauration complète ou de récupération utilisant les journaux de transactions.  
  
 Le but d'une restauration de fichiers est de restaurer un ou plusieurs fichiers endommagés sans restaurer l'ensemble de la base de données. Un scénario de restauration de fichiers consiste en une séquence de restauration unique qui copie, restaure par progression et récupère les données appropriées.  
  
 Si le groupe de fichiers en cours de restauration est en lecture-écriture, une séquence ininterrompue de sauvegardes de journal doit être appliquée après la restauration de la dernière sauvegarde de données ou différentielle. Cette opération restaure le groupe de fichiers par progression jusqu'aux enregistrements de journal dans les enregistrements actifs en cours du fichier journal. Le point de récupération est généralement situé vers la fin du journal, mais pas nécessairement.  
  
 Si le groupe de fichiers en cours de restauration est en lecture seule, l'application de sauvegardes de journal est souvent inutile et ignorée. Si la sauvegarde a été réalisée après la mise en lecture seule du fichier, il s'agit de la dernière sauvegarde à restaurer. La récupération par progression s'arrête au point cible.  
  
 Les scénarios de restauration de fichiers sont les suivants :  
  
-   Restauration de fichiers hors ligne  
  
     Dans une *restauration de fichiers hors ligne*, la base de données est hors connexion pendant la restauration des fichiers ou des groupes de fichiers endommagés. À la fin de la séquence de restauration, la base de données est mise en ligne.  
  
     Toutes les éditions de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prennent en charge la restauration de fichiers hors connexion.  
  
-   Restauration de fichiers en ligne  
  
     Dans une *restauration de fichiers en ligne*, si la base de données est en ligne au moment de la restauration, elle reste en ligne durant la restauration de fichiers. Toutefois, chaque groupe de fichiers dans lequel un fichier est restauré est hors connexion pendant l'opération de restauration. Une fois que tous les fichiers d'un groupe de fichiers hors connexion sont récupérés, le groupe de fichiers est automatiquement mis en ligne.  
  
     Pour plus d’informations sur la prise en charge de la restauration de fichiers et de pages en ligne, consultez [Éditions et fonctionnalités prises en charge dans SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). Pour plus d’informations sur les restaurations en ligne, consultez [Restauration en ligne (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md).
  
    > [!TIP]  
    >  Si vous voulez que la base de données soit hors connexion pour une restauration de fichiers, mettez celle-ci hors connexion avant de démarrer la séquence de restauration en exécutant l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) suivante : ALTER DATABASE *nom_base_de_données* SET OFFLINE.  
  
  
##  <a name="restoring-damaged-files-from-file-backups"></a><a name="Overview"></a> Restauration de fichiers endommagés à partir de sauvegardes de fichiers  
  
1.  Avant de restaurer un ou plusieurs fichiers endommagés, essayez de créer une [sauvegarde de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
     Si l'enregistrement a été endommagé, il est impossible de créer une sauvegarde de la fin du journal, et vous devez restaurer la base de données.  
  
     Pour plus d’informations sur la sauvegarde d’un journal de transactions, consultez [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Lors d'une restauration de fichiers hors connexion, vous devez toujours effectuer une sauvegarde de la fin du journal avant de restaurer les fichiers. Lors d'une restauration de fichiers en ligne, vous devez toujours effectuer la sauvegarde de journal après la restauration des fichiers Cette opération est nécessaire pour permettre au fichier d'être récupéré dans un état cohérent avec le reste de la base de données.  
  
2.  Restaurez chaque fichier endommagé à partir de la sauvegarde la plus récente de ce fichier.  
  
3.  Restaurez la sauvegarde différentielle de fichiers la plus récente, si elle existe, pour chaque fichier restauré.  
  
4.  Restaurez dans l'ordre les sauvegardes du journal des transactions, en commençant par la sauvegarde correspondant aux fichiers restaurés les plus anciens jusqu'à la sauvegarde de la fin du journal créée à l'étape 1.  
  
     Vous devez restaurer les sauvegardes du journal des transactions créées après les sauvegardes de fichiers pour rendre à la base de données sa cohérence. Les sauvegardes du journal des transactions peuvent être rapidement restaurées par progression, car seuls les changements concernant les fichiers restaurés sont appliqués. La restauration de fichiers individuels est parfois une meilleure option que la restauration intégrale d'une base de données parce que les fichiers non endommagés ne sont pas copiés puis restaurés par progression. Cependant, la lecture de la séquence complète des sauvegardes de journaux reste nécessaire.  
  
5.  Récupérez la base de données.  

> [!NOTE]  
>  Les sauvegardes de fichiers peuvent également servir à restaurer la base de données à un état antérieur dans le temps. Pour cela, vous devez restaurer un jeu complet de sauvegardes de fichiers, puis restaurer dans l'ordre les sauvegardes du journal des transactions pour atteindre un moment cible postérieur à la fin de la dernière sauvegarde de fichiers restaurée. Pour plus d’informations sur la récupération jusqu’à une date et heure, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
## <a name="transact-sql-restore-sequence-for-an-offline-file-restore-full-recovery-model"></a>Séquence de restauration Transact-SQL pour une restauration de fichiers hors connexion (mode de restauration complète)  
 Un scénario de restauration de fichiers consiste en une séquence de restauration unique qui copie, restaure par progression et récupère les données appropriées.  
  
 Cette section présente les options [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) essentielles pour une séquence de restauration de fichiers. La syntaxe et les détails qui ne sont pas pertinents sont omis.  
  
 L'exemple de séquence de restauration suivant illustre une restauration hors ligne de deux fichiers secondaires, `A` et `B`, à l'aide de WITH NORECOVERY. Ensuite, deux sauvegardes de journal sont appliquées à l'aide de NORECOVERY, suivies de la sauvegarde de la fin du journal qui est restaurée à l'aide de WITH RECOVERY.  
  
> [!NOTE]  
>  L'exemple de séquence de restauration suivant commence par placer le fichier hors connexion, puis crée une sauvegarde de la fin du journal.  
  
```  
--Take the file offline.  
ALTER DATABASE database_name MODIFY FILE SET OFFLINE;  
-- Back up the currently active transaction log.  
BACKUP LOG database_name  
   TO <tail_log_backup>  
   WITH NORECOVERY;  
GO   
-- Restore the files.  
RESTORE DATABASE database_name FILE=name   
   FROM <file_backup_of_file_A>   
   WITH NORECOVERY;  
RESTORE DATABASE database_name FILE=<name> ......  
   FROM <file_backup_of_file_B>   
   WITH NORECOVERY;  
-- Restore the log backups.  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <log_backup>   
   WITH NORECOVERY;  
RESTORE LOG database_name FROM <tail_log_backup>   
   WITH RECOVERY;  
```  
  
## <a name="examples"></a>Exemples  
  
-   [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
-   [Exemple : restauration hors ligne du groupe de fichiers primaire et d’un autre groupe de fichiers &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour restaurer des fichiers et des groupes de fichiers**  
  
-   [Restaurer des fichiers à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.SqlRestore%2A> (SMO)  
  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration : interopérabilité et coexistence &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  

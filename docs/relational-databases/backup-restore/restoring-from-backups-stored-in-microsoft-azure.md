---
title: Restauration à partir de sauvegardes stockées dans Windows Azure | Microsoft Docs
description: Découvrez les considérations relatives à la restauration d’une base de données SQL Server effectuée à l’aide d’une sauvegarde stockée dans le stockage Blob Azure.
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6ae358b2-6f6f-46e0-a7c8-f9ac6ce79a0e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5329181957e09e169caae74f9611a2426f377f7b
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125504"
---
# <a name="restoring-from-backups-stored-in-microsoft-azure"></a>Restauration à partir de sauvegardes stockées dans Windows Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique présente les éléments à prendre en considération quand vous restaurez une base de données à l’aide d’une sauvegarde stockée dans le service de stockage Blob Azure. Ceci s'applique aux sauvegardes créées à l'aide de la sauvegarde SQL Server vers l'URL ou par la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Nous vous recommandons de consulter cette rubrique si vous avez des sauvegardes stockées dans le service de stockage Blob Azure et que vous planifiez de les restaurer. Lisez ensuite la rubrique qui explique comment restaurer une base de données dont les sauvegardes sont locales ou sur le cloud, les deux méthodes étant identiques.  
  
## <a name="overview"></a>Vue d’ensemble  
 Les outils et les méthodes utilisés pour restaurer une base de données à partir d'une sauvegarde local s'appliquent également à la restauration d'une base de données depuis une sauvegarde sur le cloud.  Les sections suivantes présentent ces considérations et les différences que vous devez connaître lorsque vous utilisez des sauvegardes stockées dans le service de stockage Blob Azure.  
  
### <a name="using-transact-sql"></a>Utilisation de Transact-SQL  
  
-   Étant donné que SQL Server doit se connecter à une source externe pour récupérer les fichiers de sauvegarde, les informations d'identification SQL sont utilisées pour authentifier le compte de stockage. Par conséquent, l'instruction RESTORE nécessite l'option WITH CREDENTIAL. Pour plus d’informations, voir [SQL Server Backup and Restore with Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)(en anglais).  
  
-   Si vous utilisez une [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour gérer vos sauvegardes dans le cloud, vous pouvez passer en revue toutes les sauvegardes disponibles dans le stockage, en utilisant la fonction système **smart_admin.fn_available_backups** . Cette fonction système retourne toutes les sauvegardes disponibles pour une base de données dans une table. Comme les résultats sont retournés dans une table, vous pouvez les filtres ou les trier. Pour plus d’informations, consultez [managed_backup.fn_available_backups &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md).  
  
### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio  
  
-   La tâche de restauration est utilisée pour restaurer une base de données à l'aide de SQL Server Management Studio. La page de support de sauvegarde comprend maintenant l’option **URL** pour afficher les fichiers de sauvegarde stockés dans le service de stockage Blob Azure. Vous devez également fournir les informations d'identification SQL utilisées par vous authentifier sur le compte de stockage. La grille **Jeux de sauvegarde à restaurer** est ensuite remplie avec les sauvegardes disponibles dans le stockage Blob Azure. Pour plus d’informations, consultez [Restauration à partir du stockage Azure à l’aide de SQL Server Management Studio](../../relational-databases/backup-restore/sql-server-backup-to-url.md#RestoreSSMS).  
  
### <a name="optimizing-restores"></a>Optimisation des restaurations  
 Pour réduire le temps d’écriture des restaurations, ajoutez le droit d’utilisateur **Effectuer les tâches de maintenance de volume** au compte d’utilisateur SQL Server. Pour plus d’informations, consultez [Initialisation des fichiers de base de données](/previous-versions/sql/sql-server-2008-r2/ms175935(v=sql.105)). Si la restauration est toujours lente avec l'initialisation instantanée des fichiers activée, examinez la taille du fichier journal sur l'instance où la base de données a été sauvegardée. Si le fichier journal est de très grande taille (plusieurs Go), il faut s'attendre à ce que la restauration soit lente. Pendant la restauration, le fichier journal doit être remis à zéro, ce qui prend beaucoup de temps.  
  
 Pour réduire les durées de restauration, il est recommandé d'utiliser des sauvegardes compressées.  Pour des tailles de sauvegarde de plus de 25 Go, utilisez l’ [utilitaire AzCopy](/archive/blogs/windowsazurestorage/azcopy-uploadingdownloading-files-for-windows-azure-blobs) pour un téléchargement sur le disque local, puis effectuez la restauration. Pour connaître les bonnes pratiques et obtenir des recommandations, consultez [Meilleures pratiques et dépannage de sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md).  
  
 Vous pouvez également activer l'indicateur de trace 3051 lors de la restauration afin de générer un journal détaillé. Ce fichier journal se trouve dans le répertoire du journal et son nom est au format suivant : BackupToUrl-\<instancename>-\<dbname>-action-\<PID>.log. Le fichier journal contient des informations sur chaque aller-retour dans le Stockage Azure, y compris le délai d’attente qui peut être utile lors du diagnostic de problèmes.  
  
### <a name="topics-on-performing-restore-operations"></a>Rubriques relatives aux procédures des opérations de restauration  
  
-   [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)  
  
-   [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)  
  
-   [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)  
  
-   [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  

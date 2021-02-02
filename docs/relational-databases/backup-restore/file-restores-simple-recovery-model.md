---
title: Restauration de fichiers (mode de récupération simple) | Microsoft Docs
description: Dans SQL Server, une restauration de fichiers s’applique à un ou plusieurs fichiers endommagés et ne porte pas sur l’ensemble de la base de données.
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- simple recovery model [SQL Server]
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- Transact-SQL restore sequence
- restoring files [SQL Server], simple recovery model
- file restores [SQL Server], simple recovery model
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: b6d07386-7c6f-4cc6-be32-93289adbd3d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e2df8edb5b31ff2ffcd45eba9b5d9cf234c528d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076887"
---
# <a name="file-restores-simple-recovery-model"></a>Restauration de fichiers (mode de récupération simple)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique ne concerne que les bases de données en mode simple contenant au moins un groupe de fichiers secondaire en lecture seule.  
  
 Le but d'une restauration de fichiers est de restaurer un ou plusieurs fichiers endommagés sans restaurer l'ensemble de la base de données. Dans le cadre du mode de récupération simple, les sauvegardes de fichiers sont prises en charge uniquement pour les fichiers en lecture seule. Le groupe de fichiers primaire et les groupes de fichiers secondaires en lecture-écriture sont toujours restaurés conjointement lors de la restauration d'une sauvegarde partielle ou d'une base de données.  
  
 Les scénarios de restauration de fichiers sont les suivants :  
  
-   Restauration de fichiers hors ligne  
  
     Dans une *restauration de fichiers hors ligne*, la base de données est hors connexion pendant la restauration des fichiers ou des groupes de fichiers endommagés. À la fin de la séquence de restauration, la base de données est mise en ligne.  
  
     Toutes les éditions de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prennent en charge la restauration de fichiers hors connexion.  
  
-   Restauration de fichiers en ligne  
  
     Dans une *restauration de fichiers en ligne*, si la base de données est en ligne au moment de la restauration, elle reste en ligne durant la restauration de fichiers. Toutefois, chaque groupe de fichiers dans lequel un fichier est restauré est hors connexion pendant l'opération de restauration. Une fois que tous les fichiers d'un groupe de fichiers hors connexion sont récupérés, le groupe de fichiers est automatiquement mis en ligne.  
  
     Pour plus d’informations sur la prise en charge de la restauration de fichiers et de pages en ligne, consultez [Fonctionnalités et tâches du moteur de base de données](../../sql-server/what-s-new-in-sql-server-ver15.md). Pour plus d’informations sur les restaurations en ligne, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
    > [!TIP]  
    >  Si vous voulez que la base de données soit hors connexion pour une restauration de fichiers, mettez celle-ci hors connexion avant de démarrer la séquence de restauration en exécutant l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) suivante : ALTER DATABASE *nom_base_de_données* SET OFFLINE.  
  
 **Dans cette rubrique :**  
  
-   [Vue d'ensemble de la restauration de fichiers et de groupes de fichiers en mode de récupération simple](#Overview)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="overview-of-file-and-filegroup-restore-under-the-simple-recovery-model"></a><a name="Overview"></a> Vue d'ensemble de la restauration de fichiers et de groupes de fichiers en mode de récupération simple  
 Un scénario de restauration de fichiers consiste en une séquence de restauration unique qui copie, restaure par progression et récupère les données appropriées comme suit :  
  
1.  Restauration de chaque fichier endommagé à partir de sa toute dernière sauvegarde.  
  
2.  Restauration de la toute dernière sauvegarde de fichiers différentielle de chaque fichier restauré et récupération de la base de données.  
  
### <a name="transact-sql-steps-for-file-restore-sequence-simple-recovery-model"></a>Étapes Transact-SQL pour une séquence de restauration de fichier (mode de récupération simple)  
 Cette section présente les options [!INCLUDE[tsql](../../includes/tsql-md.md)][RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) essentielles pour une séquence de restauration de fichiers simple. La syntaxe et les détails qui ne sont pas pertinents sont omis.  
  
 La séquence de restauration contient uniquement deux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . La première instruction restaure un fichier secondaire, le fichier `A`, qui est restauré avec WITH NORECOVERY. La seconde opération restaure deux autres fichiers, `B` et `C` , qui sont restaurés avec WITH RECOVERY depuis une unité de sauvegarde différente :  
  
1.  RESTORE DATABASE *base_de_données* FILE **=** _nom_fichier_A_  
  
     FROM *sauvegarde_de_fichier_A*  
  
     WITH NORECOVERY **;**  
  
2.  RESTORE DATABASE *base_de_données* FILE **=** _nom_fichier_B_ **,** _nom_fichier_C_  
  
     FROM *sauvegarde_des_fichiers_B_et_C*  
  
     WITH RECOVERY **;**  
  
### <a name="examples"></a>Exemples  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Exemple : restauration hors ligne du groupe de fichiers primaire et d’un autre groupe de fichiers &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour restaurer des fichiers et des groupes de fichiers**  
  
-   [Restaurer des fichiers et groupes de fichiers en remplaçant des fichiers existants &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restore.SqlRestore, méthode (Serveur) (SMO)](/dotnet/api/microsoft.sqlserver.management.smo.restore.sqlrestore)   
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration : interopérabilité et coexistence &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  

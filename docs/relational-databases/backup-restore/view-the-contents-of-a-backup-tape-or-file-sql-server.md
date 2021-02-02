---
title: Afficher le contenu de la sauvegarde (fichier ou bande)
description: Cet article explique comment afficher le contenu d’une bande ou d’un fichier de sauvegarde dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], tapes
- displaying backup content
- viewing backup content
- tape backup devices, viewing contents
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
ms.assetid: cd6674a2-ca55-4b5a-a971-878ba001821e
author: cawrites
ms.author: chadam
ms.openlocfilehash: c50e7b4a46e7abf2f0c99720b622057ed3f29729
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076867"
---
# <a name="view-the-contents-of-a-backup-tape-or-file-sql-server"></a>Afficher le contenu d’un fichier ou d’une bande de sauvegarde (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment afficher le contenu d'un fichier ou d'une bande de sauvegarde dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  La prise en charge des unités de sauvegarde sur bande sera supprimée dans une prochaine version de SQL Server. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher le contenu d'un fichier ou d'une bande de sauvegarde, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Pour plus d’informations sur la sécurité, consultez [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, l'obtention d'informations relatives à un jeu de sauvegarde ou une unité de sauvegarde requiert l'autorisation CREATE DATABASE. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-content-of-a-backup-tape-or-file"></a>Pour afficher le contenu d'un fichier ou d'une bande de sauvegarde  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données à sauvegarder, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  
  
4.  Dans la section **Destination** de la page **Général** , cliquez sur **Disque** ou sur **Bande**. Dans la zone de liste **Sauvegarde sur** , recherchez le fichier disque ou la bande souhaitée.  
  
     Si le fichier disque ou la bande ne figure pas dans la zone de liste, cliquez sur **Ajouter**. Sélectionnez un nom de fichier ou un lecteur de bande. Pour l’ajouter à la zone de liste **Sauvegarde sur** , cliquez sur **OK**.  
  
5.  Dans la zone de liste **Sauvegarde sur** , sélectionnez le chemin d’accès au disque ou au lecteur de bande que vous souhaitez afficher et cliquez sur **Sommaire**. La boîte de dialogue **Contenu de l'unité** s'ouvre.  
  
6.  Le volet droit affiche des informations relatives au support de sauvegarde et aux jeux de sauvegarde sur la bande ou le fichier sélectionné.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-content-of-a-backup-tape-or-file"></a>Pour afficher le contenu d'un fichier ou d'une bande de sauvegarde  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Utilisez l'instruction [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) . Cet exemple retourne des informations de base sur le fichier nommé `AdventureWorks2012-FullBackup.bak`.  
  
```sql  
USE AdventureWorks2012;  
RESTORE HEADERONLY   
FROM DISK = N'C:\AdventureWorks2012-FullBackup.bak' ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
  

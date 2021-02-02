---
title: Définir une unité logique de sauvegarde - disque
description: Cet article explique comment définir une unité de sauvegarde logique pour un fichier sur disque dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], defining
- backup devices [SQL Server], disks
- disk backup devices [SQL Server]
- database backups [SQL Server], disks
- backing up databases [SQL Server], disks
ms.assetid: 86331d43-c738-4523-ae3d-7d6700348ed1
author: cawrites
ms.author: chadam
ms.openlocfilehash: fffcdfb7ad88a2bf3b8c72f9a15b4ff30b796166
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075971"
---
# <a name="define-a-logical-backup-device-for-a-disk-file-sql-server"></a>Définir une unité de sauvegarde logique pour un fichier de disque (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment définir une unité de sauvegarde logique pour un fichier de disque dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Une unité logique est un nom défini par l'utilisateur qui désigne une unité de sauvegarde physique spécifique (un fichier de disque ou un lecteur de bande).  L'initialisation de l'unité physique se produit ultérieurement, lorsqu'une sauvegarde est écrite sur l'unité de sauvegarde.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour définir une unité de sauvegarde logique pour un fichier de disque, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le nom d'unité logique doit être unique parmi toutes les unités de sauvegarde logiques résidant sur l'instance de serveur. Pour afficher les noms d’unités logiques existantes, interrogez l’affichage catalogue [sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md) .  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   À titre de recommandation, il est préférable que le disque de sauvegarde ne soit pas le même que les disques de données ou de journal de la base de données. Ce paramètre est primordial pour garantir l'accès aux sauvegardes en cas d'échec du disque de données ou de journal.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **diskadmin** .  
  
 Requiert l'autorisation d'écrire sur le disque.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-define-a-logical-backup-device-for-a-disk-file"></a>Pour définir une unité de sauvegarde logique pour un fichier de disque  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Objets serveur**, puis cliquez avec le bouton droit sur **Unités de sauvegarde**.  
  
3.  Cliquez sur **Nouvelle unité de sauvegarde**. La boîte de dialogue **Unité de sauvegarde** s'ouvre.  
  
4.  Entrez un nom d'unité.  
  
5.  Pour la destination, cliquez sur **Fichier** et spécifiez le chemin d'accès complet du fichier.  
  
6.  Pour définir la nouvelle unité, cliquez sur **OK**.  
  
 Pour procéder à une sauvegarde sur cette nouvelle unité, ajoutez-la au champ **Sauvegarde sur** dans la boîte de dialogue **Sauvegarder la base de données** (**Général**). Pour plus d’informations, consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-define-a-logical-backup-for-a-disk-file"></a>Pour définir une unité logique pour un fichier de disque  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) pour définir une unité de sauvegarde logique pour un fichier de disque. Cet exemple ajoute l'unité de sauvegarde sur disque appelée `mydiskdump`, dont le nom physique est `c:\dump\dump1.bak`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak' ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [Afficher les propriétés et le contenu d’une unité de sauvegarde logique &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
  

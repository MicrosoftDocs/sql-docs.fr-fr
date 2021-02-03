---
title: Afficher les fichiers de données et les fichiers journaux dans un jeu de sauvegarde
description: Découvrez comment afficher les fichiers de données et les fichiers journaux d’un jeu de sauvegarde dans SQL Server, à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], viewing backup sets
- viewing backup set information
- backup sets [SQL Server], viewing files in
- displaying backup set information
- transaction log backups [SQL Server], viewing backup sets
- backing up [SQL Server], viewing backup sets
ms.assetid: abb6420c-f809-426e-aeb4-d0a74989cf39
author: cawrites
ms.author: chadam
ms.openlocfilehash: b96e066b275ebd406fcecd81c185a65ac65abc22
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250355"
---
# <a name="view-the-data-and-log-files-in-a-backup-set-sql-server"></a>Afficher les fichiers de données et les fichiers journaux dans un jeu de sauvegarde (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment afficher les fichiers de données et les fichiers journaux d'un jeu de sauvegarde dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher les fichiers de données et les fichiers journaux dans un jeu de sauvegarde, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Pour plus d’informations sur la sécurité, consultez [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, l'obtention d'informations relatives à un jeu de sauvegarde ou une unité de sauvegarde requiert l'autorisation CREATE DATABASE. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-data-and-log-files-in-a-backup-set"></a>Pour afficher les données et les fichiers journaux dans un jeu de sauvegarde  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit de la souris sur la base de données, puis cliquez sur **Propriétés** pour ouvrir la boîte de dialogue **Propriétés de la base de données** .  
  
4.  Dans le volet **Sélectionnez une page** , cliquez sur **Fichiers**.  
  
5.  Dans la grille **Fichiers de la base de données** , recherchez la liste des fichiers de données et des fichiers journaux et leurs propriétés.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-data-and-log-files-in-a-backup-set"></a>Pour afficher les données et les fichiers journaux dans un jeu de sauvegarde  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Utilisez l'instruction [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md) . Cet exemple retourne des informations sur le deuxième jeu de sauvegarde (`FILE=2`) sur l'unité de sauvegarde `AdventureWorksBackups` .  
  
```sql  
USE AdventureWorks2012 ;  
RESTORE FILELISTONLY FROM AdventureWorksBackups   
   WITH FILE=2;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  

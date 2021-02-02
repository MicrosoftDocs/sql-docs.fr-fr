---
title: Supprimer des groupes de fichiers obsolètes (SQL Server) | Microsoft Docs
description: Cet article explique comment supprimer des groupes de fichiers défunts dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], defunct filegroups
- defunct filegroups
- restoring filegroups [SQL Server]
- deferred transactions
- filegroups [SQL Server], defunct
- unrestored filegroups
ms.assetid: 055f9c6a-5c18-4942-98e7-ec918f0ff975
author: cawrites
ms.author: chadam
ms.openlocfilehash: 88f08e8f660fc2887f38395e2737ee79b8bccefb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077047"
---
# <a name="remove-defunct-filegroups-sql-server"></a>Supprimer des groupes de fichiers obsolètes (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment supprimer des groupes de fichiers obsolètes dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer des groupes de fichiers obsolètes, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Cette rubrique concerne les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contiennent plusieurs fichiers ou groupes de fichiers et, dans le mode simple, seulement les groupes de fichiers en lecture seule.  
  
-   Tous les fichiers d'un groupe de fichiers prennent l'état « ancien » quand un groupe de fichiers hors connexion est supprimé.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Si un groupe de fichiers non restauré ne doit jamais faire l'objet d'une restauration, vous pouvez rendre le groupe de fichiers *obsolète* en le supprimant de la base de données. Le groupe de fichiers obsolète ne peut jamais être restauré dans cette base de données, mais ses métadonnées restent. Une fois le groupe de fichiers obsolète, la base de données peut être redémarrée et la récupération assurera la cohérence de la base de données au sein des groupes de fichiers restaurés.  
  
     Ainsi, rendre obsolète un groupe de fichiers permet de résoudre les transactions différées causées par un groupe de fichiers hors connexion qui n'a plus sa place dans la base de données. Les transactions qui étaient différées en raison du groupe de fichiers hors connexion quittent l'état différé une fois que le groupe de fichiers est obsolète. Pour plus d’informations, consultez [Transactions différées &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-remove-defunct-filegroups"></a>Pour supprimer des groupes de fichiers obsolètes  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez le dossier **Bases de données**, cliquez avec le bouton droit sur la base de données de laquelle vous souhaitez supprimer le fichier, puis cliquez sur **Propriétés**.  
  
3.  Sélectionnez la page **Fichiers** .  
  
4.  Dans la grille **Fichiers de la base de données** , sélectionnez les fichiers à supprimer, cliquez sur **Supprimer**, puis sur **OK**.  
  
5.  Sélectionnez la page **Groupes de fichiers** .  
  
6.  Dans la grille **Lignes** , sélectionnez le groupe de fichiers à supprimer, cliquez sur **Supprimer**, puis sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-remove-defunct-filegroups"></a>Pour supprimer des groupes de fichiers obsolètes  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. (**Remarque :** Cet exemple part du principe que les fichiers et le groupe de fichiers existent déjà. Pour créer ces objets, consultez l’exemple B dans la rubrique [Options de fichiers et de groupes de fichiers ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).) Le premier exemple supprime les fichiers `test1dat3` et `test1dat4` du groupe de fichiers obsolète à l'aide de l'instruction `ALTER DATABASE` avec la clause `REMOVE FILE`. Le deuxième exemple supprime le groupe de fichiers obsolète `Test1FG1`à l'aide de la clause `REMOVE FILEGROUP` .  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat3 ;  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat4 ;  
GO  
  
```  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILEGROUP Test1FG1 ;  
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
 [Transactions différées &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  

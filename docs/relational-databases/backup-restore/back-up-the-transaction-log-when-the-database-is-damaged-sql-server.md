---
title: Sauvegarder le journal des transactions quand la base de données est endommagée (SQL Server) | Microsoft Docs
description: Cette rubrique explique comment sauvegarder un journal des transactions dans SQL Server lorsque la base de données est endommagée, à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], damaged
- backing up [SQL Server]. damaged database
- transaction log backups [SQL Server], damaged databases
ms.assetid: 9b8873cc-df54-4336-ab9b-8f525132c2b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e03f51fa7a90e7a547e85f226148870043d89a0e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077087"
---
# <a name="back-up-the-transaction-log-when-the-database-is-damaged-sql-server"></a>Sauvegarder le journal des transactions lorsque la base de données est endommagée (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment sauvegarder un journal des transactions lorsque la base de données est endommagée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour sauvegarder le journal des transactions lorsque la base de données est endommagée, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Pour une base de données en mode de récupération complète ou utilisant les journaux de transactions, vous devez généralement effectuer une sauvegarde de la fin du journal avant d'entamer la restauration de la base de données. Vous devez également effectuer une sauvegarde de la fin du journal de la base de données primaire avant de basculer vers une configuration de la copie des journaux de transaction. La restauration de la sauvegarde de la fin du journal en tant que sauvegarde finale de fichier journal avant la récupération de la base de données permet d'éviter les pertes de données après une défaillance. Pour plus d’informations sur les sauvegardes de la fin du journal, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-back-up-the-tail-of-the-transaction-log"></a>Pour effectuer une sauvegarde de fichier journal de transactions après défaillance  

1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  
  
4.  Dans la zone de liste **Base de données** , vérifiez le nom de la base de données. Vous pouvez éventuellement sélectionner une autre base de données dans la liste.  
  
5.  Vérifiez que le mode de récupération est **FULL** ou **BULK_LOGGED**.  
  
6.  Dans la zone de liste **Type de sauvegarde** , sélectionnez **Journal des transactions**.  
  
7.  Laissez l'option **Sauvegarde de copie uniquement** désactivée.  
  
8.  Dans la zone **Jeu de sauvegarde** , acceptez le nom du jeu de sauvegarde par défaut proposé dans la zone de texte **Nom** , ou attribuez-lui un autre nom.  
  
9. Dans la zone de texte **Description** , entrez une description de la sauvegarde de la fin du journal.  
  
10. Indiquez quand le jeu de sauvegarde arrivera à expiration :  
  
    -   Pour que le jeu de sauvegarde expire au bout d’un nombre de jours spécifique, cliquez sur **Après** (option par défaut) et entrez le nombre de jours souhaité pour l’expiration du jeu après sa création. Cette valeur doit être comprise entre 0 et 99999 jours ; une valeur de 0 jour signifie que le jeu de sauvegarde n'expirera jamais.  
  
         La valeur par défaut est définie dans l’option **Délai de rétention par défaut du support de sauvegarde (jours)** de la boîte de dialogue **Propriétés du serveur** (page **Paramètres de base de données** ). Pour accéder à cette base de données, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets et sélectionnez Propriétés. Ensuite, sélectionnez la page **Paramètres de base de données** .  
  
    -   Pour que le jeu de sauvegarde expire à une date spécifique, cliquez sur **Le** et entrez la date d'expiration souhaitée.  
  
11. Choisissez le type de destination de la sauvegarde : **Disque** ou **Bande**. Pour sélectionner les chemins d'accès à 64 lecteurs de bande ou de disque au maximum contenant un seul support de sauvegarde, cliquez sur **Ajouter**. Les chemins d'accès sélectionnés apparaissent dans la zone de liste **Sauvegarde sur** .  
  
     Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d'une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Sommaire**.  
  
12. Sur la page **Options** , sélectionnez une option **Remplacer le support** en cliquant sur un des éléments suivants :  
  
    -   **Sauvegarder sur le support de sauvegarde existant**  
  
         Pour cette option, cliquez sur **Ajouter au jeu de sauvegarde existant** ou sur **Remplacer tous les jeux de sauvegarde existants**.  
  
         Vous pouvez aussi activer la case à cocher **Vérifier le nom du support de sauvegarde et la date d'expiration du jeu de sauvegarde** pour forcer l'opération de sauvegarde à vérifier la date et l'heure de l'expiration du jeu de supports ou du jeu de sauvegarde.  
  
         Vous pouvez éventuellement entrer un nom dans la zone de texte **Nom du support de sauvegarde** . Si aucun nom n'est spécifié, un support de sauvegarde avec un nom vide est créé. Si vous spécifiez un nom pour le support de sauvegarde, ce support (bande ou disque) est vérifié pour voir si le nom réel correspond bien au nom que vous entrez ici.  
  
         Si vous n'entrez pas de nom et que vous activez la case à cocher pour demander la vérification par rapport au support, le nom du support sera également vide sur le support.  
  
    -   **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**  
  
         Pour cette option, entrez un nom dans la zone de texte **Nouveau nom du support de sauvegarde** et décrivez éventuellement le jeu de supports dans la zone de texte **Description du nouveau support de sauvegarde** .  
  
     Pour plus d’informations sur les options de supports de sauvegarde, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
13. Dans la section **Fiabilité** , vous pouvez éventuellement activer les cases à cocher :  
  
    -   **Vérifier la sauvegarde en fin d'opération**;  
  
    -   **Effectuer une somme de contrôle avant d'écrire sur le support**.  
  
    -   **Continuer lors d'erreurs de somme de contrôle**  
  
     Pour plus d’informations sur les sommes de contrôle, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
14. Dans la section **Journal des transactions** , activez la case à cocher **Sauvegarder la fin du journal et laisser la base de données dans l'état de restauration**.  
  
     Cela équivaut à spécifier l'instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) suivante :  
  
     `BACKUP LOG <database_name> TO <backup_device> WITH NORECOVERY`  
  
    > [!IMPORTANT]  
    >  Lors de la restauration, la boîte de dialogue Restaurer la base de données affiche le type de sauvegarde de la fin du journal en tant que **Journal des transactions (copie uniquement)** .  
  
15. Si vous effectuez la sauvegarde sur un lecteur de bande (spécifié dans la section **Destination** de la page **Général** ), l’option **Décharger la bande après la sauvegarde** est active. Vous pouvez cliquer sur cette option pour activer l'option **Rembobiner la bande avant de décharger** .  
  
16. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et versions ultérieures prennent en charge la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Par défaut, la compression d’une sauvegarde dépend de la valeur de l’option de configuration de serveur **Compression par défaut des sauvegardes** . Toutefois, quelle que soit la valeur par défaut actuelle au niveau du serveur, vous pouvez compresser une sauvegarde en activant **Compresser la sauvegarde**, et vous pouvez empêcher la compression en activant **Ne pas compresser la sauvegarde**.  
  
     **Pour consulter la valeur par défaut de compression de la sauvegarde actuelle**  
  
    -   [Afficher ou configurer la compression par défaut des sauvegardes (option de configuration de serveur)](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-backup-of-the-currently-active-transaction-log"></a>Pour créer une sauvegarde du journal des transactions actif  
  
1.  Exécutez l'instruction BACKUP LOG pour sauvegarder le journal des transactions actif en cours, en spécifiant :  
  
    -   le nom de la base de données à laquelle appartient le journal des transactions à sauvegarder ;  
  
    -   l'unité de sauvegarde dans laquelle sera écrite la sauvegarde du journal des transactions ;  
  
    -   la clause NO_TRUNCATE.  
  
         Cette clause permet la sauvegarde de la partie active du journal des transactions même si la base de données est inaccessible, à condition que le journal des transactions soit accessible et qu'il ne soit pas endommagé.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
  
> [!NOTE]  
>  Cet exemple utilise le mode de récupération simple, [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Pour autoriser les sauvegardes de fichier journal, avant d'effectuer une sauvegarde complète de base de données, la base de données a été configurée pour utiliser le mode de récupération complète. Pour plus d’informations, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
 Cet exemple sauvegarde le journal des transactions actif lorsqu'une base de données est endommagée et inaccessible, à la condition que le journal des transactions soit intact et accessible.  
  
```sql  
BACKUP LOG AdventureWorks2012  
   TO MyAdvWorks_FullRM_log1  
   WITH NO_TRUNCATE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)  
  
  

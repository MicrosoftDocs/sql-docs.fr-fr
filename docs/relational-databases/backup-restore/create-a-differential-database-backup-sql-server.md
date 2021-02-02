---
title: Sauvegarde différentielle
description: Cet article explique comment créer une sauvegarde différentielle de base de données dans SQL Server, à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- database backups [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
- backups [SQL Server], creating
ms.assetid: 70f49794-b217-4519-9f2a-76ed61fa9f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a59b05df4805b580aade70a9d2bf11cc3399f04
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076121"
---
# <a name="create-a-differential-database-backup-sql-server"></a>Créer une sauvegarde différentielle de base de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Créez une sauvegarde différentielle de base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Sections de cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Composants requis](#Prerequisites)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour créer une sauvegarde différentielle de base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   La création d’une sauvegarde différentielle de base de données suppose l’existence d’une sauvegarde complète de base de données préalable. Si la base de données sélectionnée n’a jamais été sauvegardée, exécutez une sauvegarde complète de base de données avant de créer des sauvegardes différentielles. Pour plus d’informations, consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   À mesure que la taille des sauvegardes différentielles augmente, leur restauration peut accroître considérablement le temps nécessaire à la restauration d'une base de données. Nous vous recommandons donc d’effectuer une nouvelle sauvegarde complète selon une périodicité fixe, afin d’établir une nouvelle base différentielle pour les données. Par exemple, vous pouvez effectuer une sauvegarde complète hebdomadaire de la base de données dans son entier (soit une sauvegarde complète de la base de données), puis des séries régulières de sauvegardes de bases de données différentielles au cours de la semaine.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="check-your-permissions-first"></a><a name="Permissions"></a> Vérifiez d’abord les autorisations dont vous bénéficiez.  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d’autorisations portant sur le fichier physique de l’unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d’écrire des données sur l’unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute doit disposer d’autorisations d’écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie **pas** les autorisations d’accès au fichier. Les problèmes relatifs aux autorisations portant sur le fichier physique de l’unité de sauvegarde n’apparaissent clairement qu’un fois la ressource physique sollicitée lorsque vous tentez une sauvegarde ou une restauration.  
  
##  <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a> SQL Server Management Studio  
  
#### <a name="create-a-differential-database-backup"></a>Créer une sauvegarde différentielle de base de données  

1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  
  
4.  Dans la zone de liste **Base de données** , vérifiez le nom de la base de données. Vous pouvez éventuellement sélectionner une autre base de données dans la liste.  
  
     Vous pouvez effectuer une sauvegarde différentielle pour n'importe quel mode de récupération (complet, simple ou utilisant les journaux de transactions).  
  
5.  Dans la zone de liste **Type de sauvegarde** , sélectionnez **Différentielle**.  
  
    > [!IMPORTANT]  
    >  Lorsque l’option **Différentielle** est sélectionnée, vérifiez que la case **Sauvegarde de copie uniquement** est décochée.  
  
6.  Pour l'option **Composant de sauvegarde**, cliquez sur **Base de données**.  
  
7.  Acceptez le nom du jeu de sauvegarde par défaut proposé dans la zone de texte **Nom** , ou attribuez-lui un autre nom.  
  
8.  Dans la zone de texte **Description** , vous avez la possibilité de saisir une description du jeu de sauvegarde.  
  
9. Indiquez quand le jeu de sauvegarde arrivera à expiration :  
  
    -   Pour que le jeu de sauvegarde expire au bout d’un nombre de jours spécifique, cliquez sur **Après** (option par défaut) et entrez le nombre de jours souhaité pour l’expiration du jeu après sa création. Cette valeur doit être comprise entre 0 et 99 999 jours ; une valeur de 0 signifie que le jeu de sauvegarde n’expirera jamais.  
  
         La valeur par défaut est définie dans l’option **Délai de rétention par défaut du support de sauvegarde (jours)** de la boîte de dialogue **Propriétés du serveur** (page **Paramètres de base de données** ). Pour y accéder, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets et sélectionnez les propriétés. Ensuite, sélectionnez la page **Paramètres de base de données** .  
  
    -   Pour que le jeu de sauvegarde expire à une date spécifique, cliquez sur **Le** et entrez la date d'expiration souhaitée.  
  
10. Choisissez le type de destination de la sauvegarde : **Disque** ou **Bande**. Pour sélectionner le chemin d'accès des lecteurs de disque ou de bande (dans la limite de 64) contenant un support de sauvegarde unique, cliquez sur **Ajouter**. Les chemins d'accès sélectionnés apparaissent dans la zone de liste **Sauvegarde sur** .  
  
     Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d'une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Sommaire**.  
  
11. Pour afficher ou sélectionner les options avancées, cliquez sur **Options** dans le volet **Sélectionner une page** .  
  
12. Sélectionnez une option **Remplacer le support** en cliquant sur un des éléments suivants :  
  
    -   **Sauvegarder sur le support de sauvegarde existant**  
  
         Pour cette option, cliquez sur **Ajouter au jeu de sauvegarde existant** ou sur **Remplacer tous les jeux de sauvegarde existants**. Vous pouvez aussi activer la case à cocher **Vérifier le nom du support de sauvegarde et la date d'expiration du jeu de sauvegarde** puis entrer si vous le souhaitez un nom dans la zone de texte **Nom du support de sauvegarde** . Si aucun nom n'est spécifié, un support de sauvegarde avec un nom vide est créé. Si vous spécifiez un nom de support de sauvegarde, le système vérifie si le nom réel du support (bande ou disque) correspond au nom que vous entrez ici.  
  
         Si vous n'entrez pas de nom et que vous activez la case à cocher pour demander la vérification par rapport au support, le nom du support sera également vide sur le support.  
  
    -   **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**  
  
         Pour cette option, entrez un nom dans la zone de texte **Nouveau nom du support de sauvegarde** et décrivez éventuellement le jeu de supports dans la zone de texte **Description du nouveau support de sauvegarde** .  
  
13. Dans la section **Fiabilité** , vous pouvez éventuellement activer les cases à cocher :  
  
    -   **Vérifier la sauvegarde en fin d'opération**;  
  
    -   **Effectuer une somme de contrôle avant d'écrire sur le support** et éventuellement **Continuer lors d'erreurs de somme de contrôle**. Pour plus d’informations sur les sommes de contrôle, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
14. Si vous effectuez la sauvegarde sur un lecteur de bande (spécifié dans la section **Destination** de la page **Général** ), l’option **Décharger la bande après la sauvegarde** est active. Vous pouvez cliquer sur cette option pour activer l'option **Rembobiner la bande avant de décharger** .  
  
    > [!NOTE]  
    >  Les options de la section **Journal des transactions** sont inactives, à moins que vous ne sauvegardiez un journal des transactions (comme spécifié dans la section **Type de sauvegarde** de la page **Général** ).  
  
15. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et versions ultérieures prennent en charge la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Par défaut, la compression d’une sauvegarde dépend de la valeur de l’option de configuration de serveur **Compression par défaut des sauvegardes** . Toutefois, quelle que soit la valeur par défaut actuelle au niveau du serveur, vous pouvez compresser une sauvegarde en activant **Compresser la sauvegarde**, et vous pouvez empêcher la compression en activant **Ne pas compresser la sauvegarde**.  
  
     **Pour consulter la valeur par défaut de compression de la sauvegarde actuelle**  
  
    -   [Afficher ou configurer la compression par défaut des sauvegardes (option de configuration de serveur)](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
    > [!NOTE]  
    >  Vous pouvez également utiliser l'Assistant Plan de maintenance pour créer des sauvegardes différentielles de base de données.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL  
  
#### <a name="create-a-differential-database-backup"></a>Créer une sauvegarde différentielle de base de données  
  
1.  Exécutez l'instruction BACKUP DATABASE pour créer une sauvegarde différentielle de base de données, en spécifiant les éléments suivants :  
  
    -   le nom de la base de données à sauvegarder ;  
  
    -   l'unité de sauvegarde où est écrite la sauvegarde complète de la base de données.  
  
    -   la clause DIFFERENTIAL afin de préciser que seules les parties de la base de données qui ont été modifiées après la création de la dernière sauvegarde complète de la base de données sont sauvegardées.  
  
     La syntaxe requise est la suivante :  
  
     BACKUP DATABASE *nom_base_de_données* TO <appareil_sauvegarde> WITH DIFFERENTIAL  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple crée une sauvegarde complète et différentielle de la base de données `MyAdvWorks` .  
  
```sql  
-- Create a full database backup first.  
BACKUP DATABASE MyAdvWorks   
   TO MyAdvWorks_1   
   WITH INIT;  
GO  
-- Time elapses.  
-- Create a differential database backup, appending the backup  
-- to the backup device containing the full database backup.  
BACKUP DATABASE MyAdvWorks  
   TO MyAdvWorks_1  
   WITH DIFFERENTIAL;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Restaurer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)   
 [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)  
  
  

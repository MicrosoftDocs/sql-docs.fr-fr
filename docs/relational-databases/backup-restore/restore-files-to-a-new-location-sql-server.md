---
title: Restaurer les fichiers à un nouvel emplacement (SQL Server) | Microsoft Docs
description: Cet article explique comment restaurer des fichiers vers un nouvel emplacement SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring files [SQL Server], how-to topics
- restoring databases [SQL Server], moving
- restoring files [SQL Server], steps
- file restores [SQL Server], how-to topics
- filegroups [SQL Server], restoring
- restoring filegroups [SQL Server]
ms.assetid: b4f4791d-646e-4632-9980-baae9cb1aade
author: cawrites
ms.author: chadam
ms.openlocfilehash: d1b7d7b90d4893e4031477780cf96c2162f509a6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764791"
---
# <a name="restore-files-to-a-new-location-sql-server"></a>Restaurer les fichiers à un nouvel emplacement (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment restaurer des fichiers à un nouvel emplacement dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour restaurer des fichiers à un nouvel emplacement, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L'administrateur système qui restaure les fichiers doit être la seule personne à utiliser la base de données au moment de la restauration.  
  
-   La commande RESTORE n'est pas autorisée dans une transaction explicite ou implicite.  
  
-   Que vous soyez en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, pour pouvoir restaurer des fichiers, vous devez d'abord sauvegarder le journal des transactions actif (appelé fin du journal). Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
-   Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-restore-files-to-a-new-location"></a>Pour restaurer les fichiers à un nouvel emplacement  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], développez cette instance, puis développez **Bases de données**.  
  
2.  Cliquez avec le bouton droit sur la base de données de votre choix, pointez sur **Tâches** et sur **Restaurer**, puis cliquez sur **Fichiers et groupes de fichiers**.  
  
3.  Sur la page **Général** , dans la zone de liste **Vers la base de données** , entrez la base de données à restaurer. Vous pouvez entrer une nouvelle base de données ou choisir une base de données existante dans la liste déroulante. La liste contient toutes les bases de données sur le serveur, à l'exception des bases de données système **master** et **tempdb**.  
  
4.  Pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer, cliquez sur l'une des options suivantes :  
  
    -   **À partir de la base de données**  
  
         Entrez un nom de base de données dans la zone de liste. Cette liste ne contient que les bases de données ayant été sauvegardées d'après l'historique de sauvegarde de **msdb** .  
  
    -   **À partir de l'unité**  
  
         Cliquez sur le bouton de recherche. Dans la boîte de dialogue **Spécifier les unités de sauvegarde** , sélectionnez l'un des types d'unités proposés dans la zone de liste **Type de support de sauvegarde** . Pour sélectionner une ou plusieurs unités pour la zone de liste **Support de sauvegarde** , cliquez sur **Ajouter**.  
  
         Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .  
  
5.  Dans la grille **Sélectionnez les jeux de sauvegarde à restaurer** , sélectionnez les sauvegardes à restaurer. Cette grille affiche les sauvegardes disponibles pour l'emplacement spécifié. Par défaut, un plan de récupération est suggéré. Pour ignorer le plan de récupération suggéré, vous pouvez modifier les sélections dans la grille. Les sauvegardes qui dépendent d'une sauvegarde désélectionnée sont automatiquement désélectionnées.  
  
    |En-tête de colonne|Valeurs|  
    |-----------------|------------|  
    |**Restauration**|Les cases à cocher sélectionnées correspondent aux jeux de sauvegarde à restaurer.|  
    |**Nom**|Nom du jeu de sauvegarde.|  
    |**Type de fichier**|Spécifie le type des données figurant dans la sauvegarde : **Données**, **Journal**, ou **Données Filestream**. Les données qui sont contenues dans les tables sont dans les fichiers **Données** . Les données du journal des transactions sont dans les fichiers **Journaux** . Les données des objets BLOB (Binary Large Object) qui sont stockées dans le système de fichiers se trouvent dans des fichiers **Données Filestream** .|  
    |**Type**|Type de sauvegarde effectuée : **Complète**, **Différentielle** ou **Journal des transactions**.|  
    |**Serveur**|Nom de l'instance Base de données-Moteur ayant effectué l'opération de sauvegarde.|  
    |**Nom logique du fichier**|Comme son nom l'indique.|  
    |**Sauvegarde de la base de données**|Nom de la base de données impliquée dans la sauvegarde.|  
    |**Date de début**|Date et heure de lancement de l'opération de sauvegarde, présentée conformément aux paramètres régionaux du client.|  
    |**Date de fin**|Date et heure de fin de l'opération de sauvegarde, exprimée d'après les paramètres régionaux du client.|  
    |**Taille**|Taille du jeu de sauvegarde, exprimée en octets.|  
    |**Nom d’utilisateur**|Nom de l'utilisateur qui a exécuté l'opération de sauvegarde.|  
  
6.  Dans le volet **Sélectionner une page** , cliquez sur la page **Options** .  
  
7.  Dans la grille **Restaurer les fichiers de base de données en tant que** , spécifiez un nouvel emplacement pour le ou les fichiers à déplacer.  
  
    |En-tête de colonne|Valeurs|  
    |-----------------|------------|  
    |**Nom du fichier d'origine**|Chemin d'accès complet d'un fichier de sauvegarde source.|  
    |**Type de fichier**|Spécifie le type des données figurant dans la sauvegarde : **Données**, **Journal**, ou **Données Filestream**. Les données qui sont contenues dans les tables sont dans les fichiers **Données** . Les données du journal des transactions sont dans les fichiers **Journaux** . Les données des objets BLOB (Binary Large Object) qui sont stockées dans le système de fichiers se trouvent dans des fichiers **Données Filestream** .|  
    |**Restaurer sous**|Chemin d'accès complet du fichier de base de données à restaurer. Pour définir un nouveau fichier de restauration, cliquez sur la zone de texte et modifiez le chemin d'accès et le nom de fichier proposés. La modification du chemin d'accès ou du nom de fichier dans la colonne **Restaurer sous** équivaut à utiliser l'option MOVE dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE.|  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-restore-files-to-a-new-location"></a>Pour restaurer les fichiers à un nouvel emplacement  
  
1.  Exécutez éventuellement l'instruction RESTORE FILELISTONLY pour déterminer le nombre de fichiers et les noms des fichiers dans la sauvegarde complète de la base de données.  
  
2.  Exécutez l'instruction RESTORE DATABASE pour restaurer la sauvegarde complète de la base de données, en spécifiant :  
  
    -   le nom de la base de données à restaurer ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde complète de la base de données ;  
  
    -   la clause MOVE de chaque fichier à restaurer au nouvel emplacement ;  
  
    -   la clause NORECOVERY.  
  
3.  Si les fichiers ont été modifiés depuis la création de la sauvegarde du fichier, exécutez l'instruction RESTORE LOG pour appliquer la sauvegarde du journal des transactions, en spécifiant :  
  
    -   le nom de la base de données à laquelle sera appliqué le journal des transactions ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde du journal des transactions ;  
  
    -   la clause NORECOVERY si vous devez appliquer une autre sauvegarde du journal des transactions après la sauvegarde en cours. Dans les autres cas, spécifiez la clause RECOVERY.  
  
         Les sauvegardes du journal des transactions, lorsqu’elles sont appliquées, doivent couvrir le temps de sauvegarde des fichiers et groupes de fichiers.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L’exemple suivant restaure deux des fichiers pour la base de données `MyNwind` ; ces fichiers, à l’origine sur le lecteur C, sont placés à de nouveaux emplacements sur le lecteur D. Deux journaux des transactions seront également appliqués pour restaurer la base de données à l’heure actuelle. L'instruction `RESTORE FILELISTONLY` permet de déterminer le nombre et les noms logique et physique des fichiers de la base de données en cours de restauration.  
  
```sql  
USE master;  
GO  
-- First determine the number and names of the files in the backup.  
RESTORE FILELISTONLY  
   FROM MyNwind_1;  
-- Restore the files for MyNwind.  
RESTORE DATABASE MyNwind  
   FROM MyNwind_1  
   WITH NORECOVERY,  
   MOVE 'MyNwind_data_1' TO 'D:\MyData\MyNwind_data_1.mdf',   
   MOVE 'MyNwind_data_2' TO 'D:\MyData\MyNwind_data_2.ndf';  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
  

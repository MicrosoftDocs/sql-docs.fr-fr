---
title: Restaurer des fichiers et des groupes de fichiers (SQL Server) | Microsoft Docs
description: Cet article explique comment restaurer des fichiers et des groupes de fichiers dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restorefilesandfilegrps.general.f1
- sql13.swb.bselectfilegrpsfiles.f1
- sql13.swb.restorefilesandfilegrps.options.f1
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], restoring files and filegroups
- restoring [SQL Server], files
ms.assetid: 72603b21-3065-4b56-8b01-11b707911b05
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4a79c574e576b157d5dd4c07c8e725e02d8d74e7
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076917"
---
# <a name="restore-files-and-filegroups-sql-server"></a>Restaurer des fichiers et des groupes de fichiers (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment restaurer des fichiers et des groupes de fichiers dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   [Sécurité](#Security)  
  
-   **Pour restaurer des fichiers et des groupes de fichiers, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L’administrateur système restaurant les fichiers et groupes de fichiers doit être la seule personne utilisant la base de données à restaurer.  
  
-   La commande RESTORE n'est pas autorisée dans une transaction explicite ou implicite.  
  
-   En mode de récupération simple, le fichier doit appartenir à un groupe de fichiers en lecture seule.  
  
-   Que vous soyez en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, pour pouvoir restaurer des fichiers, vous devez d'abord sauvegarder le journal des transactions actif (appelé fin du journal). Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
-   Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-restore-files-and-filegroups"></a>Pour restaurer des fichiers et des groupes de fichiers  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**. Selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Restaurer**.  
  
4.  Cliquez sur **Fichiers et groupes de fichiers** pour ouvrir la boîte de dialogue **Restaurer des fichiers et des groupes de fichiers** .  
  
5.  Sur la page **Général** , dans la zone de liste **Vers la base de données** , entrez la base de données à restaurer. Vous pouvez entrer une nouvelle base de données ou choisir une base de données existante dans la liste déroulante. La liste contient toutes les bases de données sur le serveur, à l'exception des bases de données système **master** et **tempdb**.  
  
6.  Pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer, cliquez sur l'une des options suivantes :  
  
    -   **À partir de la base de données**  
  
         Entrez un nom de base de données dans la zone de liste. Cette liste ne contient que les bases de données ayant été sauvegardées d'après l'historique de sauvegarde de **msdb** .  
  
    -   **À partir de l'unité**  
  
         Cliquez sur le bouton de recherche. Dans la boîte de dialogue **Spécifier les unités de sauvegarde** , sélectionnez l'un des types d'unités proposés dans la zone de liste **Type de support de sauvegarde** . Pour sélectionner une ou plusieurs unités pour la zone de liste **Support de sauvegarde** , cliquez sur **Ajouter**.  
  
         Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .  
  
7.  Dans la grille **Sélectionnez les jeux de sauvegarde à restaurer** , sélectionnez les sauvegardes à restaurer. Cette grille affiche les sauvegardes disponibles pour l'emplacement spécifié. Par défaut, un plan de récupération est suggéré. Pour ignorer le plan de récupération suggéré, vous pouvez modifier les sélections dans la grille. Les sauvegardes qui dépendent d'une sauvegarde désélectionnée sont automatiquement désélectionnées.  
  
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
  
8.  Pour afficher ou sélectionner les options avancées, cliquez sur **Options** dans le volet **Sélectionner une page**.  
  
9. Dans le panneau **Options de restauration** , choisissez les options suivantes si elles s'appliquent à votre situation.  
  
     **Restaurer en tant que groupe de fichiers**  
     Indique qu'un groupe de fichiers entier doit être restauré.  
  
     **Remplacer la base de données existante**  
     Indique que la restauration doit remplacer les bases de données existantes et leurs fichiers associés, même si une autre base de données ou un autre fichier existant porte le même nom.  
  
     L'activation de cette option revient à utiliser l'option REPLACE dans une instruction RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Demander confirmation avant chaque restauration de sauvegarde**  
     Demande une confirmation avant la restauration de chaque jeu de sauvegarde.  
  
     Cette option est particulièrement utile lorsque vous devez permuter des jeux de supports différents, lorsque, par exemple, le serveur n'a qu'un seul périphérique à bandes.  
  
     **Restreindre l'accès à la base de données restaurée**  
     Limite l’accès à la base de données restaurée aux membres de **db_owner**, **dbcreator** ou **sysadmin**.  
  
     La sélection de cette option équivaut à utiliser l'option RESTRICTED_USER dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE.  
  
10. Le cas échéant, vous pouvez restaurer la base de données vers un nouvel emplacement, en spécifiant une nouvelle destination de restauration pour chaque fichier dans la grille **Restaurer les fichiers de la base de données en tant que** .  
  
    |En-tête de colonne|Valeurs|  
    |-----------------|------------|  
    |**Nom du fichier d'origine**|Chemin d'accès complet d'un fichier de sauvegarde source.|  
    |**Type de fichier**|Spécifie le type des données figurant dans la sauvegarde : **Données**, **Journal**, ou **Données Filestream**. Les données qui sont contenues dans les tables sont dans les fichiers **Données** . Les données du journal des transactions sont dans les fichiers **Journaux** . Les données des objets BLOB (Binary Large Object) qui sont stockées dans le système de fichiers se trouvent dans des fichiers **Données Filestream** .|  
    |**Restaurer sous**|Chemin d'accès complet du fichier de base de données à restaurer. Pour définir un nouveau fichier de restauration, cliquez sur la zone de texte et modifiez le chemin d'accès et le nom de fichier proposés. La modification du chemin d'accès ou du nom de fichier dans la colonne **Restaurer sous** équivaut à utiliser l'option MOVE dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE.|  
  
11. Le volet **État de récupération** détermine l'état de la base de données à l'issue de l'opération de restauration.  

  **Laisser la base de données opérationnelle en restaurant les transactions non validées. Les journaux des transactions supplémentaires ne peuvent pas être restaurés. (RESTORE WITH RECOVERY)**  
  Récupère la base de données. Il s'agit du comportement par défaut. Ne choisissez cette option que si vous restaurez toutes les sauvegardes nécessaires maintenant. Cette option revient à spécifier WITH RECOVERY dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE.  
  
  **Laisser la base de données non opérationnelle, et ne pas restaurer les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. (RESTORE WITH NORECOVERY)**  
  Laisse la base de données en état de restauration. Pour restaurer la base de données, vous devez exécuter une autre restauration en utilisant l'option RESTORE WITH RECOVERY précédente (voir ci-dessus). Cette option revient à spécifier WITH NORECOVERY dans une instruction RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
  Si vous sélectionnez cette option, l'option **Conserver les paramètres de réplication** n'est pas disponible.  
  
  **Laisser la base de données en lecture seule. Annulez les transactions non validées, mais enregistrez l'opération d'annulation dans un fichier pour pouvoir annuler les effets de la restauration. (RESTORE WITH STANDBY)**  
  Laisse la base de données dans un état de secours. Cette option revient à spécifier WITH STANDBY dans une instruction RESTORE [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
  Cette option nécessite de définir un fichier d'annulation.  
  
  **Fichier journal des annulations de la restauration**  
  Définissez un fichier d’annulation dans la zone de texte **Fichier journal des annulations de la restauration** . Cette option est nécessaire si vous laissez la base de données en lecture seule (RESTORE WITH STANDBY).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-restore-files-and-filegroups"></a>Pour restaurer des fichiers et des groupes de fichiers  
  
1.  Exécutez l’instruction RESTORE DATABASE pour restaurer la sauvegarde du fichier ou du groupe de fichiers, en spécifiant :  
  
    -   le nom de la base de données à restaurer ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde complète de la base de données ;  
  
    -   La clause FILE pour chaque fichier à restaurer.  
  
    -   La clause FILEGROUP pour chaque groupe de fichiers à restaurer.  
  
    -   la clause NORECOVERY. Si les fichiers n'ont pas été modifiés depuis la création de la sauvegarde, spécifiez la clause RECOVERY.  
  
2.  Si les fichiers ont été modifiés depuis la création de la sauvegarde du fichier, exécutez l'instruction RESTORE LOG pour appliquer la sauvegarde du journal des transactions, en spécifiant :  
  
    -   le nom de la base de données à laquelle sera appliqué le journal des transactions ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde du journal des transactions ;  
  
    -   la clause NORECOVERY si vous devez appliquer une autre sauvegarde du journal des transactions après la sauvegarde en cours. Dans les autres cas, spécifiez la clause RECOVERY.  
  
         Les sauvegardes du journal des transactions, lorsqu'elles sont appliquées, doivent couvrir la période de sauvegarde des fichiers et groupes de fichiers jusqu'à la fin du journal (sauf si TOUS les fichiers de base de données sont restaurés).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple restaure les fichiers et les groupes de fichiers pour la base de données `MyDatabase` . Pour restaurer la base de données à l'heure actuelle, deux journaux de transactions sont appliqués.  
  
```sql  
USE master;  
GO  
-- Restore the files and filesgroups for MyDatabase.  
RESTORE DATABASE MyDatabase  
   FILE = 'MyDatabase_data_1',  
   FILEGROUP = 'new_customers',  
   FILE = 'MyDatabase_data_2',  
   FILEGROUP = 'first_qtr_sales'  
   FROM MyDatabase_1  
   WITH NORECOVERY;  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  

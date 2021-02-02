---
title: Gérer la table suspect_pages (SQL Server) | Microsoft Docs
description: Découvrez comment gérer la table suspect_pages dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL. Les pages qui produisent certaines erreurs sont suspectes.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- 824 (Database Engine error)
- restoring pages [SQL Server]
- pages [SQL Server], suspect
- pages [SQL Server], restoring
- suspect_pages system table
- suspect pages [SQL Server]
- restoring [SQL Server], pages
ms.assetid: f394d4bc-1518-4e61-97fc-bf184d972e2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 88859e39a29c227d2cbb56328fbc96a4de7528d1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076907"
---
# <a name="manage-the-suspect_pages-table-sql-server"></a>Gérer la table suspect_pages (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment gérer la table **suspect_pages** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La table **suspect_pages** est conçue pour gérer les informations sur les pages suspectes et aide à déterminer si une restauration est nécessaire ou non. La table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) réside dans la [base de données msdb](../../relational-databases/databases/msdb-database.md).  
  
 Une page est considérée « suspecte » quand le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] rencontre l'une des erreurs suivantes alors qu'il essaie de lire une page de données :  
  
-   Une erreur 823 provoquée par un contrôle de redondance cyclique (CRC) généré par le système d’exploitation, telle qu’une erreur disque (certaines défaillances matérielles)  
  
-   Une erreur 824, telle qu’une page endommagée (toute erreur logique)  
  
 L’ID de page de chaque page suspecte est enregistré dans la table **suspect_pages** . Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre toutes les pages suspectes détectées lors du traitement standard, notamment dans les cas suivants :  
  
-   une requête doit lire une page ;  
  
-   au cours d'une opération DBCC CHECKDB ;  
  
-   au cours d'une opération de sauvegarde.  
  
 La table **suspect_pages** est aussi mise à jour selon les besoins au cours d’une opération de restauration, une opération de réparation DBCC ou une opération de suppression de base de données.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour gérer la table suspect_pages, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   **Erreurs enregistrées dans la table suspect_pages**  
  
     La table **suspect_pages** contient une ligne pour chaque page dans laquelle une erreur 824 (avec une limite de 1 000 lignes) s’est produite. Le tableau suivant présente les erreurs consignées dans la colonne **event_type** de la table **suspect_pages** .  
  
    |Description de l'erreur|Valeur **event_type**|  
    |-----------------------|---------------------------|  
    |Erreur 823 provoquée par une erreur CRC du système d'exploitation ou erreur 824 autre qu'une somme de contrôle incorrecte ou une page endommagée (par exemple, ID de page incorrect)|1|  
    |Somme de contrôle incorrecte|2|  
    |Page endommagée|3|  
    |Page restaurée (la page a été restaurée après avoir été marquée comme incorrecte)|4|  
    |Page réparée (par DBCC)|5|  
    |Page désallouée par DBCC|7|  
  
     La table **suspect_pages** enregistre aussi les erreurs temporaires.  Elles peuvent être le résultat notamment d'une erreur d'E/S (un câble déconnecté, par exemple) ou de l'échec temporaire d'un test de somme de contrôle répété sur une page.  
  
-   **Mise à jour de la table suspect_pages à l'aide du moteur de base de données**  
  
     Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] entreprend les actions suivantes dans la table **suspect_pages** :  
  
    -   Si la table n'est pas remplie, elle est mise à jour à chaque erreur 824 afin de signaler qu'une erreur s'est produite, et le compteur d'erreurs est incrémenté. Si une page comporte une erreur après qu’elle a été corrigée par réparation, restauration ou désallocation, son nombre **number_of_errors** est incrémenté et sa colonne **last_update** est mise à jour.  
  
    -   Après la correction d’une page répertoriée par une opération de restauration ou de réparation, cette opération met à jour la ligne **suspect_pages** pour indiquer que la page est réparée (**event_type** = 5) ou restaurée (**event_type** = 4).  
  
    -   Si vous procédez à une vérification DBCC, toutes les pages exemptes d’erreurs sont marquées comme étant réparées (**event_type** = 5) ou désallouées (**event_type** = 7).  
  
-   **Mises à jour automatiques dans la table suspect_pages**  
  
     Un serveur partenaire de mise en miroir de bases de données ou un réplica de disponibilité Always On met à jour la table **suspect_pages** après l’échec d’une tentative de lecture d’une page d’un fichier de données pour l’une des raisons suivantes.  
  
    -   Une erreur 823 provoquée par une erreur CRC du système d'exploitation  
  
    -   Une erreur 824 (altération logique telle qu'une page endommagée)  
  
     Les actions suivantes mettent également à jour automatiquement des lignes de la table **suspect_pages** .  
  
    -   DBCC CHECKDB REPAIR_ALLOW_DATA_LOSS met à jour la table **suspect_pages** pour indiquer chaque page qu’il a désallouée ou réparée.  
  
    -   Une restauration (RESTORE) complète de fichier ou de page marque les entrées de pages comme restaurées.  
  
     Les actions suivantes suppriment automatiquement des lignes de la table **suspect_pages** .  
  
    -   ALTER DATABASE REMOVE FILE  
  
    -   DROP DATABASE  
  
-   **Rôle de maintenance de l'administrateur de base de données**  
  
     Les administrateurs de base de données sont responsables de la gestion de la table ; leur rôle consiste principalement à supprimer les lignes anciennes. La table **suspect_pages** est limitée en taille ; une fois la table remplie, les nouvelles erreurs ne sont plus consignées. Pour empêcher la saturation de la table, l'administrateur de la base de données ou l'administrateur système doit manuellement y effacer les anciennes entrées en supprimant les lignes. Ainsi, nous vous recommandons de supprimer ou d’archiver régulièrement les lignes comportant un **event_type** restauré ou réparé, ou les lignes comportant une valeur ancienne pour **last_update** .  
  
     Pour surveiller l’activité sur la table suspect_pages, vous pouvez utiliser la [classe d’événements Database Suspect Data Page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md). Des lignes sont parfois ajoutées à la table **suspect_pages** à cause d’erreurs temporaires. Cependant, si de nombreuses lignes sont ajoutées à la table, le sous-système d'E/S est probablement défectueux. Si vous remarquez une augmentation soudaine du nombre de lignes ajoutées à la table, nous vous recommandons de rechercher la source des problèmes éventuels dans le sous-système d'E/S.  
  
     Un administrateur de base de données peut également insérer ou mettre à jour des enregistrements. Par exemple, la mise à jour d'une ligne peut être utile si un administrateur de base de données sait qu'une page suspecte particulière est intacte en réalité, mais souhaite malgré tout conserver l'enregistrement correspondant pendant un certain temps.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Toute personne ayant accès à **msdb** peut lire les données de la table **suspect_pages** . Toute personne ayant l'autorisation UPDATE sur la table suspect_pages peut mettre à jour ses enregistrements. Les membres du rôle de base de données fixe **db_owner** sur **msdb** ou du rôle serveur fixe **sysadmin** peuvent insérer, mettre à jour et supprimer des enregistrements.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-manage-the-suspect_pages-table"></a>Pour gérer la table suspect_pages  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], développez cette instance, puis développez **Bases de données**.  
  
2.  Développez **Bases de données système**, puis **msdb**, **Tables** et **Tables système**.  
  
3.  Développez **dbo.suspect_pages** et cliquez avec le bouton droit sur **Modifier les 200 lignes du haut**.  
  
4.  Dans la fenêtre de requête, modifiez, mettez à jour ou supprimez les lignes de votre choix.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-manage-the-suspect_pages-table"></a>Pour gérer la table suspect_pages  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez les exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple supprime certaines des lignes de la table `suspect_pages` .  
  
```  
-- Delete restored, repaired, or deallocated pages.  
DELETE FROM msdb..suspect_pages  
   WHERE (event_type = 4 OR event_type = 5 OR event_type = 7);  
GO  
  
```  
  
 Cet exemple retourne les pages incorrectes dans la table `suspect_pages` .  
  
```  
-- Select nonspecific 824, bad checksum, and torn page errors.  
SELECT * FROM msdb..suspect_pages  
   WHERE (event_type = 1 OR event_type = 2 OR event_type = 3);  
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
    
   
  
  




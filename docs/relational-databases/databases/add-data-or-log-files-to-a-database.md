---
title: Ajouter des fichiers de données ou des fichiers journaux à une base de données | Microsoft Docs
description: Découvrez comment ajouter des fichiers de données ou journaux dans SQL Server 2019 à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- adding data files
- adding files
- adding log files
- file additions [SQL Server], steps
- files [SQL Server], adding
- data additions [SQL Server]
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: stevestein
ms.author: sstein
ms.openlocfilehash: e0e411e82a6492fda48cc8d3541dbd272075f994
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813136"
---
# <a name="add-data-or-log-files-to-a-database"></a>Ajouter des fichiers de données ou journaux à une base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment ajouter des fichiers de données ou des fichiers journaux à une base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour ajouter des fichiers de données ou des fichiers journaux à une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez pas ajouter ou supprimer de fichier tant qu'une instruction BACKUP est en cours d'exécution.  
  
-   Un maximum de 32 767 fichiers et 32 767 groupes de fichiers peut être spécifié pour chaque base de données.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Pour ajouter des fichiers de données ou journaux à une base de données  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données d’où viennent les fichiers à ajouter, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de la base de données** , sélectionnez la page **Fichiers** .  
  
4.  Pour ajouter un fichier de données ou un fichier de journal des transactions, cliquez sur **Ajouter**.  
  
5.  Dans la grille **Fichiers de la base de données** , tapez le nom logique du fichier. Ce nom doit être unique dans la base de données.  
  
6.  Sélectionnez le type de fichier : données ou journal.  
  
7.  Pour un fichier de données, sélectionnez le groupe de fichiers dans lequel le fichier doit être inclus, ou sélectionnez **\<new filegroup>** pour créer un nouveau groupe de fichiers. Les journaux des transactions ne peuvent pas être placés dans des groupes de fichiers.  
  
8.  Spécifiez la taille initiale du fichier. Attribuez aux fichiers de données un maximum d'espace en tenant compte du volume maximal de données qu'est censée contenir la base de données.  
  
9. Pour spécifier la manière dont la taille du fichier doit augmenter, cliquez sur ( **...** ) dans la colonne **Croissance automatique**. Choisissez parmi les options suivantes :  
  
    1.  Pour autoriser la croissance du fichier sélectionné au fur et à mesure que l'espace requis pour les données augmente, activez la case à cocher **Activer la croissance automatique** , puis sélectionnez l'une des options suivantes :  
  
    2.  Pour spécifier que le fichier doit augmenter de taille par incréments fixes, cliquez sur **En mégaoctets** et spécifiez une valeur.  
  
    3.  Pour spécifier que le fichier doit grandir d'un pourcentage de sa taille actuelle, cliquez sur **En pourcentage** et spécifiez une valeur.  
  
10. Pour spécifier la taille limite du fichier, choisissez l'une des options suivantes :  
  
    1.  Pour spécifier la taille maximale que le fichier peut atteindre, cliquez sur **Restreindre la croissance des fichiers (Mo)** et spécifiez une valeur.  
  
    2.  Pour permettre au fichier d'augmenter de taille en fonction des besoins, cliquez sur **Croissance des fichiers illimitée**.  
  
    3.  Pour empêcher toute croissance du fichier, désactivez la case à cocher **Activer la croissance automatique** . La taille du fichier ne dépassera jamais la valeur spécifiée dans la colonne **Taille initiale (Mo)** .  
  
    > [!NOTE]  
    >  La taille maximale de la base de données est déterminée par la quantité d'espace disponible sur le disque et par les limites de licences fixées par la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous utilisez.  
  
11. Spécifiez le chemin d'accès de l'emplacement du fichier. Le chemin d'accès spécifié doit exister avant l'ajout du fichier.  
  
    > [!NOTE]  
    >  Par défaut, les fichiers de données et les journaux des transactions sont placés au même endroit sur le même lecteur pour des raisons de compatibilité avec les systèmes à disque unique, ce qui n'est parfois pas idéal pour les environnements de production. Pour plus d'informations, consultez [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).  
  
12. Cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Pour ajouter des fichiers de données ou journaux à une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple ajoute un groupe de deux fichiers à une base de données. L'exemple crée le groupe de fichiers `Test1FG1` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et ajoute deux fichiers de 5 Mo au groupe de fichiers.  
  
 [!code-sql[DatabaseDDL#AlterDatabase2](../../relational-databases/databases/codesnippet/tsql/add-data-or-log-files-to_1.sql)]  
  
 Pour plus d’exemples, consultez [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [Supprimer des fichiers de données ou des fichiers journaux d'une base de données](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [Augmenter la taille d’une base de données](../../relational-databases/databases/increase-the-size-of-a-database.md)  
  
  

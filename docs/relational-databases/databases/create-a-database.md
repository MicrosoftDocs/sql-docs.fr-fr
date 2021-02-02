---
title: Créer une base de données | Microsoft Docs
description: Découvrez comment détacher une base de données dans SQL Server 2019 à l’aide de SQL Server Management Studio ou Transact-SQL. Consultez les recommandations pour la procédure.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], creating
- database creation [SQL Server], SQL Server Management Studio
- creating databases
ms.assetid: 4c4beea2-6cbc-4352-9db6-49ea8130bb64
author: stevestein
ms.author: sstein
ms.openlocfilehash: 304dbec65d26c1d7daa63ca127a545a587f76bd1
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813600"
---
# <a name="create-a-database"></a>Créer une base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer une base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  

> [!NOTE]
> Pour créer une base de données dans Azure SQL Database à l’aide de T-SQL, consultez [Créer une base de données dans Azure SQL Database](../../t-sql/statements/create-database-transact-sql.md).
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Composants requis](#Prerequisites)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour créer une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous pouvez spécifier un maximum de 32 767 bases de données sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   L'instruction CREATE DATABASE doit être exécutée en mode de validation automatique (mode de gestion des transactions par défaut) et n'est pas autorisée dans une transaction explicite ou implicite.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   La base de données [master](../../relational-databases/databases/master-database.md) doit être sauvegardée chaque fois qu'une base de données utilisateur est créée, modifiée ou supprimée.  
  
-   Lorsque vous créez une base de données, attribuez aux fichiers une taille aussi grande que possible, en tenant compte du volume maximal de données qu'est censée contenir la base de données.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation CREATE DATABASE sur la base de données master, ou l'autorisation ALTER ANY DATABASE ou VIEW ANY DEFINITION.  
  
 Pour garder le contrôle de l'utilisation du disque sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'autorisation de création de bases de données est généralement limitée à quelques comptes de connexion.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-database"></a>Pour créer une base de données  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Cliquez avec le bouton droit sur **Bases de données**, puis cliquez sur **Nouvelle base de données**.  
  
3.  Dans **Nouvelle base de données**, entrez le nom de la base de données.  
  
4.  Pour créer la base de données en acceptant toutes les valeurs par défaut, cliquez sur **OK**, sinon effectuez les étapes facultatives ci-après.  
  
5.  Pour changer le nom du propriétaire, cliquez sur ( **...** ) pour sélectionner un autre propriétaire.  
  
    > [!NOTE]  
    >  L’option **Utiliser l’indexation de texte intégral** est toujours activée et estompée, car toutes les bases de données utilisateur sont activées pour la recherche en texte intégral à compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
6.  Pour modifier les valeurs par défaut des données primaires et des fichiers journaux de transactions, dans la grille **Fichiers de la base de données** , cliquez sur la cellule appropriée, puis entrez la nouvelle valeur. Pour plus d’informations, consultez [Ajouter des fichiers de données ou journaux à une base de données](../../relational-databases/databases/add-data-or-log-files-to-a-database.md).  
  
7.  Pour modifier le classement de la base de données, sélectionnez la page **Options** , puis sélectionnez un classement dans la liste.  
  
8.  Pour modifier le mode de récupération, sélectionnez la page **Options** , puis sélectionnez un mode de récupération dans la liste.  
  
9. Pour modifier les options de la base de données, sélectionnez la page **Options** , puis apportez les modifications de votre choix. Pour obtenir une description de chaque option, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
10. Pour ajouter un nouveau groupe de fichiers, cliquez sur la page **Groupes de fichiers** . Cliquez sur **Ajouter** , puis entrez les valeurs du groupe de fichiers.  
  
11. Pour ajouter une propriété étendue à la base de données, sélectionnez la page **Propriétés étendues** .  
  
    1.  Dans la colonne **Nom** , entrez le nom de la propriété étendue.  
  
    2.  Dans la colonne **Valeur** , entrez le texte de la propriété étendue. Par exemple, vous pouvez entrer une description de la base de données.  
  
12. Pour créer la base de données, cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-database"></a>Pour créer une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée la base de données `Sales`. Le mot clé PRIMARY n’étant pas utilisé, le premier fichier (`Sales_dat`) devient le fichier principal. Le paramètre SIZE n'étant spécifié ni en Mo ni en Ko pour le fichier `Sales_dat` , la valeur par défaut est Mo et il est alloué en mégaoctets. La base de données `Sales_log` est alloué en mégaoctets car le suffixe `MB` est défini explicitement dans le paramètre `SIZE` .  
  
```sql  
USE master ;  
GO  
CREATE DATABASE Sales  
ON   
( NAME = Sales_dat,  
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',  
    SIZE = 10,  
    MAXSIZE = 50,  
    FILEGROWTH = 5 )  
LOG ON  
( NAME = Sales_log,  
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',  
    SIZE = 5MB,  
    MAXSIZE = 25MB,  
    FILEGROWTH = 5MB ) ;  
GO  
```  
  
 Pour obtenir plus d’exemples, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Ajouter des fichiers de données ou journaux à une base de données](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
  

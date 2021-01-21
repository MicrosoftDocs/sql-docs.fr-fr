---
title: Conseils et astuces sur l’utilisation de SSMS
description: Déocuvrez comment commenter & supprimer des commentaires du code, mettre en retrait du texte, filtrer des objets, accéder à des journaux d’erreurs et rechercher des noms d’instances SQL avec SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
helpviewer_keywords:
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- Find SQL Server Instance
- find instance name
- find sql server instance name
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: 9d480496cad6dafddc428c6049601521ebba88b8
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596455"
---
# <a name="tips-and-tricks-for-using-sql-server-management-studio-ssms"></a>Conseils et astuces pour utiliser SQL Server Management Studio (SSMS)

Cet article vous propose des conseils et astuces pour utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS). Cet article vous montre comment : 

> [!div class="checklist"]
> * Ajouter/supprimer des marques de commentaire dans le texte Transact-SQL (T-SQL)
> * Mettre en retrait du texte
> * Filtrer des objets dans l’Explorateur d’objets
> * Accéder à votre journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
> * Rechercher du nom de votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

## <a name="prerequisites"></a>Prérequis

Pour tester les étapes fournies dans cet article, vous avez besoin de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], d’un accès à un serveur SQL et d’une base de données AdventureWorks. 

* Installez [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Installez [[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Édition Développeur](https://www.microsoft.com/sql-server/sql-server-downloads).
* Téléchargez un [exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour savoir comment restaurer une base de données dans SSMS, consultez [Restauration d’une base de données](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="commentuncomment-your-t-sql-code"></a>Ajouter/supprimer des marques de commentaire dans votre code T-SQL

Vous pouvez ajouter et supprimer des marques de commentaire dans des parties de votre texte à l’aide du bouton **Commenter** de la barre d’outils. Le texte qui est commenté n’est pas exécuté.

1. Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

2. Connectez-vous à votre serveur SQL.

3. Ouvrez une fenêtre Nouvelle requête.

4. Collez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant dans votre fenêtre de texte.

    ```sql
    USE master
        GO

        -- Drop the database if it already exists
        IF  EXISTS (
            SELECT name 
                FROM sys.databases 
                WHERE name = N'TutorialDB'
                )

        DROP DATABASE TutorialDB
        GO

        CREATE DATABASE TutorialDB
        GO

        ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
        GO
   ```

5. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Commenter** dans la barre d’outils : 

    ![Bouton Commenter](media/ssms-tricks/comment.png)
6. Sélectionnez **Exécuter** pour exécuter la partie de texte sans marque de commentaire. 

7. Mettez en surbrillance tous les éléments à l’exception de la commande **ALTER DATABASE**, puis sélectionnez le bouton **Commenter** :

    ![Tout commenter](media/ssms-tricks/commenteverything.png)

    > [!NOTE]
    > Le raccourci clavier permettant d’ajouter des marques commentaire au texte est **Ctrl+K, Ctrl+C**.

8. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Supprimer les marques de commentaire** pour supprimer les marques de commentaire :

    ![Supprimer les marques de commentaire dans le texte](media/ssms-tricks/uncomment.png)

    > [!NOTE]
    > Le raccourci clavier permettant de supprimer des marques de commentaire du texte est **Ctrl+K, Ctrl+U**.

9. Sélectionnez **Exécuter** pour exécuter la partie de texte sans marque de commentaire.

## <a name="indent-your-text"></a>Mettre en retrait du texte

Vous pouvez utiliser les boutons de mise en retrait de la barre d’outils pour augmenter ou réduire le retrait du texte.

1. Ouvrez une fenêtre Nouvelle requête.

2. Collez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant dans votre fenêtre de texte :

    ```sql
    USE master
      GO

      --Drop the database if it already exists
      IF  EXISTS (
            SELECT name
                FROM sys.databases
                WHERE name = N'TutorialDB'
              )

      DROP DATABASE TutorialDB
      GO

      CREATE DATABASE TutorialDB
      GO

      ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
      GO
    ```

3. Mettez en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Augmenter le retrait** dans la barre d’outils pour déplacer ce texte vers l’avant :

    ![Augmenter le retrait](media/ssms-tricks/increaseindent.png)

4. Mettez à nouveau en surbrillance la partie **ALTER DATABASE** du texte et sélectionnez le bouton **Réduire le retrait** pour déplacer ce texte vers l’arrière.

    ![Réduire le retrait](media/ssms-tricks/decreaseindent.png)

## <a name="filter-objects-in-object-explorer"></a>Filtrer des objets dans l’Explorateur d’objets

Dans les bases de données qui ont de nombreux objets, vous pouvez utiliser le filtrage pour rechercher des tables, des vues et plus encore. Cette section décrit comment filtrer les tables, mais vous pouvez utiliser les étapes suivantes pour n’importe quel autre nœud dans l’Explorateur d’objets :

1. Connectez-vous à votre serveur SQL.

2. Développez **Bases de données** > **AdventureWorks** > **Tables**. Toutes les tables de la base de données s’affichent.

3. Cliquez avec le bouton droit sur **Tables**, puis sélectionnez **Filtrer** > **Paramètres de filtre**:

    ![Paramètres du filtre](media/ssms-tricks/filtersettings.png)

4. Dans la fenêtre **Paramètres de filtre**, vous pouvez modifier les paramètres de filtre suivants :
    * Filtrer par nom : 

      ![Filtrer par nom](media/ssms-tricks/filterbyname.png)

    * Filtrer par schéma : 

      ![Filtrer par schéma](media/ssms-tricks/filterbyschema.png)

5. Pour effacer le filtre, cliquez avec le bouton droit sur **Tables**, puis sélectionnez **Supprimer le filtre**.

    ![Supprimer le filtre](media/ssms-tricks/removefilter.png)

## <a name="access-your-sql-server-error-log"></a>Accéder à votre journal des erreurs SQL Server

Le journal des erreurs est un fichier qui contient les détails de ce qui se produit dans votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez parcourir et interroger le journal des erreurs dans SSMS. Le journal des erreurs est un fichier .log qui se trouve sur votre disque.

### <a name="open-the-error-log-in-ssms"></a>Ouvrir le journal des erreurs dans SSMS

1. Connectez-vous à votre serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

2. Développez **Gestion** > **Journaux SQL Server**. 

3. Cliquez avec le bouton droit sur le journal des erreurs **Actuel**, puis sélectionnez **Voir le journal SQL Server** :

    ![Voir le journal des erreurs dans SSMS](media/ssms-tricks/viewerrorloginssms.png)

### <a name="query-the-error-log-in-ssms"></a>Interroger le journal des erreurs dans SSMS

1. Connectez-vous à votre serveur SQL.

2. Ouvrez une fenêtre Nouvelle requête.

3. Collez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant dans votre fenêtre de requête :

     ```sql
       sp_readerrorlog 0,1,'Server process ID'
      ```

4. Remplacez le texte entre guillemets simples par le texte à rechercher.

5. Exécutez la requête et passez en revue les résultats :

    ![Interroger le journal des erreurs](media/ssms-tricks/queryerrorlog.png)

### <a name="find-the-error-log-location-if-youre-connected-to-sql-server"></a>Rechercher l’emplacement du journal des erreurs si vous êtes connecté à SQL Server

1. Connectez-vous à votre serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

2. Ouvrez une fenêtre Nouvelle requête.

3. Collez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant dans votre fenêtre de requête, puis sélectionnez **Exécuter** :

     ```sql
        SELECT SERVERPROPERTY('ErrorLogFileName') AS 'Error log file location'  
      ```

4. Les résultats indiquent l’emplacement du journal des erreurs dans le système de fichiers : 

    ![Rechercher le journal des erreurs par requête](media/ssms-tricks/finderrorlogquery.png)

### <a name="find-the-error-log-location-if-you-cant-connect-to-sql-server"></a>Rechercher l’emplacement du journal des erreurs si vous ne pouvez pas vous connecter à SQL Server

Le chemin de votre journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut varier en fonction de vos paramètres de configuration. Vous trouverez le chemin d’accès à l’emplacement du journal des erreurs dans les paramètres de démarrage au sein du Gestionnaire de configuration SQL Server. Suivez les étapes ci-dessous pour trouver le paramètre de démarrage pertinent identifiant l’emplacement de votre journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Votre chemin d'accès peut varier en fonction du chemin d’accès indiqué ci-dessous*.

1. Ouvrez le Gestionnaire de configuration SQL Server.

2. Développez **Services**.

3. Cliquez avec le bouton droit sur votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis sélectionnez **Propriétés** :

    ![Propriétés de serveur du Gestionnaire de configuration](media/ssms-tricks/serverproperties.PNG)

4. Sélectionnez l’onglet **Paramètres de démarrage**.

5. Dans la zone **Paramètres existants**, le chemin après le « -e » correspond à l’emplacement du journal des erreurs : 

    ![Journal des erreurs](media/ssms-tricks/errorlog.png)

    Plusieurs fichiers journaux d’erreurs se trouvent à cet emplacement. Le nom de fichier qui se termine par *.log est le fichier du journal des erreurs actuel. Les noms de fichiers qui se terminent par des chiffres sont les fichiers journaux précédents. Un journal est créé chaque fois que le serveur SQL redémarre.

6. Ouvrez le fichier errorlog.log dans le Bloc-notes.

## <a name="find-sql-server-instance-name"></a>Trouver le nom de l’instance SQL Server

Vous avez plusieurs options pour rechercher le nom de votre serveur SQL avant et après vous être connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

### <a name="before-you-connect-to-sql-server"></a>Avant de vous connecter à SQL Server

1. Suivez les étapes pour localiser le [journal des erreurs SQL Server sur le disque](#find-the-error-log-location-if-you-cant-connect-to-sql-server). Votre chemin peut être différent du chemin indiqué dans l’image ci-dessous.

2. Ouvrez le fichier errorlog.log dans le Bloc-notes.

3. Recherchez le texte *Server name is*.

    Ce qui figure entre guillemets simples est le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous allez vous connecter :

    ![Rechercher le nom de serveur dans le journal des erreurs](media/ssms-tricks/servernameinlog.png)

    Le format du nom est HOSTNAME\INSTANCENAME. Si vous voyez uniquement le nom d’hôte, vous avez installé l’instance par défaut et son nom est MSSQLSERVER. Quand vous vous connectez à une instance par défaut, il vous suffit d’entrer le nom d’hôte pour vous connecter à votre serveur SQL.  

### <a name="when-youre-connected-to-sql-server"></a>Une fois que vous êtes connecté à SQL Server

Une fois que vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le nom du serveur est disponible à trois emplacements : 

1. Le nom du serveur est indiqué dans l’Explorateur d’objets :

    ![Nom de l’instance SQL Server dans l’Explorateur d’objets](media/ssms-tricks/nameinobjectexplorer.png)
2. Le nom du serveur est indiqué dans la fenêtre de requête :

    ![Nom de l’instance SQL Server dans la fenêtre de requête](media/ssms-tricks/nameinquerywindow.png)
3. Le nom du serveur est indiqué dans **Propriétés**.
    * Dans le menu **Affichage**, sélectionnez **Fenêtre Propriétés** :

      ![Nom de l’instance SQL Server dans la fenêtre Propriétés](media/ssms-tricks/nameinproperties.png)

### <a name="if-youre-connected-to-an-alias-or-availability-group-listener"></a>Si vous êtes connecté à un alias ou un écouteur de groupe de disponibilité

Quand vous êtes connecté à un alias ou un écouteur de groupe de disponibilité, ces informations sont indiquées dans l’Explorateur d’objets et la fenêtre Propriétés. Dans ce cas, le nom du serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ne pas être visible de manière évidente et doit être interrogé :

1. Connectez-vous à votre serveur SQL.

2. Ouvrez une fenêtre Nouvelle requête.

3. Collez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant dans la fenêtre :

      ```sql
       select @@Servername
     ```

4. Consultez les résultats de la requête pour identifier le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté : 

    ![Interroger le nom du serveur SQL](media/ssms-tricks/queryservername.png)

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles *Tutoriel* et *Procédure* vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS.  Ces articles vous apprennent à gérer les composants de SSMS et à trouver les fonctionnalités utilisées régulièrement.

* [Se connecter à une instance et l’interroger](../quickstarts/ssms-connect-query-sql-server.md)
* [Création de scripts](scripting-ssms.md)
* [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
* [Configuration de SSMS](ssms-configuration.md)
---
title: Se connecter à et interroger Azure SQL Database
description: Suivez un guide de démarrage rapide dans lequel vous utiliserez Azure Data Studio pour vous connecter à un serveur Azure SQL Database, puis vous créerez et interrogerez une base de données.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu; maghan; sstein
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.custom: seodec18; sqlfreshmay19; seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: c372b09d9dd3c96fb34f75e16a27a5a6aa3238b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048249"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-azure-sql-database"></a>Démarrage rapide : Utiliser Azure Data Studio pour se connecter et interroger une base de données Azure SQL

Dans ce démarrage rapide, vous allez utiliser Azure Data Studio pour vous connecter à un serveur Azure SQL Database. Vous exécuterez ensuite les instructions Transact-SQL (T-SQL) pour créer et interroger la base de données TutorialDB, qui est utilisée dans d’autres didacticiels Azure Data Studio.

## <a name="prerequisites"></a>Prérequis

Pour suivre ce guide de démarrage rapide, vous aurez besoin d’Azure Data Studio et d’un serveur Azure SQL Database.

- [Installer Azure Data Studio](./download-azure-data-studio.md)

Si vous n’avez pas de serveur Azure SQL, effectuez un des démarrages rapides Azure SQL Database suivants. N’oubliez pas le nom complet du serveur et les informations d’identification de connexion pour les étapes ultérieures :

- [Créer une base de données - Portail](/azure/sql-database/sql-database-get-started-portal)
- [Créer une base de données - CLI](/azure/sql-database/sql-database-get-started-cli)
- [Créer une base de données - PowerShell](/azure/sql-database/sql-database-get-started-powershell)


## <a name="connect-to-your-azure-sql-database-server"></a>Connexion à votre serveur Azure SQL Database

Utilisez Azure Data Studio pour établir une connexion à votre serveur Azure SQL Database.

1. La première fois que vous exécutez Azure Data Studio, la **page d’accueil** doit s’ouvrir. Si vous ne voyez pas la **page d’accueil**, sélectionnez **Aide** > **Bienvenue**. Sélectionnez **Nouvelle connexion** pour ouvrir le volet **Connexion** :
   
   ![Capture d’écran montrant la boîte de dialogue Bienvenue d’Azure Delta Studio, avec l’option Connexion suivante mise en évidence.](media/quickstart-sql-database/new-connection-icon.png)

2. Cet article utilise la connexion SQL, mais prend également en charge l’authentification Windows. Renseignez les champs comme suit en utilisant le nom du serveur, le nom d’utilisateur et le mot de passe de votre serveur Azure SQL :

   | Paramètre       | Valeur suggérée | Description |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | Nom complet du serveur | Par exemple : **servername.database.windows.net**. |
   | **Authentification** | Connexion SQL| Ce didacticiel utilise l’authentification SQL. |
   | **Nom d'utilisateur** | Le nom d’utilisateur du compte administrateur du serveur | Le nom d’utilisateur du compte utilisé pour créer le serveur. |
   | **Mot de passe (connexion SQL)** | Le mot de passe du compte administrateur du serveur | Le mot de passe du compte utilisé pour créer le serveur. |
   | **Enregistrer le mot de passe ?** | Oui ou Non | Sélectionnez **Oui** si vous ne souhaitez pas entrer le mot de passe à chaque fois. |
   | **Nom de la base de données** | *laisser vide* | Vous vous connectez uniquement au serveur ici. |
   | **Groupe de serveurs** | Sélectionnez <Default> | Vous pouvez définir ce champ sur un groupe de serveurs spécifique que vous avez créé. | 

   ![Capture d’écran de la page de connexion d’Azure Data Studio.](media/quickstart-sql-database/new-connection-screen.png)  

3. Sélectionnez **Connecter**.

4. Si votre serveur ne dispose pas d’une règle de pare-feu autorisant Azure Data Studio à se connecter, le formulaire **Créer une règle de pare-feu** s’ouvre. Remplissez le formulaire pour créer une nouvelle règle de pare-feu. Pour plus d’informations, consultez [Règles de pare-feu](/azure/sql-database/sql-database-firewall-configure).

   ![Nouvelle règle de pare-feu](media/quickstart-sql-database/firewall.png)  

Une fois la connexion établie, votre serveur s'ouvre dans la barre latérale **SERVEURS**.

## <a name="create-the-tutorial-database"></a>Créer la base de données didacticiel

Les sections suivantes créent la base de données TutorialDB utilisée dans d'autres didacticiels Azure Data Studio.

1. Cliquez avec le bouton droit sur votre serveur Azure SQL dans la barre latérale **SERVEURS**, puis sélectionnez **Nouvelle requête**.

1. Collez ce SQL dans l’éditeur de requête.

   ```sql
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO

   ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
   GO
   ```

1. Dans la barre d’outils, sélectionnez **Exécuter**. Les notifications s'affichent dans le volet **MESSAGES** pour afficher la progression de la requête.

## <a name="create-a-table"></a>Créer une table

L’éditeur de requête est connecté à la base de données **MASTER**, mais nous souhaitons créer une table dans la base de données **TutorialDB**. 

1. Connectez-vous à la base de données **TutorialDB**.

   ![Modifier le contexte](media/quickstart-sql-database/change-context2.png)



1. Créez une table `Customers`. 

   Remplacez la requête précédente dans l’éditeur de requête par celle-ci, puis sélectionnez **Exécuter**.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows-into-the-table"></a>Insérer des lignes dans la table

Remplacez la requête précédente par celle-ci, puis sélectionnez **Exécuter**.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="view-the-result"></a>Afficher le résultat

Remplacez la requête précédente par celle-ci, puis sélectionnez **Exécuter**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

Les résultats de la requête s’affichent :

   ![Sélectionner les résultats](media/quickstart-sql-database/select-results2.png)


## <a name="clean-up-resources"></a>Nettoyer les ressources

Les articles de démarrage rapide ultérieurs reposent sur les ressources créées ici. Si vous envisagez de travailler avec ces articles, veillez à ne pas supprimer ces ressources. Sinon, dans le portail Azure, supprimez les ressources dont vous n’avez plus besoin. Pour plus d’informations, consultez [Nettoyer les ressources](/azure/sql-database/sql-database-get-started-portal#clean-up-resources).

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez réussi à vous connecter à une base de données Azure SQL et à exécuter une requête, essayez le [Didacticiel de l’éditeur de code](tutorial-sql-editor.md).
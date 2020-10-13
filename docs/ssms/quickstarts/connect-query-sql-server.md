---
title: Se connecter à une instance SQL Server et l’interroger
description: Connectez-vous à une instance SQL Server en utilisant SQL Server Management Studio et en exécutant des requêtes T-SQL de base.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.custom: seo-lt-2019
ms.date: 09/28/2020
ms.openlocfilehash: ba646353b0ded0a1cc4617c1b4c9ffc3c159662e
ms.sourcegitcommit: 9386ae1b90705a39d37d5541b70c5e8a6564f253
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91662789"
---
# <a name="quickstart-connect-to-and-query-a-sql-server-instance-by-using-sql-server-management-studio-ssms"></a>Démarrage rapide : Se connecter à une instance SQL Server et l’interroger en utilisant SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Ce guide de démarrage rapide vous apprend à utiliser SSMS (SQL Server Management Studio) pour vous connecter à votre instance SQL Server et exécuter des commandes T-SQL (Transact-SQL) de base. L’article explique comment suivre les étapes ci-dessous :

> [!div class="checklist"]
> * Se connecter à une instance de SQL Server
> * Créer une base de données ("TutorialDB")
> * Créer une table ("Customers") dans votre nouvelle base de données
> * Insérer des lignes dans votre nouvelle table
> * Interroger la nouvelle table et afficher les résultats
> * Utiliser la table de fenêtre de requête pour vérifier les propriétés de votre connexion
> * Changer le serveur auquel votre fenêtre de requête est connectée

## <a name="prerequisites"></a>Prérequis

Pour effectuer les étapes de cet article, vous avez besoin de SQL Server Management Studio et d’un accès à une instance SQL Server.

* Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

Si vous n’avez pas accès à une instance SQL Server, sélectionnez votre plateforme parmi les liens suivants. Si vous choisissez l’authentification SQL, utilisez vos informations d’identification de connexion SQL Server.

* **Windows** : [Télécharger SQL Server 2019 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* **macOS** : [Télécharger SQL Server 2019 sur Docker](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker).

## <a name="connect-to-a-sql-server-instance"></a>Se connecter à une instance de SQL Server

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Exécutez SQL Server Management Studio. La première fois que vous exécutez SSMS, la fenêtre **Se connecter au serveur** s’ouvre. Si elle ne s’ouvre pas, vous pouvez l’ouvrir manuellement en sélectionnant **Explorateur d’objets** > **Se connecter** > **Moteur de base de données**.

    ![Lien Se connecter dans l’Explorateur d’objets](media/connect-query-sql-server/connect-object-explorer.png)

2. Dans la fenêtre **Se connecter au serveur**, suivez les instructions de la liste ci-dessous :

    * Pour **Type de serveur**, sélectionnez **Moteur de base de données** (généralement l’option par défaut).
    * Pour **Nom du serveur**, entrez le nom de votre instance SQL Server. (Cet article utilise le nom d’instance SQL2016ST sur le nom d’hôte NODE5 [NODE5\SQL2016ST].) Si vous ne savez pas comment déterminer le nom de votre instance SQL Server, consultez [Conseils et astuces supplémentaires sur l’utilisation de SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name).
    * Pour **Authentification**, sélectionnez **Authentification Windows**. Cet article utilise l’authentification Windows, mais la connexion SQL Server est également prise en charge. Si vous sélectionnez **Connexion SQL**, vous êtes invité à fournir un nom d’utilisateur et un mot de passe. Pour plus d’informations sur les types d’authentification, consultez [Se connecter au serveur (moteur de base de données)](https://docs.microsoft.com/sql/ssms/f1-help/connect-to-server-database-engine).

    ![Champ « Nom du serveur » avec la possibilité d’utiliser une instance SQL Server](media/connect-query-sql-server/connection-2.png)

    Vous pouvez également modifier d’autres options de connexion en sélectionnant **Options**. Exemples d’options de connexion : la base de données à laquelle vous êtes connecté, la valeur du délai d’expiration de la connexion et le protocole réseau. Cet article utilise les valeurs par défaut pour toutes les options.

3. Une fois que vous avez renseigné tous les champs, sélectionnez **Se connecter**.

### <a name="examples-of-successful-connections"></a>Exemples de connexions réussies

Pour vérifier que votre connexion au serveur SQL Server a réussi, développez et explorez les objets dans l’**Explorateur d’objets**. Ces objets varient en fonction du type de serveur auquel vous choisissez de vous connecter.

* Connexion à un serveur SQL Server local ; dans le cas présent, NODE5\SQL2016ST : ![Connexion à un serveur local](media/connect-query-sql-server/connect-on-prem.png)

* Connexion à SQL Azure DB (dans ce cas, msftestserver.database.windows.net : ![Connexion à une base de données SQL Azure DB](media/connect-query-sql-server/connect-sql-azure.png)

> [!NOTE]
> Dans cet article, vous avez utilisé précédemment l’*authentification Windows* pour vous connecter à votre serveur SQL Server local, mais cette méthode n’est pas prise en charge pour SQL Azure DB. De ce fait, c’est l’authentification SQL qui est utilisée dans cette image pour établir une connexion à la base de données SQL Azure DB. Pour plus d’informations, consultez [Authentification locale SQL](../../relational-databases/security/choose-an-authentication-mode.md) et [Authentification SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#access-management).

## <a name="create-a-database"></a>Création d'une base de données

Créez une base de données appelée TutorialDB en effectuant les étapes suivantes :

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de serveur et sélectionnez **Nouvelle requête** :

   ![Lien Nouvelle requête](media/connect-query-sql-server/new-query.png)

2. Dans la fenêtre de la requête, collez l’extrait de code T-SQL suivant :

   ```sql
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

3. Pour exécuter la requête, sélectionnez **Exécuter** (ou sélectionnez F5 sur votre clavier).

   ![Commande Exécuter](media/connect-query-sql-server/execute.png)
  
    Une fois que la requête est terminée, la nouvelle base de données TutorialDB s’affiche dans la liste des bases de données de l’Explorateur d’objets. Si elle n’apparaît pas, cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Actualiser**.  

## <a name="create-a-table-in-the-new-database"></a>Créer une table dans la nouvelle base de données

Dans cette section, vous allez créer une table dans la base de données TutorialDB que vous venez de créer. Étant donné que l’éditeur de requête est toujours dans le contexte de la base de données *master*, changez le contexte de connexion avec la base de données *TutorialDB* en effectuant les étapes suivantes :

1. Dans la liste déroulante des bases de données, sélectionnez la base de données que vous voulez, comme ici :

   ![Changer la base de données](media/connect-query-sql-server/change-db.png)

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, sélectionnez-le et sélectionnez **Exécuter** (ou sélectionnez F5 sur votre clavier).  
   Vous pouvez remplacer le texte existant dans la fenêtre de requête ou l’ajouter à la fin. Pour exécuter tous les éléments de la fenêtre de requête, sélectionnez **Exécuter**. Si vous avez ajouté le texte, vous souhaiterez exécuter uniquement la partie correspondant au texte. Donc, mettez en surbrillance cette partie, puis sélectionnez **Exécuter**.  
  
   ```sql
   USE [TutorialDB]
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

Une fois que la requête est terminée, la nouvelle table Customers s’affiche dans la liste des tables de l’Explorateur d’objets. Si la table ne s’affiche pas, cliquez avec le bouton droit sur le nœud **TutorialDB** > **Tables** dans l’Explorateur d’objets et sélectionnez **Actualiser**.

## <a name="insert-rows-into-the-new-table"></a>Insérer des lignes dans la nouvelle table

Insérez des lignes dans la table Customers que vous avez créée précédemment. Pour ce faire, collez l’extrait de code T-SQL suivant dans la fenêtre de requête et sélectionnez **Exécuter** :

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

## <a name="query-the-table-and-view-the-results"></a>Interroger la table et afficher les résultats

Les résultats d’une requête sont visibles sous la fenêtre du texte de requête. Pour interroger la table Customers et afficher les lignes qui ont été précédemment insérées, effectuez les étapes suivantes :

1. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Les résultats de la requête sont affichés sous la zone dans laquelle le texte a été entré : 

   ![Liste des résultats](media/connect-query-sql-server/query-results.png)

2. Modifiez la présentation des résultats en sélectionnant l’une des options suivantes :

     ![Trois options d’affichage des résultats de requête](media/connect-query-sql-server/results.png)

    * Le bouton du milieu affiche les résultats en **Mode grille**, qui est l’option par défaut.
    * Le premier bouton affiche les résultats en **Mode texte**, comme illustré dans l’image de la section suivante.
    * Le troisième bouton vous permet d’enregistrer les résultats dans un fichier dont l’extension est .rpt par défaut.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Vérifier les propriétés de votre connexion en utilisant la table de la fenêtre de requête

Des informations sur les propriétés de connexion figurent sous les résultats de votre requête. Après avoir exécuté la requête citée plus haut dans l’étape précédente, vérifiez les propriétés de connexion en bas de la fenêtre de requête.

* Vous pouvez identifier le serveur et la base de données auxquels vous êtes connecté, ainsi que le nom d’utilisateur employé.
* Vous pouvez également voir la durée de la requête et le nombre de lignes qui sont retournées par la requête exécutée précédemment.

    ![Propriétés de connexion](media/connect-query-sql-server/connection-properties.png)

    > [!Note]
    > Dans l’image, les résultats sont affichés en **mode texte**.

## <a name="change-the-server-based-on-the-query-window"></a>Changer le serveur en fonction de la fenêtre de requête

Vous pouvez changer le serveur auquel votre fenêtre de requête actuelle est connectée en effectuant les étapes suivantes :

1. Cliquez avec le bouton droit dans la fenêtre de requête, puis sélectionnez **Connexion** > **Changer la connexion**. La fenêtre **Se connecter au serveur** s’ouvre de nouveau.

2. Changez le serveur utilisé par votre requête.

   ![Commande Changer la connexion](media/connect-query-sql-server/change-connection.png)

    > [!NOTE]
    > Cette action change uniquement le serveur auquel la fenêtre de requête est connectée, pas le serveur utilisé par l’Explorateur d’objets.

## <a name="azure-data-studio"></a>Azure Data Studio

Vous pouvez également vous connecter et interroger [SQL Server](../../azure-data-studio/quickstart-sql-server.md), une [base de données Azure SQL](../../azure-data-studio/quickstart-sql-database.md) et des [entrepôts de données Azure SQL](../../azure-data-studio/quickstart-sql-dw.md) à l’aide d’Azure Data Studio.

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS. Ces articles vous apprennent à gérer les composants de SSMS et à trouver les fonctionnalités utilisées régulièrement.

* [Création de scripts](../tutorials/scripting-ssms.md)
* [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
* [Configuration de SSMS](../tutorials/ssms-configuration.md)
* [Conseils et astuces supplémentaires pour utiliser SSMS](../tutorials/ssms-tricks.md)
* [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)
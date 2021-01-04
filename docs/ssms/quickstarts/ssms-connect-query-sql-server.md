---
title: Se connecter à une instance SQL Server et l’interroger avec SQL Server Management Studio (SSMS)
description: Connectez-vous à une instance SQL Server dans SSMS. Créez et interrogez une base de données SQL Server dans SSMS en exécutant des requêtes T-SQL de base.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 519b60f63da38192e2196014e0ea7820dafd5491
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619098"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-using-sql-server-management-studio-ssms"></a>Démarrage rapide : Se connecter à une instance SQL Server et l’interroger avec SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Commencez à utiliser SQL Server Management Studio (SSMS) pour vous connecter à votre instance SQL Server et exécutez des commandes Transact-SQL (T-SQL).

L’article explique comment suivre les étapes ci-dessous :

> [!div class="checklist"]
> - Se connecter à une instance de SQL Server
> - Création d'une base de données
> - Créer une table dans votre nouvelle base de données
> - Insérer des lignes dans votre nouvelle table
> - Interroger la nouvelle table et afficher les résultats
> - Utiliser la table de fenêtre de requête pour vérifier les propriétés de votre connexion

## <a name="prerequisites"></a>Prérequis

- [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md) installé.
- Une [instance SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) installée et configurée.

## <a name="connect-to-a-sql-server-instance"></a>Se connecter à une instance de SQL Server

1. Exécutez SQL Server Management Studio. La première fois que vous exécutez SSMS, la fenêtre **Se connecter au serveur** s’ouvre. Si elle ne s’ouvre pas, vous pouvez l’ouvrir manuellement en sélectionnant **Explorateur d’objets** > **Se connecter** > **Moteur de base de données**.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-object-explorer.png" alt-text="Lien de connexion dans l’Explorateur d’objets":::

2. La boîte de dialogue **Se connecter au serveur** s’affiche. Entrez les informations suivantes :

    |   Paramètre   |   Valeur(s) suggérée(s)   |   Description   |
    |--------------|-----------------------|-----------------|
    | **Type de serveur** | Moteur de base de données | Pour **Type de serveur**, sélectionnez **Moteur de base de données** (généralement l’option par défaut). |
    | **Nom du serveur** | Nom complet du serveur | Pour **Nom du serveur**, entrez le nom de votre instance SQL Server (vous pouvez aussi utiliser *localhost* comme nom de serveur si vous vous connectez en local). Si vous N’utilisez PAS l’instance par défaut (***MSSQLSERVER** _), vous devez entrer le nom du serveur et le nom de l’instance. </br> </br> Si vous ne savez pas comment déterminer le nom de votre instance SQL Server, consultez [Conseils et astuces supplémentaires sur l’utilisation de SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name). |
    | _ *Authentification** | Authentification Windows </br> </br> l’authentification SQL Server | L’authentification Windows est définie par défaut. </br> </br> Vous pouvez aussi utiliser l’**authentification SQL Server** pour vous connecter. Cependant, si vous sélectionnez l’**Authentification SQL Server**, un nom d’utilisateur et un mot de passe sont nécessaires. </br> </br> Pour plus d’informations sur les types d’authentification, consultez [Se connecter au serveur (moteur de base de données)](../f1-help/connect-to-server-database-engine.md). |
    | **Connexion** | ID d’utilisateur du compte serveur | ID d’utilisateur du compte serveur utilisé pour se connecter au serveur. Une connexion est nécessaire quand l’**authentification SQL Server** est utilisée. |
    | **Mot de passe** | Mot de passe du compte serveur | Mot de passe du compte serveur utilisé pour se connecter au serveur. Un mot de passe est nécessaire quand l’**authentification SQL Server** est utilisée. |

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-to-sql-server-object-explorer.png" alt-text="Champ de nom du serveur pour SQL Server":::

3. Une fois que vous avez renseigné tous les champs, sélectionnez **Se connecter**.

    Vous pouvez également modifier d’autres options de connexion en sélectionnant **Options**. Exemples d’options de connexion : la base de données à laquelle vous êtes connecté, la valeur du délai d’expiration de la connexion et le protocole réseau. Cet article utilise les valeurs par défaut pour tous les champs.

4. Pour vérifier que votre connexion SQL Server a abouti, développez et explorez les objets dans l’**Explorateur d’objets** où figurent le nom du serveur, la version de SQL Server et le nom d’utilisateur. Ces objets sont différents selon le type de serveur.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-on-prem.png" alt-text="Connexion à un serveur local":::

## <a name="troubleshoot-connectivity-issues"></a>Résoudre les problèmes de connectivité

Pour passer en revue les techniques de résolution des problèmes à employer en cas d’échec de connexion à une instance de votre moteur de base de données SQL Server sur un serveur unique, consultez [Résoudre les problèmes de connexion au moteur de base de données SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="create-a-database"></a>Création d'une base de données

Nous allons maintenant créer une base de données nommée TutorialDB en effectuant les étapes suivantes :

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de serveur et sélectionnez **Nouvelle requête** :

   :::image type="content" source="media/ssms-connect-query-sql-server/new-query.png" alt-text="Lien Nouvelle requête":::

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête :

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

3. Exécutez la requête en sélectionnant **Exécuter** ou en sélectionnant F5 sur votre clavier.

   :::image type="content" source="media/ssms-connect-query-sql-server/execute.png" alt-text="Commande Exécuter":::
  
    Une fois que la requête est terminée, la nouvelle base de données TutorialDB s’affiche dans la liste des bases de données de l’Explorateur d’objets. Si elle n’apparaît pas, cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Actualiser**.

## <a name="create-a-table-in-the-new-database"></a>Créer une table dans la nouvelle base de données

Dans cette section, vous allez créer une table dans la base de données TutorialDB que vous venez de créer. Étant donné que l’éditeur de requête est toujours dans le contexte de la base de données *master*, changez le contexte de connexion avec la base de données *TutorialDB* en effectuant les étapes suivantes :

1. Dans la liste déroulante des bases de données, sélectionnez la base de données que vous voulez, comme ici :

   :::image type="content" source="media/ssms-connect-query-sql-server/change-db.png" alt-text="Changer la base de données":::

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête :

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

3. Exécutez la requête en sélectionnant **Exécuter** ou en sélectionnant F5 sur votre clavier.

Une fois que la requête est terminée, la nouvelle table Customers s’affiche dans la liste des tables de l’Explorateur d’objets. Si la table ne s’affiche pas, cliquez avec le bouton droit sur le nœud **TutorialDB** > **Tables** dans l’Explorateur d’objets et sélectionnez **Actualiser**.

   :::image type="content" source="media/ssms-connect-query-sql-server/new-table.png" alt-text="Nouvelle table":::

## <a name="insert-rows-into-the-new-table"></a>Insérer des lignes dans la nouvelle table

À présent, insérons quelques lignes dans la table Customers que vous avez créée. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** :

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

Les résultats d’une requête sont visibles sous la fenêtre du texte de requête. Pour interroger la table Customers et voir les lignes qui ont été insérées, suivez les étapes ci-dessous :

1. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Les résultats de la requête s’affichent en dessous de la zone où le texte a été entré.

   :::image type="content" source="media/ssms-connect-query-sql-server/query-results.png" alt-text="Liste des résultats":::

    Vous pouvez aussi modifier la façon dont les résultats sont présentés en sélectionnant une des options suivantes :

   ![Trois options d’affichage des résultats de requête](media/ssms-connect-query-sql-server/results.png)

   - Le premier bouton affiche les résultats en **Mode texte**, comme illustré dans l’image de la section suivante.
   - Le bouton du milieu affiche les résultats en **Mode grille**, qui est l’option par défaut.
       - Il s’agit du paramètre par défaut
   - Le troisième bouton vous permet d’enregistrer les résultats dans un fichier dont l’extension est .rpt par défaut.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Vérifier les propriétés de votre connexion en utilisant la table de la fenêtre de requête

Des informations sur les propriétés de connexion figurent sous les résultats de votre requête. Après avoir exécuté la requête citée plus haut dans l’étape précédente, vérifiez les propriétés de connexion en bas de la fenêtre de requête.

- Vous pouvez identifier le serveur et la base de données auxquels vous êtes connecté, ainsi que le nom d’utilisateur employé.
- Vous pouvez également voir la durée de la requête et le nombre de lignes qui sont retournées par la requête exécutée précédemment.

   :::image type="content" source="media/ssms-connect-query-sql-server/connection-properties.png" alt-text="Propriétés de connexion":::

## <a name="additional-tools"></a>Outils supplémentaires

Vous pouvez aussi utiliser [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) pour vous connecter à [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md) et [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md) et les interroger.

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS.

- [Éditeur de requêtes SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Création de scripts](../tutorials/scripting-ssms.md)
- [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
- [Configuration de SSMS](../tutorials/ssms-configuration.md)
- [Conseils et astuces supplémentaires pour utiliser SSMS](../tutorials/ssms-tricks.md)
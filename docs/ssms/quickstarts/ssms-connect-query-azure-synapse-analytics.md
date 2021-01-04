---
title: Se connecter à un pool SQL dédié dans Azure Synapse Analytics et l’interroger avec SQL Server Management Studio (SSMS)
description: Connectez-vous à un pool SQL dédié dans Azure Synapse Analytics à l’aide de SSMS. Créez et interrogez un pool SQL dédié dans Azure Synapse Analytics en exécutant des requêtes T-SQL de base dans SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 102eaff45b6cdfb89a159e3de77039c8735395e2
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619123"
---
# <a name="quickstart-connect-and-query-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>Démarrage rapide : Se connecter à un pool SQL dédié dans Azure Synapse Analytics et l’interroger avec SQL Server Management Studio (SSMS)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Commencez à utiliser SQL Server Management Studio (SSMS) pour vous connecter à votre pool SQL dédié dans Azure Synapse Analytics et exécutez des commandes Transact-SQL (T-SQL).

> [!div class="checklist"]
> - Se connecter à un pool SQL dédié dans Azure Synapse Analytics
> - Création d'une base de données
> - Créer une table dans votre nouvelle base de données
> - Insérer des lignes dans votre nouvelle table
> - Interroger la nouvelle table et afficher les résultats
> - Utiliser la table de fenêtre de requête pour vérifier les propriétés de votre connexion

## <a name="prerequisites"></a>Prérequis

Pour effectuer les étapes de cet article, vous avez besoin de SQL Server Management Studio et d’un accès à une source de données.

- Installez [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE)

## <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Se connecter à un pool SQL dédié dans Azure Synapse Analytics

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Exécutez SQL Server Management Studio. La première fois que vous exécutez SSMS, la fenêtre **Se connecter au serveur** s’ouvre. Si elle ne s’ouvre pas, vous pouvez l’ouvrir manuellement en sélectionnant **Explorateur d’objets** > **Se connecter** > **Moteur de base de données**.

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-object-explorer.png" alt-text="Lien de connexion dans l’Explorateur d’objets":::

2. Dans la fenêtre **Se connecter au serveur**, suivez les instructions de la liste ci-dessous :

    |   Paramètre   |   Valeur(s) suggérée(s)   |   Description   |
    |-------------|------------------------|-----------------|
    | **Type de serveur** | Moteur de base de données | Pour **Type de serveur**, sélectionnez **Moteur de base de données** (généralement l’option par défaut). |
    | **Nom du serveur** | Nom complet du serveur | Pour **Nom du serveur**, entrez le nom de votre serveur Azure Synapse Analytics. |
    | **Authentification** | l’authentification SQL Server | Utilisez l’**Authentification SQL Server** pour la connexion d’Azure Synapse Analytics. </br> </br> La méthode d’**authentification Windows** n’est pas prise en charge pour Azure SQL. Pour plus d’informations, consultez [Authentification Azure SQL](/azure/sql-database/sql-database-security-overview#access-management). |
    | **Connexion** | ID d’utilisateur du compte serveur | ID d’utilisateur du compte serveur utilisé pour créer le serveur. |
    | **Mot de passe** | Mot de passe du compte serveur | Mot de passe du compte serveur utilisé pour créer le serveur. |

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-to-azure-synapse-analytics-object-explorer.png" alt-text="Champ Nom du serveur pour Azure Synapse Analytics":::

3. Une fois que vous avez renseigné tous les champs, sélectionnez **Se connecter**.

    Vous pouvez également modifier d’autres options de connexion en sélectionnant **Options**. Exemples d’options de connexion : la base de données à laquelle vous êtes connecté, la valeur du délai d’expiration de la connexion et le protocole réseau. Cet article utilise les valeurs par défaut pour toutes les options.

    Si vous n’avez pas configuré les paramètres de votre pare-feu, une invite s’affiche pour configurer le pare-feu. Une fois que vous êtes connecté, renseignez les informations de connexion de votre compte Azure et continuez à définir la règle de pare-feu. Sélectionnez ensuite **OK**. Cette invite est une action unique. Une fois que vous avez configuré le pare-feu, l’invite de pare-feu ne s’affiche plus.

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Nouvelle règle de pare-feu Azure SQL":::

4. Pour vérifier que votre connexion Azure Synapse Analytics a abouti, développez et explorez les objets dans l’**Explorateur d’objets** où figurent le nom du serveur, la version de SQL Server et le nom d’utilisateur. Ces objets sont différents selon le type de serveur.

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-azure-synapse-analytics.png" alt-text="Connexion à une base de données Azure Synapse Analytics":::

## <a name="troubleshoot-connectivity-issues"></a>Résoudre les problèmes de connectivité

Vous pouvez rencontrer des problèmes de connexion avec Azure Synapse Analytics. Pour plus d’informations sur la résolution des problèmes de connexion, consultez [Résolution des problèmes de connectivité](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues).

Vous pouvez prévenir, résoudre, diagnostiquer et limiter les erreurs de connexion et les erreurs temporaires que vous rencontrez pendant vos interactions avec Azure SQL Database ou Azure SQL Managed Instance. Pour plus d’informations, consultez [Résoudre les erreurs de connexion temporaires](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-connectivity-issues).

## <a name="create-a-database"></a>Création d'une base de données

Nous allons maintenant créer une base de données nommée TutorialDB en effectuant les étapes suivantes :

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre instance de serveur et sélectionnez **Nouvelle requête** :

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-query.png" alt-text="Lien Nouvelle requête":::

2. Collez l’extrait de code T-SQL suivant dans la fenêtre de requête :

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

3. Exécutez la requête en sélectionnant **Exécuter** ou en sélectionnant F5 sur votre clavier.

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/execute.png" alt-text="Commande Exécuter":::
  
    Une fois que la requête est terminée, la nouvelle base de données TutorialDB s’affiche dans la liste des bases de données de l’Explorateur d’objets. Si elle n’apparaît pas, cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Actualiser**.

## <a name="create-a-table-in-the-new-database"></a>Créer une table dans la nouvelle base de données

Dans cette section, vous allez créer une table dans la base de données TutorialDB que vous venez de créer. Étant donné que l’éditeur de requête est toujours dans le contexte de la base de données *master*, changez le contexte de connexion avec la base de données *TutorialDB* en effectuant les étapes suivantes :

1. Dans la liste déroulante des bases de données, sélectionnez la base de données que vous voulez, comme ici :

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/change-db.png" alt-text="Changer la base de données":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-table.png" alt-text="Nouvelle table":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/query-results.png" alt-text="Liste des résultats":::

    Vous pouvez aussi modifier la façon dont les résultats sont présentés en sélectionnant une des options suivantes :

   ![Trois options d’affichage des résultats de requête](media/ssms-connect-query-azure-synapse-analytics/results.png)

   - Le premier bouton affiche les résultats en **Mode texte**, comme illustré dans l’image de la section suivante.
   - Le bouton du milieu affiche les résultats en **Mode grille**, qui est l’option par défaut.
       - Il s’agit du paramètre par défaut
   - Le troisième bouton vous permet d’enregistrer les résultats dans un fichier dont l’extension est .rpt par défaut.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Vérifier les propriétés de votre connexion en utilisant la table de la fenêtre de requête

Des informations sur les propriétés de connexion figurent sous les résultats de votre requête. Après avoir exécuté la requête citée plus haut dans l’étape précédente, vérifiez les propriétés de connexion en bas de la fenêtre de requête.

- Vous pouvez identifier le serveur et la base de données auxquels vous êtes connecté, ainsi que le nom d’utilisateur employé.
- Vous pouvez également voir la durée de la requête et le nombre de lignes qui sont retournées par la requête exécutée précédemment.

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connection-properties.png" alt-text="Propriétés de connexion":::

## <a name="additional-tools"></a>Outils supplémentaires

Vous pouvez aussi utiliser [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) pour vous connecter à [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md) et [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md) et les interroger.

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS.

- [Éditeur de requêtes SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Création de scripts](../tutorials/scripting-ssms.md)
- [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
- [Configuration de SSMS](../tutorials/ssms-configuration.md)
- [Conseils et astuces supplémentaires pour utiliser SSMS](../tutorials/ssms-tricks.md)
---
title: Se connecter et interroger avec Azure Synapse Analytics
description: Ce guide démarrage rapide montre comment se connecter à un pool SQL dédié dans Azure Synapse Analytics avec Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 8215e84c4c61935b3f5831ca771e2868e27be428
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091722"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Démarrage rapide : Connexion et interrogation de données avec Azure Data Studio à l’aide d’un pool SQL dédié dans Azure Synapse Analytics

Ce guide démarrage rapide montre comment se connecter à un pool SQL dédié dans Azure Synapse Analytics avec Azure Data Studio.

## <a name="prerequisites"></a>Prérequis
Pour suivre ce guide démarrage rapide, vous avez besoin d’Azure Data Studio et d’un pool SQL dédié dans Azure Synapse Analytics.

- [Installez Azure Data Studio](./download-azure-data-studio.md).

Si vous n’avez pas encore de pool SQL dédié, consultez [Créer un pool SQL dédié](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

N’oubliez pas le nom du serveur et les informations d’identification de connexion !


## <a name="connect-to-your-dedicated-sql-pool"></a>Se connecter à votre pool SQL dédié

Utilisez Azure Data Studio pour établir une connexion à votre serveur Azure Synapse Analytics.

1. La première fois que vous exécutez Azure Data Studio, la page **Connexion** doit s’ouvrir. Si la page **Connexion** n’apparaît pas, sélectionnez **Ajouter une connexion** ou l’icône **Nouvelle connexion** dans la barre latérale **SERVEURS** :
   
   ![Capture d’écran de la page Connexion avec l’icône Nouvelle connexion mise en évidence.](media/quickstart-sql-dw/new-connection-icon.png)

2. Cet article utilise la *connexion SQL*, mais *l'authentification Windows* est aussi prise en charge. Renseignez les champs comme suit en utilisant le nom du serveur, le nom d’utilisateur et le mot de passe de *votre* serveur Azure SQL :

   |   Paramètre    | Valeur suggérée | Description |
   |--------------|-----------------|-------------| 
   | **Nom du serveur** | Nom complet du serveur | Par exemple, le nom se présente ainsi : **sqlpoolservername.database.windows.net**. |
   | **Authentification** | Connexion SQL| L’authentification SQL est utilisée dans ce didacticiel. |
   | **Nom d'utilisateur** | Compte d’administrateur de serveur | Il s’agit du compte que vous avez spécifié lorsque vous avez créé le serveur. |
   | **Mot de passe (connexion SQL)** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié lorsque vous avez créé le serveur. |
   | **Enregistrer le mot de passe ?** | Oui ou Non | Sélectionnez Oui si vous ne souhaitez pas entrer le mot de passe à chaque fois. |
   | **Nom de la base de données** | *laisser vide* | Nom de la base de données à laquelle se connecter. |
   | **Groupe de serveurs** | Sélectionnez <Default> | Si vous avez créé un groupe de serveurs, vous pouvez le définir sur un groupe de serveurs spécifique. | 

3. Si votre serveur ne dispose pas d’une règle de pare-feu autorisant Azure Data Studio à se connecter, le formulaire **Créer une règle de pare-feu** s’ouvre. Remplissez le formulaire pour créer une nouvelle règle de pare-feu. Pour plus d’informations, consultez [Règles de pare-feu](/azure/sql-database/sql-database-firewall-configure).

4. Une fois la connexion établie, votre serveur s'ouvre dans la barre latérale *Serveurs*.

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>Création d’une base de données dans un pool SQL dédié

1. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre serveur et sélectionnez **Nouvelle requête**.

2. Collez l’extrait suivant dans l’éditeur de requête, puis sélectionnez **Exécuter** :

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>Créer une table

L’éditeur de requête est toujours connecté à la base de données *MASTER*, mais nous souhaitons créer une table dans la base de données *TutorialDB*. 

1. Remplacez le contexte de connexion par **TutorialDB** :

2. Collez l’extrait suivant dans l’éditeur de requête, puis sélectionnez **Exécuter** :

   > [!NOTE]
   > Vous pouvez ajouter cela à ou remplacer la requête précédente dans l’éditeur. À savoir : le fait de sélectionner **Exécuter** a seulement pour effet d’exécuter la requête sélectionnée. Si rien n’est sélectionné, toutes les requêtes de l’Éditeur sont exécutées.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="Création d’une table dans la base de données TutorialDB":::


## <a name="insert-rows"></a>Insérer des lignes

1. Collez l’extrait suivant dans l’éditeur de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="Création de lignes dans la table":::

## <a name="view-the-result"></a>Afficher le résultat

1. Collez l’extrait suivant dans l’éditeur de requête, puis sélectionnez **Exécuter** :

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. Les résultats de la requête sont affichés :

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="Affichage des résultats":::


## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous n’avez pas l’intention de continuer à utiliser les exemples de bases de données créés dans cet article, [supprimez le groupe de ressources](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources).

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez [Connexion à Synapse SQL avec Azure Data Studio](/azure/synapse-analytics/sql/get-started-azure-data-studio).

Maintenant que vous avez réussi à vous connecter à Azure Synapse Analytics et à exécuter une requête, essayez le [tutoriel de l’éditeur de code](tutorial-sql-editor.md).
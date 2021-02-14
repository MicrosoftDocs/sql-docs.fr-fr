---
title: 'Démarrage rapide : Se connecter à et interroger PostgreSQL'
description: Suivez un guide de démarrage rapide dans lequel vous utiliserez Azure Data Studio pour vous connecter à PostgreSQL, puis utiliserez des instructions SQL pour créer et interroger une base de données.
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: dad9f6348b3df9d7589be739af5ba7c748d37b6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040081"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>Démarrage rapide : Utilisez Azure Data Studio pour vous connecter et interroger PostgreSQL

Ce guide de démarrage rapide montre comment utiliser Azure Data Studio pour se connecter à PostgreSQL, puis utiliser des instructions SQL pour créer la base de données *tutorialdb* et l’interroger.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce guide de démarrage rapide, vous avez besoin d’Azure Data Studio, de l’extension PostgreSQL pour Azure Data Studio et de l’accès à un serveur PostgreSQL.

- [Installez Azure Data Studio](./download-azure-data-studio.md).
- [Installez l’extension PostgreSQL pour Azure Data Studio](./extensions/postgres-extension.md).
- [Installez PostgreSQL](https://www.postgresql.org/download/). (Vous pouvez également créer une base de données Postgres dans le cloud avec la commande [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli)). 

## <a name="connect-to-postgresql"></a>Se connecter à PostgreSQL

1. Démarrez **Azure Data Studio**.

2. La première fois que vous démarrez Azure Data Studio, la boîte de dialogue **Connexion** s’ouvre. Si la boîte de dialogue **Connexion** ne s’ouvre pas, cliquez sur l'icône **Nouvelle connexion** sur la page **SERVEUR** :

   ![Icône de nouvelle connexion](media/quickstart-postgresql/new-connection-icon.png)

3. Dans le formulaire qui s’affiche, accédez à **Type de connexion**, puis sélectionnez **PostgreSQL** dans la liste déroulante.


4. Renseignez les champs restants en utilisant le nom du serveur, le nom d’utilisateur et le mot de passe de votre serveur PostgreSQL. 

   ![Écran de nouvelle connexion](media/quickstart-postgresql/new-connection-screen.png)  

   | Paramètre       | Valeur d'exemple | Description |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | localhost | Nom complet du serveur |
   | **Nom d'utilisateur** | postgres | Nom d’utilisateur avec lequel vous souhaitez vous connecter. |
   | **Mot de passe (connexion SQL)** | *mot de passe* | Mot de passe du compte avec lequel vous vous connectez. |
   | **Mot de passe** | *Vérification* | Cochez cette case si vous ne souhaitez pas entrer le mot de passe chaque fois que vous vous connectez. |
   | **Nom de la base de données** | \<Default\> | Renseignez ce champ si vous souhaitez que la connexion spécifie une base de données. |
   | **Groupe de serveurs** | \<Default\> | Cette option vous permet d’attribuer cette connexion à un groupe de serveurs spécifique que vous créez. | 
   | **Nom (facultatif)** | *laisser vide* | Cette option vous permet de spécifier un nom convivial pour votre serveur. | 

5. Sélectionnez **Connecter**. 

Une fois la connexion établie, votre serveur s'ouvre dans la barre latérale **SERVEURS**.


## <a name="create-a-database"></a>Création d'une base de données

La procédure suivante crée une base de données nommée **tutorialdb** :

1. Cliquez avec le bouton droit sur votre serveur PostgreSQL dans la barre latérale **SERVEURS**, puis sélectionnez **Nouvelle requête**.

2. Collez cette instruction SQL dans l’éditeur de requête qui s’ouvre.

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. Dans la barre d'outils, sélectionnez **Exécuter** pour exécuter la requête. Les notifications s'affichent dans le volet **MESSAGES** pour afficher la progression de la requête.

>[!TIP]
> Vous pouvez utiliser la touche **F5** de votre clavier pour exécuter l’instruction au lieu d'**Exécuter**.

Une fois la requête terminée, cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Actualiser** pour afficher **tutorialdb** dans la liste sous le nœud **Bases de données**.


## <a name="create-a-table"></a>Créer une table

 Les étapes suivantes créent une table dans **tutorialdb** :

1. Modifiez le contexte de connexion à **tutorialdb** à l’aide de la liste déroulante dans l’éditeur de requête. 

   ![Modifier le contexte](media/quickstart-postgresql/change-context.png)

2. Collez l’instruction SQL suivante dans l’éditeur de requête et cliquez sur **Exécuter**. 

   > [!NOTE]
   > Vous pouvez ajouter cela ou remplacer la requête existante dans l’éditeur. Cliquer sur **Exécuter** exécute uniquement la requête qui est mise en surbrillance. Si rien n’est mis en surbrillance, cliquez sur **Exécuter** pour exécuter toutes les requêtes dans l’éditeur.

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>Insérer des lignes

Collez l’extrait suivant dans la fenêtre de requête, puis cliquez sur **Exécuter** :

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>Interroger les données

1. Collez l’extrait suivant dans l’éditeur de requête, puis cliquez sur **Exécuter** :
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. Les résultats de la requête sont affichés :

   ![Afficher les résultats](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les [scénarios disponibles pour Postgres dans Azure Data Studio.](./extensions/postgres-extension.md)
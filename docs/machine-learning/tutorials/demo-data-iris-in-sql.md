---
description: Données de démonstration Iris pour les tutoriels Python et R dans le Machine Learning SQL
title: Jeu de données de démonstration Iris pour les tutoriels
titleSuffix: SQL machine learning
Description: Créez une base de données contenant le jeu de données Iris et des modèles prédictifs. Ce jeu de données est utilisé dans les tutoriels R et Python avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 25cc9c7c93786dcc0d88e8bcd152196c17bf91c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347314"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>Données de démonstration Iris pour les tutoriels Python et R dans le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Dans cet exercice, vous allez créer une base de données pour stocker les données du [jeu de données des fleurs Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) et des modèles basés sur les mêmes données. Les données Iris, incluses dans les distributions R et Python, sont utilisées dans les tutoriels de Machine Learning pour le Machine Learning SQL.

Pour effectuer cet exercice, vous devez avoir [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) ou un autre outil capable d’exécuter des requêtes T-SQL.

Voici les tutoriels et les démarrages rapides qui utilisant ce jeu de données :

+ [Démarrage rapide : Création et scoring d’un modèle prédictif en Python](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>Création de la base de données

1. Démarrez SQL Server Management Studio, puis ouvrez une nouvelle fenêtre **Requête**.  

2. Créez une base de données pour ce projet, puis changez le contexte de votre fenêtre **Requête** de façon à utiliser la nouvelle base de données.

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. Ajoutez des tables vides : une pour stocker les données et une autre pour stocker les modèles entraînés. La table **iris_models** sert à stocker les modèles sérialisés générés dans les autres exercices.

    Le code suivant crée la table pour les données d’entraînement.

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. Exécutez le code suivant pour créer la table qui va stocker le modèle entraîné. Pour enregistrer des modèles Python (ou R) dans SQL Server, ils doivent être sérialisés et stockés dans une colonne de type **varbinary(max)** .

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    Outre le contenu du modèle, il est courant d’ajouter aussi des colonnes pour d’autres métadonnées utiles, comme le nom du modèle, la date à laquelle il a été entraîné, l’algorithme et les paramètres sources, les données sources, etc. Dans un souci de simplicité, nous allons pour l’instant utiliser uniquement le nom du modèle.

## <a name="populate-the-table"></a>Remplir la table

Vous pouvez obtenir les données Iris intégrées à partir de R ou de Python. Vous pouvez utiliser Python ou R pour charger les données dans une trame de données, puis insérer celle-ci dans une table de la base de données. Le processus de déplacement de données d’apprentissage d’une session externe dans une table comporte plusieurs étapes :

+ Concevez une procédure stockée qui obtient les données qui vous intéressent.
+ Exécutez la procédure stockée pour récupérer réellement les données.
+ Construisez une instruction INSERT pour spécifier l’emplacement d’enregistrement des données récupérées.

1. Sur les systèmes dotés de l’intégration Python, créez la procédure stockée ci-dessous qui utilise du code Python pour charger les données.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    Quand vous exécutez ce code, vous obtenez normalement le message suivant : « Commandes réussies. » Cela signifie que la procédure stockée a été créée selon vos spécifications.

2. Autre possibilité : sur les systèmes dotés de l’intégration R, créez une procédure qui utilise R à la place.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. Pour remplir réellement la table, exécutez la procédure stockée et spécifiez la table dans laquelle les données doivent être écrites. Quand elle est exécutée, la procédure stockée exécute le code Python ou R, qui charge le jeu de données Iris intégré, puis insère les données dans la table **iris_data**.

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    Si vous débutez avec T-SQL, sachez que l’instruction INSERT ajoute uniquement de nouvelles données ; elle ne recherche pas de données existantes ni ne supprime et regénère la table. Pour éviter d’avoir plusieurs copies des mêmes données dans une table, vous pouvez d’abord exécuter cette l’instruction : `TRUNCATE TABLE iris_data`. L’instruction T-SQL [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) supprime les données existantes, mais conserve la structure de la table intacte.

## <a name="query-the-data"></a>Interroger les données

En guise d’étape de validation, exécutez une requête pour confirmer que les données ont été chargées.

1. Dans l’Explorateur d’objets, sous Bases de données, cliquez avec le bouton droit sur la base de données **irissql**, puis démarrez une nouvelle requête.

2. Exécutez des requêtes simples :

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>Étapes suivantes

Dans le démarrage rapide suivant, vous allez créer un modèle de Machine Learning et l’enregistrer dans une table. Vous vous en servirez ensuite pour générer des résultats prédits.

+ [Démarrage rapide : Création et scoring d’un modèle prédictif en Python](quickstart-python-train-score-model.md)
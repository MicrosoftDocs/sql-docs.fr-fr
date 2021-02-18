---
title: 'Tutoriel : Déployer un modèle prédictif en R'
titleSuffix: SQL machine learning
description: Dans la quatrième partie de ce tutoriel qui en contient 4, vous allez déployer un modèle prédictif dans R avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0f264f0996a3597085c8e5103eed9ebc0ee2c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348049"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutoriel : Déployer un modèle prédictif dans R avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle Machine Learning développé en R dans SQL Server Machine Learning Services ou sur Clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle Machine Learning dans R dans SQL Server à l’aide de Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle Machine Learning dans R dans SQL Server à l’aide de SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle Machine Learning développé en R dans Azure SQL Managed Instance à l’aide de Machine Learning Services.
::: moniker-end

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Créer une procédure stockée qui génère le modèle Machine Learning
> * Stocker le modèle dans une table de base de données
> * Créer une procédure stockée qui effectue des prédictions à l’aide du modèle
> * Exécuter le modèle avec de nouvelles données

Dans la [première partie](r-predictive-model-introduction.md), vous avez appris à restaurer l’exemple de base de données.

Dans la [deuxième partie](r-predictive-model-prepare-data.md), vous avez appris à importer un exemple de base de données, puis à préparer les données destinées à être utilisées pour effectuer l’apprentissage d’un modèle prédictif dans R.

Dans la [troisième partie](r-predictive-model-train.md), vous avez appris à créer et à effectuer l’apprentissage de plusieurs modèles Machine Learning dans R, puis à choisir le plus précis.

## <a name="prerequisites"></a>Prérequis

La quatrième partie de ce tutoriel part du principe que vous avez satisfait les prérequis de la [**première partie**](r-predictive-model-introduction.md) et effectué les étapes décrites dans la [**deuxième partie**](r-predictive-model-prepare-data.md) et la [**troisième partie**](r-predictive-model-train.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Créer une procédure stockée qui génère le modèle

Dans la troisième partie de cette série de tutoriels, vous avez décidé qu’un modèle d’arbre de décision (dtree) était le plus précis. Maintenant, à l’aide des scripts en R que vous avez développés, créez une procédure stockée (`generate_rental_model`) qui entraîne et génère le modèle dtree avec rpart à partir du package R.

Exécutez les commandes suivantes dans Azure Data Studio.

```sql
USE [TutorialDB]
DROP PROCEDURE IF EXISTS generate_rental_model;
GO
CREATE PROCEDURE generate_rental_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
rental_train_data$Month   <- factor(rental_train_data$Month);
rental_train_data$Day     <- factor(rental_train_data$Day);
rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
library(rpart);
model_dtree <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Stocker le modèle dans une table de base de données

Créez une table dans la base de données TutorialDB, puis enregistrez le modèle dans la table.

1. Créez une table (`rental_models`) pour stocker le modèle.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_models;
    GO
    CREATE TABLE rental_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Enregistrez le modèle dans la table en tant qu’objet binaire, en utilisant le nom de modèle « DTree ».

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_model @model OUTPUT;
    
    INSERT INTO rental_models (
          model_name
        , model
        )
    VALUES (
         'DTree'
        , @model
        );
    
    SELECT *
    FROM rental_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Créer une procédure stockée qui effectue des prédictions

Créez une procédure stockée (`predict_rentalcount_new`) qui effectue des prédictions à l’aide du modèle entraîné et d’un jeu de nouvelles données.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
USE [TutorialDB]
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @model VARBINARY(MAX) = (
            SELECT model
            FROM rental_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
#Convert types to factors
rentals$Month   <- factor(rentals$Month);
rentals$Day     <- factor(rentals$Day);
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(model);

#Call prediction function
rental_predictions <- predict(rental_model, rentals);
rental_predictions <- data.frame(rental_predictions);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@model varbinary(max)'
        , @model = @model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Exécuter le modèle avec de nouvelles données

Vous pouvez désormais utiliser la procédure stockée `predict_rentalcount_new` pour prédire le nombre de locations à partir de nouvelles données.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'DTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Vous devez voir un résultat similaire à celui qui suit.

```results
RentalCount_Predicted
332.571428571429
```

Vous avez créé, entraîné et déployé un modèle dans une base de données. Vous avez ensuite utilisé ce modèle dans une procédure stockée pour prédire des valeurs en fonction de nouvelles données.


## <a name="clean-up-resources"></a>Nettoyer les ressources

Quand vous n’avez plus besoin de la base de données TutorialDB, supprimez-la de votre serveur.

## <a name="next-steps"></a>Étapes suivantes

Dans la quatrième partie de cette série de tutoriels, vous avez appris à effectuer les tâches suivantes :

* Créer une procédure stockée qui génère le modèle Machine Learning
* Stocker le modèle dans une table de base de données
* Créer une procédure stockée qui effectue des prédictions à l’aide du modèle
* Exécuter le modèle avec de nouvelles données

Pour en savoir plus sur l’utilisation de R dans Machine Learning Services, consultez :

* [Exécuter des scripts R simples](quickstart-r-create-script.md)
* [Structures de données, types et objets R](quickstart-r-data-types-and-objects.md)
* [Fonctions R](quickstart-r-functions.md)

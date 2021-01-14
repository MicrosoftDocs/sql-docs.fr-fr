---
title: 'Démarrage rapide : Effectuer l’apprentissage d’un modèle dans R'
titleSuffix: SQL machine learning
description: Dans ce guide de démarrage rapide, vous allez créer et effectuer l’apprentissage d’un modèle prédictif à l’aide de R. Vous allez enregistrer le modèle dans une table, puis utiliser le modèle pour prédire des valeurs à partir de nouvelles données avec l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/23/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 21e815a95ebc6a407749195323748d6c73c19e00
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100167"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-r-with-sql-machine-learning"></a>Démarrage rapide : Créer et scorer un modèle prédictif dans R avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans ce guide de démarrage rapide, vous allez créer et effectuer l’apprentissage d’un modèle prédictif à l’aide de R. Vous allez enregistrer le modèle dans une table de votre instance de SQL Server, puis utiliser le modèle pour prédire des valeurs à partir de nouvelles données avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
Dans ce guide de démarrage rapide, vous allez créer et effectuer l’apprentissage d’un modèle prédictif à l’aide de R. Vous allez enregistrer le modèle dans une table de votre instance de SQL Server, puis utiliser le modèle pour prédire des valeurs à partir de nouvelles données avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2016"
Dans ce guide de démarrage rapide, vous allez créer et effectuer l’apprentissage d’un modèle prédictif à l’aide de R. Vous allez enregistrer le modèle dans une table de votre instance de SQL Server, puis utiliser le modèle pour prédire des valeurs à partir de nouvelles données avec [SQL Server R Services](../r/sql-server-r-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans ce démarrage rapide, vous allez créer et entraîner un modèle prédictif en R. Vous l’enregistrerez dans une table de votre instance SQL Server, puis l’utiliserez pour prédire des valeurs à partir de nouvelles données avec [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Vous allez créer et exécuter deux procédures stockées qui s’exécutent dans SQL. La première utilise le jeu de données **mtcars** inclus avec R et génère un modèle linéaire généralisé simple (GLM) qui prédit la probabilité qu’un véhicule ait été équipé d’une transmission manuelle. La deuxième procédure concerne le scoring : elle appelle le modèle généré dans la première procédure pour générer un ensemble de prédictions basées sur de nouvelles données. En plaçant le code R dans une procédure stockée SQL, les opérations sont contenues dans SQL et sont réutilisables. Elles peuvent alors être appelées par d’autres procédures stockées et applications clientes.

> [!TIP]
> Si vous avez besoin de rafraîchir vos connaissances sur les modèles linéaires, consultez le tutoriel suivant, qui décrit le processus d’ajustement des modèles linéaire à l’aide de rxLInMod : [Ajustement des modèles linéaires](/machine-learning-server/r/how-to-revoscaler-linear-model)

En suivant ce guide de démarrage rapide, vous apprendrez :

> [!div class="checklist"]
> - Comment incorporer du code R dans une procédure stockée
> - Comment transmettre des entrées dans votre code via des entrées sur la procédure stockée
> - Comment les procédures stockées sont utilisées pour rendre les modèles opérationnels

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce démarrage rapide, vous avez besoin de ce qui suit.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server Machine Learning Services. Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server Machine Learning Services. Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Pour installer R Services, consultez le [Guide d’installation Windows](../install/sql-r-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Un outil pour exécuter les requêtes SQL qui contiennent des scripts R. Ce guide de démarrage rapide utilise [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="create-the-model"></a>Créer le modèle

Pour créer le modèle, vous allez créer des données sources pour l’apprentissage, créer le modèle et effectuer son apprentissage à l’aide des données, puis stocker le modèle dans une base de données où il peut être utilisé pour générer des prédictions avec de nouvelles données.

### <a name="create-the-source-data"></a>Créer la source de données

1. Ouvrez Azure Data Studio, connectez-vous à votre instance et ouvrez une nouvelle fenêtre de requête.

1. Créez une table pour enregistrer les données d’apprentissage.

   ```sql
   CREATE TABLE dbo.MTCars(
       mpg decimal(10, 1) NOT NULL,
       cyl int NOT NULL,
       disp decimal(10, 1) NOT NULL,
       hp int NOT NULL,
       drat decimal(10, 2) NOT NULL,
       wt decimal(10, 3) NOT NULL,
       qsec decimal(10, 2) NOT NULL,
       vs int NOT NULL,
       am int NOT NULL,
       gear int NOT NULL,
       carb int NOT NULL
   );
   ```

1. Insérez les données à partir du jeu de données intégré `mtcars`.

   ```SQL
   INSERT INTO dbo.MTCars
   EXEC sp_execute_external_script @language = N'R'
       , @script = N'MTCars <- mtcars;'
       , @input_data_1 = N''
       , @output_data_1_name = N'MTCars';
   ```

   > [!TIP]
   > De nombreux jeux de données, petits et grands, sont inclus avec le runtime R. Pour obtenir la liste des datasets installées avec R, tapez `library(help="datasets")` à partir d’une invite de commandes R.

### <a name="create-and-train-the-model"></a>Créer et effectuer l’apprentissage du modèle

Les données de vitesse de voiture contiennent deux colonnes, toutes deux numériques, puissance (`hp`) et poids (`wt`). À partir de ces données, vous allez créer un modèle GLM qui estime la probabilité qu’un véhicule ait été équipé d’une transmission manuelle.

Pour créer le modèle, vous devez définir la formule à l’intérieur de votre code R et passer les données en tant que paramètre d’entrée.

```sql
DROP PROCEDURE IF EXISTS generate_GLM;
GO
CREATE PROCEDURE generate_GLM
AS
BEGIN
    EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'carsModel <- glm(formula = am ~ hp + wt, data = MTCarsData, family = binomial);
        trained_model <- data.frame(payload = as.raw(serialize(carsModel, connection=NULL)));'
    , @input_data_1 = N'SELECT hp, wt, am FROM MTCars'
    , @input_data_1_name = N'MTCarsData'
    , @output_data_1_name = N'trained_model'
    WITH RESULT SETS ((model VARBINARY(max)));
END;
GO
```

- Le premier argument de `glm` est le paramètre *formula* qui définit `am` comme étant dépendante de `hp + wt`.
- Les données d’entrée sont stockées dans la variable `MTCarsData`, qui est renseignée par la requête SQL. Si vous n’attribuez pas de nom spécifique à vos données d’entrée, le nom de variable par défaut est _InputDataSet_.

### <a name="store-the-model-in-the-database"></a>Stocker le modèle dans la base de données

Ensuite, stockez le modèle dans une base de données afin de pouvoir l’utiliser à des fins de prédiction ou de reformation. 

1. Créez une table pour stocker le modèle.

   La sortie d’un package R qui crée un modèle est généralement un objet binaire. Par conséquent, la table dans laquelle vous stockez le modèle doit contenir une colonne de type **varbinary(max)** .

   ```sql
   CREATE TABLE GLM_models (
       model_name varchar(30) not null default('default model') primary key,
       model varbinary(max) not null
   );
   ```

1. Exécutez l’instruction Transact-SQL suivante de façon à appeler la procédure stockée, à générer le modèle, puis à l’enregistrer dans la table créée.

   ```sql
   INSERT INTO GLM_models(model)
   EXEC generate_GLM;
   ```

   > [!TIP]
   > Si vous exécutez ce code une deuxième fois, vous obtenez cette erreur : Violation de la contrainte CLÉ PRIMAIRE... Impossible d’insérer une clé en double dans l’objet dbo.stopping_distance_models. Pour éviter cette erreur, vous pouvez mettre à jour le nom de chaque nouveau modèle. Vous pouvez par exemple utiliser un nom plus descriptif et y inclure le type de modèle, le jour de sa création, etc.

     ```sql
     UPDATE GLM_models
     SET model_name = 'GLM_' + format(getdate(), 'yyyy.MM.HH.mm', 'en-gb')
     WHERE model_name = 'default model'
     ```

## <a name="score-new-data-using-the-trained-model"></a>Noter les nouvelles données à l’aide du modèle formé

La *notation* est un terme utilisé dans la science des données pour signifier la génération de prédictions, de probabilités ou d’autres valeurs en fonction de nouvelles données introduites dans un modèle formé. Vous utiliserez le modèle que vous avez créé dans la section précédente pour noter les prédictions en fonction des nouvelles données.

### <a name="create-a-table-of-new-data"></a>Créer une table de nouvelles données

Tout d’abord, créez une table avec de nouvelles données.

```sql
CREATE TABLE dbo.NewMTCars(
    hp INT NOT NULL
    , wt DECIMAL(10,3) NOT NULL
    , am INT NULL
)
GO

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (110, 2.634)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (72, 3.435)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (220, 5.220)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (120, 2.800)
GO
```

### <a name="predict-manual-transmission"></a>Prédire la transmission manuelle

Pour générer des prédictions basées sur votre modèle, vous devez écrire un script SQL qui effectue les opérations suivantes :

1. Obtenir le modèle voulu
1. Obtenir les nouvelles données d’entrée
1. Appeler une fonction de prédiction R compatible avec ce modèle

Vous utilisez peut-être une table qui contient plusieurs modèles R, créés avec des paramètres ou algorithmes différents, ou formés à partir de plusieurs sous-ensembles de données. Dans cet exemple, nous allons utiliser le modèle nommé `default model`.

```sql
DECLARE @glmmodel varbinary(max) = 
    (SELECT model FROM dbo.GLM_models WHERE model_name = 'default model');

EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'
            current_model <- unserialize(as.raw(glmmodel));
            new <- data.frame(NewMTCars);
            predicted.am <- predict(current_model, new, type = "response");
            str(predicted.am);
            OutputDataSet <- cbind(new, predicted.am);
            '
    , @input_data_1 = N'SELECT hp, wt FROM dbo.NewMTCars'
    , @input_data_1_name = N'NewMTCars'
    , @params = N'@glmmodel varbinary(max)'
    , @glmmodel = @glmmodel
WITH RESULT SETS ((new_hp INT, new_wt DECIMAL(10,3), predicted_am DECIMAL(10,3)));
```

Le script ci-dessus effectue les étapes suivantes :

- Utilisez une instruction SELECT pour récupérer un modèle dans la table et le passer en tant que paramètre d’entrée.

- Après avoir récupéré le modèle dans la table, appelez la fonction `unserialize` sur le modèle.

- Appliquez la fonction `predict` avec les arguments appropriés au modèle de fonction, et fournissez les nouvelles données d’entrée.

> [!NOTE]
> Dans l’exemple, la fonction `str` est ajoutée pendant le test pour vérifier le schéma de données renvoyées à partir de R. Vous pourrez supprimer l’instruction ultérieurement si vous le souhaitez.
>
> Les noms de colonne utilisés dans le script R ne sont pas nécessairement transmis à la sortie de la procédure stockée. Ici, la clause WITH RESULTs est utilisée pour définir de nouveaux noms de colonnes.

**Résultats**

![Jeu de résultats pour la probabilité de prédiction d’une transmission manuelle](./media/r-predict-am-resultset.png)

Il est également possible d’utiliser l’instruction [PREDICT (Transact-SQL)](../../t-sql/queries/predict-transact-sql.md) pour générer une valeur prédite ou un score basé sur un modèle stocké.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les didacticiels pour R avec l’apprentissage automatique SQL, consultez :

- [Tutoriels sur R](r-tutorials.md)
---
title: Scoring natif avec T-SQL PREDICT
titleSuffix: SQL machine learning
description: Découvrez comment utiliser le scoring natif avec la fonction T-SQL PREDICT afin de générer des valeurs de prédiction pour de nouvelles entrées de données en quasi-temps réel.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 74fc66dd6ed235d7508f774e7397c9b4d55e80a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352545"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>Scoring natif à l’aide de la fonction T-SQL PREDICT avec le Machine Learning SQL

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Découvrez comment utiliser le scoring natif avec la [fonction T-SQL PREDICT](../../t-sql/queries/predict-transact-sql.md) afin de générer des valeurs de prédiction pour de nouvelles entrées de données en quasi-temps réel. Le scoring natif implique de disposer d’un modèle déjà entraîné.

La fonction `PREDICT` utilise les fonctionnalités d’extension C++ natives du [Machine Learning SQL](../index.yml). Cette méthodologie, qui offre la vitesse de traitement la plus rapide possible pour les charges de travail de prévision et de prédiction, prend en charge les modèles au format [ONNX (Open Neural Network Exchange)](https://onnx.ai/get-started.html) et les modèles entraînés à l’aide des packages [RevoScaleR](../r/ref-r-revoscaler.md) et [revoscalepy](../python/ref-py-revoscalepy.md).

## <a name="how-native-scoring-works"></a>Fonctionnement du scoring natif

Le scoring natif utilise des bibliothèques permettant de lire des modèles au format ONNX ou dans un format binaire prédéfini et de générer des scores pour les nouvelles entrées de données fournies. Le modèle étant entraîné, déployé et stocké, vous pouvez l’utiliser pour le scoring sans avoir à appeler l’interpréteur R ou Python. La surcharge liée aux interactions de plusieurs processus s’en trouve donc réduite, ce qui accroît les performances de prédiction.

Pour utiliser le scoring natif, appelez la fonction T-SQL `PREDICT` et transmettez les entrées obligatoires suivantes :

+ Un modèle compatible basé sur un modèle et un algorithme pris en charge
+ Des données d’entrée, généralement définies sous forme de requête T-SQL

La fonction retourne des prédictions pour les données d’entrée ainsi que toutes les colonnes de données sources que vous souhaitez passer.

## <a name="prerequisites"></a>Prérequis

`PREDICT` est disponible sur :

+ Toutes les éditions de SQL Server 2017 et versions ultérieures sur Windows et Linux
+ Azure SQL Managed Instance
+ Azure SQL Database
+ Azure SQL Edge
+ Azure Synapse Analytics

La fonction est activée par défaut. Il n’est pas nécessaire d’installer R ou Python ni d’activer des fonctionnalités supplémentaires.

## <a name="supported-models"></a>Modèles pris en charge

Les formats de modèle pris en charge par la fonction `PREDICT` dépendent de la plateforme SQL sur laquelle est effectué le scoring natif. Consultez le tableau ci-dessous pour connaître les formats de modèle pris en charge selon la plateforme.

| Plateforme | Format de modèle ONNX | Format de modèle RevoScale |
|-|-|-|
| SQL Server | Non | Oui |
| Azure SQL Managed Instance | Oui | Oui |
| Azure SQL Database | Non | Oui |
| Azure SQL Edge | Oui | Non |
| Azure Synapse Analytics | Oui | Non |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>Modèles ONNX

Le modèle doit être au format [ONNX (Open Neural Network Exchange)](https://onnx.ai/get-started.html).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>Modèles RevoScale

Le modèle doit être entraîné à l’avance avec l’un des algorithmes **rx** pris en charge de la liste ci-dessous à l’aide du package [RevoScaleR](../r/ref-r-revoscaler.md) ou [revoscalepy](../python/ref-py-revoscalepy.md).

Sérialisez le modèle avec [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) pour R et [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) pour Python. Ces fonctions de sérialisation ont été optimisées pour prendre en charge le scoring rapide.

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>Algorithmes RevoScale pris en charge

Les algorithmes suivants sont pris en charge dans revoscalepy et RevoScaleR.

+ Algorithmes revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ Algorithmes RevoScaleR

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

Si vous devez utiliser un algorithme de MicrosoftML ou microsoftml, optez pour le [scoring en temps réel avec sp_rxPredict](../predictions/real-time-scoring.md).

Parmi les types de modèles non pris en charge, citons les suivants :

+ Modèles contenant d’autres transformations
+ Modèles utilisant les algorithmes `rxGlm` ou `rxNaiveBayes` dans les équivalents RevoScaleR ou revoscalepy
+ Modèles PMML
+ Modèles créés à l’aide d’autres bibliothèques open source ou tierces
::: moniker-end

## <a name="examples"></a>Exemples
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>PREDICT avec un modèle ONNX

Cet exemple montre comment utiliser un modèle ONNX stocké dans la table `dbo.models` pour le scoring natif.

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> Les colonnes et les valeurs retournées par **PREDICT** pouvant varier selon le type de modèle, vous devez définir le schéma des données retournées à l’aide d’une clause **WITH**.
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>PREDICT avec un modèle RevoScale

Dans cet exemple, vous créez un modèle à l’aide de **RevoScaleR** en R, puis appelez la fonction de prédiction en temps réel à partir de T-SQL.

#### <a name="step-1-prepare-and-save-the-model"></a>Étape 1. Préparer et enregistrer le modèle

Exécutez le code suivant pour créer l’exemple de base de données et les tables obligatoires.

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

Utilisez l’instruction suivante pour remplir la table de données avec les données du jeu de données **iris**.

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

Créez à présent une table pour stocker les modèles.

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

Le code suivant crée un modèle basé sur le jeu de données **iris** et l’enregistre dans la table nommée **models**.

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> Veillez à utiliser la fonction [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) de RevoScaleR pour enregistrer le modèle. La fonction R `serialize` standard ne peut pas générer le format demandé.

Vous pouvez exécuter une instruction telle que la suivante pour afficher le modèle stocké au format binaire :

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>Étape 2. Exécuter PREDICT sur le modèle

L’instruction PREDICT simple suivante obtient une classification à partir du modèle d’arbre de décision à l’aide de la fonction de **scoring natif**. Elle prédit les espèces d’iris en fonction des attributs que vous fournissez (longueur et largeur des pétales).

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

Le message « Une erreur s’est produite lors de l’exécution de la fonction PREDICT. Le modèle est endommagé ou non valide. » signifie généralement que votre requête n’a pas retourné de modèle. Vérifiez que vous avez correctement tapé le nom du modèle ou que la table des modèles n’est pas vide.

> [!NOTE]
> Les colonnes et les valeurs retournées par **PREDICT** pouvant varier selon le type de modèle, vous devez définir le schéma des données retournées à l’aide d’une clause **WITH**.
::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

+ [Fonction T-SQL PREDICT](../../t-sql/queries/predict-transact-sql.md)
+ [Documentation sur SQL Machine Learning](../index.yml)
+ [Machine Learning et IA avec ONNX dans SQL Edge](/azure/azure-sql-edge/onnx-overview)
+ [Déploiement et prédictions avec un modèle ONNX dans Azure SQL Edge](/azure/azure-sql-edge/deploy-onnx)
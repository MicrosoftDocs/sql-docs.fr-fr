---
title: 'Python + T-SQL : Exécuter des prédictions'
description: Tutoriel montrant comment rendre opérationnel un script Python incorporé dans des procédures stockées SQL Server avec des fonctions T-SQL
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/02/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 0d6994aade329150d2715eca92181f4cf2773c40
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85775324"
---
# <a name="run-predictions-using-python-embedded-in-a-stored-procedure"></a>Exécuter des prédictions avec un script Python incorporé dans une procédure stockée
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article fait partie d’un tutoriel, [Analytique Python en base de données pour développeur SQL](sqldev-in-database-python-for-sql-developers.md). 

Dans cette étape, vous allez apprendre à *rendre opérationnels* les modèles que vous avez entraînés et enregistrés à l’étape précédente.

Dans ce scénario, rendre opérationnel signifie déployer le modèle en production pour le scoring. L’intégration avec SQL Server facilite ce processus, car vous pouvez incorporer du code Python dans une procédure stockée. Pour obtenir des prédictions du modèle sur la base de nouvelles entrées, il vous suffit d’appeler la procédure stockée à partir d’une application et de transmettre les nouvelles données.

Cette leçon présente deux méthodes de création de prédictions sur la base d’un modèle Python : le scoring par lot et le scoring ligne par ligne.

- **Scoring par lot :** pour fournir plusieurs lignes de données d’entrée, transmettez une requête SELECT sous forme d’argument à la procédure stockée. Il en résulte une table d’observations correspondant aux cas d’entrée.
- **Scoring individuel :** transmettez un ensemble de valeurs de paramètres individuels sous forme d’entrée.  La procédure stockée retourne une seule ligne ou valeur.

Tout le code Python nécessaire au scoring est fourni dans les procédures stockées.

## <a name="batch-scoring"></a>Scoring par lot

Les deux premières procédures stockées illustrent la syntaxe de base pour l’encapsulation d’un appel de prédiction Python dans une procédure stockée. Les deux procédures stockées nécessitent une table de données sous forme d’entrées.

- Le nom du modèle exact à utiliser est fourni sous forme de paramètre d’entrée à la procédure stockée. La procédure stockée charge le modèle sérialisé à partir de la table de base de données `nyc_taxi_models`.table, en utilisant l’instruction SELECT de la procédure stockée.
- Le modèle sérialisé est stocké dans la variable Python `mod` pour un traitement plus poussé avec Python.
- Les nouveaux cas qui doivent être scorés sont obtenus à partir de la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifiée dans `@input_data_1`. Lors de la lecture des données de requête, les lignes sont enregistrées dans la trame de données par défaut, `InputDataSet`.
- Les deux procédures stockées utilisent des fonctions de `sklearn` pour calculer une métrique d’exactitude, AUC (Area Under Curve, zone en dessous de la courbe). Les métriques d’exactitude comme AUC peuvent être générées uniquement si vous fournissez aussi l’étiquette cible (la colonne _tipped_). Les prédictions n’ont pas besoin de l’étiquette cible (variable `y`), contrairement au calcul des métriques d’exactitude.

    Par conséquent, si vous n’avez pas d’étiquettes cibles pour les données à scorer, vous pouvez modifier la procédure stockée de façon à supprimer les calculs AUC et retourner uniquement les probabilités de pourboire à partir des caractéristiques (variable `X` dans la procédure stockée).

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

Exécutez les instructions T-SQL suivantes pour créer les procédures stockées. Cette procédure stockée a besoin d’un modèle basé sur le package scikit-learn, car elle utilise des fonctions propres à ce package :

+ La trame de données contenant les entrées est transmise à la fonction `predict_proba` du modèle de régression logistique, `mod`. La fonction `predict_proba` (`probArray = mod.predict_proba(X)`) retourne une valeur flottante (**float**) qui représente la probabilité qu’un pourboire (d’un montant quelconque) sera donné.

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Cette procédure stockée utilise les mêmes entrées et crée le même type de score que la procédure stockée précédente, sauf qu’elle utilise des fonctions du package **revoscalepy** fourni avec SQL Server Machine Learning.

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Exécuter un scoring par lot à l’aide d’une requête SELECT

Les procédures stockées **PredictTipSciKitPy** et **PredictTipRxPy** ont besoin de deux paramètres d’entrée : 

- Requête qui récupère les données pour le scoring
- Nom d’un modèle entraîné

En transmettant ces arguments à la procédure stockée, vous pouvez sélectionner un modèle particulier ou modifier les données utilisées pour le scoring.

1. Pour utiliser le modèle **scikit-learn** pour le scroring, appelez la procédure stockée **PredictTipSciKitPy**, en transmettant le nom du modèle et la chaîne de requête sous forme d’entrées.

    ```sql
    DECLARE @query_string nvarchar(max) -- Specify input query
      SET @query_string='
      select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
      dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
      from nyctaxi_sample_testing'
    EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
    ```

    La procédure stockée retourne les probabilités prédites pour chaque trajet qui a été transmis avec la requête d’entrée. 
    
    Si vous utilisez SSMS (SQL Server Management Studio) pour exécuter les requêtes, les probabilités s’affichent sous forme de tableau dans le volet **Résultats**. Le volet **Messages** sort la métrique d’exactitude (AUC ou zone en dessous de la courbe) avec une valeur d’environ 0,56.

2. Pour utiliser le modèle **revoscalepy** pour le scroring, appelez la procédure stockée **PredictTipRxPy**, en transmettant le nom du modèle et la chaîne de requête sous forme d’entrées.

    ```sql
    DECLARE @query_string nvarchar(max) -- Specify input query
      SET @query_string='
      select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
      dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
      from nyctaxi_sample_testing'
    EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
    ```

## <a name="single-row-scoring"></a>Scoring sur une ligne

Parfois, à la place du scoring par lot, vous souhaiterez peut-être transmettre un seul cas pour obtenir les valeurs d’une application et retourner un seul résultat basé sur ces valeurs. Par exemple, vous pouvez configurer une feuille de calcul Excel, une application web ou un rapport pour appeler la procédure stockée et lui transmettre les entrées tapées ou sélectionnées par les utilisateurs.

Dans cette section, vous allez créer des prédictions uniques en appelant deux procédures stockées :

+ [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) est conçu pour un scoring sur une seule ligne utilisant le modèle scikit-learn.
+ [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) est conçu pour un scoring sur une seule ligne utilisant le modèle revoscalepy.
+ Si vous n’avez pas encore entraîné un modèle, revenez à l’[étape 5](sqldev-py5-train-and-save-a-model-using-t-sql.md).

Les deux modèles acceptent en entrée une série de valeurs uniques, comme le nombre de passagers, la distance du trajet, etc. Les valeurs de latitude et de longitude des entrées sont converties par une fonction table (`fnEngineerFeatures`) pour former une nouvelle caractéristique : distance directe. La [leçon 4](sqldev-py4-create-data-features-using-t-sql.md) contient une description de cette fonction table.

Les deux procédures stockées créent un score basé sur le modèle Python.

> [!NOTE]
> 
> Quand vous appelez la procédure stockée à partir d’une application externe, il est important de fournir toutes les caractéristiques d’entrée dont a besoin le modèle Python. Pour éviter les erreurs, vous devrez peut-être effectuer un cast ou une conversion des données d’entrée vers un type de données Python, en plus de valider le type de données et la longueur des données.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Prenez une minute pour examiner le code de la procédure stockée qui effectue le scoring à l’aide du modèle **scikit-learn**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

La procédure stockée suivante effectue le scoring à l’aide du modèle **revoscalepy**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Générer des scores à partir de modèles

Une fois que les procédures stockées ont été créées, il est facile de générer un score basé sur l’un ou l’autre des modèles. Ouvrez simplement une nouvelle fenêtre **Requête**, puis tapez ou collez les paramètres pour chacune des colonnes de caractéristiques. Les sept valeurs obligatoires sont destinées à des colonnes de caractéristiques, dans l’ordre :
    
+ *passenger_count*
+ *trip_distance* v*trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Pour générer une prédiction à l’aide du modèle **revoscalepy**, exécutez l’instruction suivante :
  
    ```sql
    EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

2. Pour générer un score à l’aide du modèle **scikit-learn**, exécutez l’instruction suivante :

    ```sql
    EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

La sortie des deux procédures est une probabilité de versement de pourboire pour la course du taxi à partir des paramètres ou des caractéristiques spécifiés.

## <a name="conclusions"></a>Conclusions

Dans ce tutoriel, vous avez découvert comment utiliser du code Python incorporé dans des procédures stockées. Du fait de l’intégration avec [!INCLUDE[tsql](../../includes/tsql-md.md)], il est beaucoup plus facile de déployer des modèles Python pour la prédiction et d’incorporer le réentraînement de modèle dans un workflow de données d’entreprise.

## <a name="previous-step"></a>Étape précédente

[Entraîner et enregistrer un modèle Python](sqldev-py5-train-and-save-a-model-using-t-sql.md)

## <a name="see-also"></a>Voir aussi

[Extension Python dans SQL Server](../concepts/extension-python.md)

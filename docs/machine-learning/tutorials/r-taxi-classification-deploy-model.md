---
title: 'Didacticiel R : Exécuter des prédictions dans les procédures stockées SQL'
titleSuffix: SQL machine learning
description: Dans la cinquième et dernière partie de ce tutoriel, vous allez rendre opérationnel le script R incorporé dans les procédures stockées SQL avec des fonctions T-SQL avec SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: a3c46e719f4889ee53c98e671bd9569193b86e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349066"
---
# <a name="r-tutorial-run-predictions-in-sql-stored-procedures"></a>Didacticiel R : Exécuter des prédictions dans les procédures stockées SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Dans la cinquième et dernière partie de ce tutoriel, vous apprendrez à *rendre opérationnel* le modèle que vous avez formé et enregistré dans la partie précédente en utilisant le modèle pour prédire des résultats potentiels. Le modèle est encapsulé dans une procédure stockée qui peut être appelée directement par d’autres applications.

Cet article illustre deux façons d’effectuer le scoring :

+ **Mode de scoring par lot** : utilisez une requête SELECT comme entrée de la procédure stockée. La procédure stockée retourne une table d’observations correspondant aux cas d’entrée.

+ **Mode de calcul de score individuel**: passer un ensemble de valeurs de paramètres en tant qu’entrée.  La procédure stockée retourne une seule ligne ou valeur.

Dans cet article, vous allez :

> [!div class="checklist"]
> + Créer et utiliser des procédures stockées pour le scoring par lots
> + Créer et utiliser des procédures stockées pour le scoring d’une seule ligne

Dans la [première partie](r-taxi-classification-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](r-taxi-classification-explore-data.md), vous avez examiné les exemples de données et généré des tracés.

Dans la [troisième partie](r-taxi-classification-create-features.md), vous avez appris à créer des fonctionnalités à partir de données brutes à l’aide d’une fonction Transact-SQL. Ensuite, vous avez appelé cette fonction à partir d’une procédure stockée pour créer une table qui contient les valeurs des caractéristiques.

Dans la [quatrième partie](r-taxi-classification-train-model.md), vous avez chargé les modules et appelé les fonctions nécessaires pour créer et entraîner le modèle à l’aide d’une procédure stockée SQL Server.

## <a name="basic-scoring"></a>Scoring de base

La procédure stockée **RPredict** illustre la syntaxe de base pour le wrapping d’un appel `PREDICT` dans une procédure stockée.

```sql
CREATE PROCEDURE [dbo].[RPredict] (@model varchar(250), @inquery nvarchar(max))
AS 
BEGIN 

DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);  
EXEC sp_execute_external_script @language = N'R',
  @script = N' 
    mod <- unserialize(as.raw(model));
    print(summary(mod))
    OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
    str(OutputDataSet)
    print(OutputDataSet)
    ',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max)',
  @model = @lmodel2 
  WITH RESULT SETS (("Score" float));
END
GO
```

+ L’instruction SELECT obtient le modèle sérialisé à partir de la base de données et stocke le modèle dans la variable R `mod` pour un traitement ultérieur en utilisant R.

+ Les nouveaux cas de scoring sont obtenus à partir de la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifiée dans `@inquery`, premier paramètre de la procédure stockée. Lors de la lecture des données de requête, les lignes sont enregistrées dans la trame de données par défaut, `InputDataSet`. Ce dataframe est passé à la fonction [PREDICT](../../t-sql/queries/predict-transact-sql.md) qui génère les scores.
  
  `OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));`
  
  Une trame de données ne pouvant contenir qu’une seule ligne, vous pouvez utiliser le même code pour le calcul de score unique ou de lot.
  
+ La valeur retournée par la fonction `PREDICT` est une valeur flottante (**float**) qui représente la probabilité que le chauffeur recevra un pourboire, quel qu’en soit le montant.

## <a name="batch-scoring-a-list-of-predictions"></a>Scoring par lot (liste de prédictions)

Un scénario plus courant est celui où des prédictions sont générées pour plusieurs observations en mode batch. Dans cette étape, nous allons voir comment fonctionne le scoring par lot.

1. Commençons par nous procurer un jeu de données d’entrée plus petit sur lequel travailler. Cette requête crée une liste « top 10 » des trajets avec le nombre de passagers et d’autres caractéristiques nécessaires pour établir une prédiction.
  
   ```sql
   SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance
   
   FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a

   LEFT OUTER JOIN

   (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052)    )b

   ON a.medallion=b.medallion AND a.hack_license=b.hack_license 
   AND a.pickup_datetime=b.pickup_datetime
   WHERE b.medallion IS NULL
   ```

   **Exemples de résultats**

   ```text
   passenger_count   trip_time_in_secs    trip_distance  dropoff_datetime          direct_distance
   1                 283                  0.7            2013-03-27 14:54:50.000   0.5427964547
   1                 289                  0.7            2013-02-24 12:55:29.000   0.3797099614
   1                 214                  0.7            2013-06-26 13:28:10.000   0.6970098661
   ```

2. Créez une procédure stockée appelée **RPredictBatchOutput** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

   ```sql
   CREATE PROCEDURE [dbo].[RPredictBatchOutput] (@model varchar(250), @inquery nvarchar(max))
   AS
   BEGIN
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script 
     @language = N'R',
     @script = N'
       mod <- unserialize(as.raw(model));
       print(summary(mod))
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet)
       print(OutputDataSet)
     ',
     @input_data_1 = @inquery,
     @params = N'@model varbinary(max)',
     @model = @lmodel2
     WITH RESULT SETS ((Score float));
   END
   ```

3. Fournissez le texte de requête dans une variable et transmettez-le en tant que paramètre à la procédure stockée :

   ```sql
   -- Define the input data
   DECLARE @query_string nvarchar(max)
   SET @query_string='SELECT TOP 10 a.passenger_count as passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance FROM  (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample  )a   LEFT OUTER JOIN (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052))b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'
   
   -- Call the stored procedure for scoring and pass the input data
   EXEC [dbo].[RPredictBatchOutput] @model = 'RTrainLogit_model', @inquery = @query_string;
   ```
  
La procédure stockée retourne une série de valeurs représentant la prédiction pour chacun des 10 principaux trajets. Cependant, les principaux trajets sont aussi des trajets à passager unique relativement courts, pour lesquels le chauffeur a peu de chances de recevoir un pourboire.

> [!TIP]
> Au lieu de retourner simplement les résultats « pourboire/pas de pourboire », vous pouvez aussi retourner le score de probabilité pour la prédiction, puis appliquer une clause WHERE aux valeurs de la colonne _Score_ pour classer le score dans la catégorie « pourboire probable » ou « pourboire peu probable », en utilisant une valeur de seuil, par exemple 0,5 ou 0,7. Cette étape n’est pas incluse dans la procédure stockée, mais elle est facile à implémenter.

## <a name="single-row-scoring-of-multiple-inputs"></a>Scoring sur une ligne de plusieurs entrées

Parfois, vous souhaitez transmettre plusieurs valeurs d’entrée et obtenir une seule prédiction à partir de ces valeurs. Par exemple, vous pouvez configurer une feuille de calcul Excel, une application web ou un rapport Reporting Services pour appeler la procédure stockée et fournir des entrées tapées ou sélectionnées par les utilisateurs de ces applications.

Dans cette section, vous allez créer des prédictions uniques en utilisant une procédure stockée qui prend plusieurs entrées, comme le nombre de passagers, la distance du trajet, etc. La procédure stockée crée un score basé sur le modèle R stocké précédemment.
  
Si vous appelez la procédure stockée à partir d’une application externe, vérifiez que les données correspondent aux critères du modèle R. Vous pourriez par exemple vérifier que les données d’entrée peuvent être transtypées ou converties en un type de données R, ou valider le type de données et la longueur des données.

1. Créez une procédure stockée **RPredictSingleRow**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPredictSingleRow] @model varchar(50), @passenger_count int = 0, @trip_distance float = 0, @trip_time_in_secs int = 0, @pickup_latitude float = 0, @pickup_longitude float = 0, @dropoff_latitude float = 0, @dropoff_longitude float = 0
   AS
   BEGIN
   DECLARE @inquery nvarchar(max) = N'SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs,  @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)';
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script  
     @language = N'R',
     @script = N'  
       mod <- unserialize(as.raw(model));  
       print(summary(mod));  
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet);
       print(OutputDataSet); 
       ',  
     @input_data_1 = @inquery,  
     @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,@trip_time_in_secs int ,  @pickup_latitude float ,@pickup_longitude float ,@dropoff_latitude float ,@dropoff_longitude float', @model = @lmodel2, @passenger_count =@passenger_count, @trip_distance=@trip_distance, @trip_time_in_secs=@trip_time_in_secs, @pickup_latitude=@pickup_latitude, @pickup_longitude=@pickup_longitude, @dropoff_latitude=@dropoff_latitude, @dropoff_longitude=@dropoff_longitude  
     WITH RESULT SETS ((Score float));  
   END
   ```

2. Essayez-la en fournissant les valeurs manuellement.
  
   Ouvrez une nouvelle fenêtre **Requête** et appelez la procédure stockée en fournissant des valeurs pour chacun des paramètres. Les paramètres représentent les colonnes des caractéristiques utilisées par le modèle et sont obligatoires.

   ```sql
   EXEC [dbo].[RPredictSingleRow] @model = 'RTrainLogit_model',
   @passenger_count = 1,
   @trip_distance = 2.5,
   @trip_time_in_secs = 631,
   @pickup_latitude = 40.763958,
   @pickup_longitude = -73.973373,
   @dropoff_latitude =  40.782139,
   @dropoff_longitude = -73.977303
   ```

   Vous pouvez aussi utiliser cette forme abrégée pour les [paramètres d’une procédure stockée](../../relational-databases/stored-procedures/specify-parameters.md) :
  
   ```sql
   EXEC [dbo].[RPredictSingleRow] 'RTrainLogit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

3. Les résultats indiquent que la probabilité de recevoir un pourboire est faible (zéro) sur ces 10 principaux trajets, qui sont tous des trajets à passager unique sur une distance relativement courte.

## <a name="conclusions"></a>Conclusions

Maintenant que vous savez comment incorporer du code R dans les procédures stockées, vous pouvez étendre ces pratiques pour créer vos propres modèles. L’intégration à [!INCLUDE[tsql](../../includes/tsql-md.md)] simplifie grandement le déploiement de modèles R pour la prédiction et l’incorporation de la reformation de modèle dans le cadre d’un workflow de données d’entreprise.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :

> [!div class="checklist"]
> + Créer et utiliser des procédures stockées pour le scoring par lots
> + Créer et utiliser des procédures stockées pour le scoring d’une seule ligne

Pour plus d’informations sur R, consultez [Extension R dans SQL Server](../concepts/extension-r.md).
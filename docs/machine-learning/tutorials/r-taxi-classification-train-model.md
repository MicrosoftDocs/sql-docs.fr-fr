---
title: 'Didacticiel R : Entraîner et enregistrer le modèle'
titleSuffix: SQL machine learning
description: Dans la quatrième des cinq parties de ce tutoriel, vous allez effectuer l'apprentissage et enregistrer un modèle dans R à l’aide de Transact-SQL sur SQL Server avec SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 3b5606b9f14006632bfa2782d1d547b4565e36ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350933"
---
# <a name="r-tutorial-train-and-save-model"></a>Didacticiel R : Entraîner et enregistrer le modèle
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Dans la quatrième des cinq parties de notre tutoriel, vous découvrirez comment entraîner un modèle Machine Learning à l’aide d’un script R. Vous entraînerez le modèle à l’aide des fonctionnalités de données que vous avez créées au cours de la partie précédente, puis vous enregistrerez le modèle formé dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ici, vous pouvez tout effectuer à partir de SQL, car les packages R sont déjà installés avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].

Dans cet article, vous allez :

> [!div class="checklist"]
> + Créer et effectuer l’apprentissage d’un modèle à l’aide d’une procédure stockée SQL
> + Enregistrer le modèle formé dans une table SQL

Dans la [première partie](r-taxi-classification-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](r-taxi-classification-explore-data.md), vous avez examiné les exemples de données et généré des tracés.

Dans la [troisième partie](r-taxi-classification-create-features.md), vous avez appris à créer des fonctionnalités à partir de données brutes à l’aide d’une fonction Transact-SQL. Ensuite, vous avez appelé cette fonction à partir d’une procédure stockée pour créer une table qui contient les valeurs des caractéristiques.

Dans la [cinquième partie](r-taxi-classification-deploy-model.md), vous apprendrez à rendre opérationnels les modèles que vous avez formés et enregistrés dans la quatrième partie.

## <a name="create-the-stored-procedure"></a>Créer la procédure stockée

Lorsque vous appelez R à partir de T-SQL, vous utilisez la procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Toutefois, si les processus sont souvent répétés, notamment pour entraîner à nouveau un modèle, il est plus facile d’encapsuler l’appel à `sp_execute_external_script` dans une autre procédure stockée.

1. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez une nouvelle fenêtre de **requête**.

2. Exécutez l’instruction suivante pour créer la procédure stockée **RTrainLogitModel**. Cette procédure stockée définit les données d’entrée et utilise **glm** pour créer un modèle de régression logistique.

   ```sql
   CREATE PROCEDURE [dbo].[RTrainLogitModel] (@trained_model varbinary(max) OUTPUT)
   
   AS
   BEGIN
     DECLARE @inquery nvarchar(max) = N'
       select tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance,
       pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
       from nyctaxi_sample
       tablesample (70 percent) repeatable (98052)
   '
   
     EXEC sp_execute_external_script @language = N'R',
                                     @script = N'
   ## Create model
   logitObj <- glm(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet, family = binomial)
   summary(logitObj)
   
   ## Serialize model 
   trained_model <- as.raw(serialize(logitObj, NULL));
   ',
     @input_data_1 = @inquery,
     @params = N'@trained_model varbinary(max) OUTPUT',
     @trained_model = @trained_model OUTPUT; 
   END
   GO
   ```

   + Pour que des données soient disponibles pour le test du modèle, 70 % des données sont sélectionnées aléatoirement à des fins d’entraînement à partir de la table de données sur les taxis.

   + La requête SELECT utilise la fonction scalaire personnalisée *fnCalculateDistance* pour calculer la distance directe entre les points de prise en charge et de dépose. Les résultats de la requête sont stockés dans la variable d’entrée R par défaut, `InputDataset`.
  
   + Le script R appelle la fonction R **glm** pour créer le modèle de régression logistique.
  
     La variable binaire _tipped_ est utilisée comme *étiquette* ou colonne de résultat, et le modèle est adapté à l’aide de ces colonnes de caractéristiques :  _passenger_count_, _trip_distance_, _trip_time_in_secs_ et _direct_distance_.
  
   + Le modèle entraîné (enregistré dans la variable R `logitObj`) est sérialisé et retourné en tant que paramètre de sortie.

## <a name="train-and-deploy-the-r-model-using-the-stored-procedure"></a>Entraîner et déployer le modèle R à l’aide de la procédure stockée

Étant donné que la procédure stockée contient déjà une définition des données d’entrée, vous n’avez pas besoin de fournir de requête d’entrée.

1. Pour entraîner et déployer le modèle R, appelez la procédure stockée et insérez-la dans la table de base de données _nyc_taxi_models_ afin de pouvoir l’utiliser pour de futures prédictions :

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC RTrainLogitModel @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('RTrainLogit_model', @model);
   ```

2. Consultez la fenêtre **Messages** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour voir les messages éventuellement redirigés vers le flux **stdout** de R. Par exemple : 

   « Message(s) STDOUT provenant du script externe : Lignes lues : 1193025, Nombre total de lignes traitées : 1193025, durée totale de la segmentation : 0,093 seconde »

3. Une fois l’instruction terminée, ouvrez la table *nyc_taxi_models*. Le traitement des données et l’ajustement du modèle peuvent prendre un certain temps.

   Vous pouvez voir qu’une nouvelle ligne a été ajoutée et que celle-ci contient le modèle sérialisé dans la colonne _model_ ainsi que le nom de modèle **RTrainLogit_model** dans la colonne _name_.

   ```text
   model                        name
   ---------------------------- ------------------
   0x580A00000002000302020....  RTrainLogit_model
   ```

Dans partie suivante de ce tutoriel, vous allez utiliser le modèle entraîné pour générer des prédictions.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :

> [!div class="checklist"]
> + Créer et effectuer l’apprentissage d’un modèle à l’aide d’une procédure stockée SQL
> + Enregistrer le modèle formé dans une table SQL

> [!div class="nextstepaction"]
> [Didacticiel R : Exécuter des prédictions dans les procédures stockées SQL](r-taxi-classification-deploy-model.md)

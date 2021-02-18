---
title: 'Didacticiel R : Déployer un modèle'
description: Découvrez comment déployer des modèles R dans un environnement de production en appelant un modèle formé à partir d’une procédure stockée.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1830c441e51465f9c7cf7eab7ca503eadd677caf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272970"
---
# <a name="deploy-the-r-model-and-use-it-in-sql-server-walkthrough"></a>Déployer le modèle R et l’utiliser dans SQL Server (procédure pas à pas)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Dans cette leçon, découvrez comment déployer des modèles R dans un environnement de production en appelant un modèle formé à partir d’une procédure stockée. Vous pouvez appeler la procédure stockée à partir de R ou de n’importe quel langage de programmation d’application prenant en charge [!INCLUDE[tsql](../../includes/tsql-md.md)] (tel que C#, Java, Python, etc.) et utiliser le modèle afin d’effectuer des prédictions sur de nouvelles observations.

Cet article illustre les deux méthodes les plus courantes d’utilisation d’un modèle pour le scoring :

> [!div class="checklist"]
> * **Le mode de scoring par lot** génère plusieurs prédictions
> * **Le mode de scoring individuel** génère une prédiction à la fois

## <a name="batch-scoring"></a>Scoring par lot

Créez une procédure stockée, *PredictTipBatchMode*, qui génère plusieurs prédictions, en transmettant une requête SQL ou une table comme entrée. Une table de résultats est retournée, que vous pouvez insérer directement dans une table ou écrire dans un fichier.

- Elle obtient un jeu de données d’entrée sous la forme d’une requête SQL.
- Elle appelle le modèle de régression logistique formé que vous avez enregistré à la leçon précédente.
- Elle prédit la probabilité que le chauffeur obtienne un pourboire

1. Dans Management Studio, ouvrez une nouvelle fenêtre de requête et exécutez le script T-SQL suivant pour créer la procédure stockée PredictTipBatchMode.
  
    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipBatchMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipBatchMode] @input nvarchar(max)
    AS
    BEGIN
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model  FROM nyc_taxi_models);
      EXEC sp_execute_external_script @language = N'R',
         @script = N'
           mod <- unserialize(as.raw(model));
           print(summary(mod))
           OutputDataSet<-rxPredict(modelObject = mod,
             data = InputDataSet,
             outData = NULL,
             predVarNames = "Score", type = "response",
             writeModelVars = FALSE, overwrite = TRUE);
           str(OutputDataSet)
           print(OutputDataSet)',
      @input_data_1 = @input,
      @params = N'@model varbinary(max)',
      @model = @lmodel2
      WITH RESULT SETS ((Score float));
    END
    ```

    + Vous utilisez une instruction SELECT pour appeler le modèle stocké à partir d’une table SQL. Le modèle est récupéré à partir de la table en tant que donnée **varbinary(max)** , stocké dans la variable SQL _\@lmodel2_, puis transmis comme paramètre *mod* à la procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

    + Les données utilisées comme entrées pour le scoring sont définies en tant que requête SQL et stockées sous forme de chaîne dans _\@l’entrée_ de la variable SQL. À mesure que les données sont récupérées de la base de données, elles sont stockées dans une trame de données appelée *InputDataSet*, qui est simplement le nom par défaut des données d’entrée pour la procédure [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez définir un autre nom de variable si nécessaire à l’aide du paramètre _\@input_data_1_name_.

    + Pour générer les scores, la procédure stockée appelle la fonction rxPredict à partir de la bibliothèque **RevoScaleR**.

    + La valeur renvoyée, *Score*, est la probabilité que le chauffeur reçoive un pourboire, selon le modèle. Si vous le souhaitez, vous pouvez facilement appliquer un filtre aux valeurs renvoyées pour les regrouper selon qu’un pourboire est obtenu ou non.  Par exemple, une probabilité de moins de 0,5 signifie qu’aucun pourboire n’est susceptible d’être obtenu.
  
2.  Pour appeler la procédure stockée en mode lot, vous définissez la requête requise en tant qu’entrée dans la procédure stockée. Vous trouverez ci-dessous la requête SQL que vous pouvez exécuter dans SSMS pour vérifier qu’elle fonctionne.

    ```sql
    SELECT TOP 10
      a.passenger_count AS passenger_count,
      a.trip_time_in_secs AS trip_time_in_secs,
      a.trip_distance AS trip_distance,
      a.dropoff_datetime AS dropoff_datetime,
      dbo.fnCalculateDistance( pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance
      FROM 
        (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude 
        FROM nyctaxi_sample)a 
      LEFT OUTER JOIN
      ( SELECT medallion, hack_license, pickup_datetime
      FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b
      ON a.medallion=b.medallion
      AND a.hack_license=b.hack_license
      AND a.pickup_datetime=b.pickup_datetime
      WHERE b.medallion is null
    ```

3. Utilisez ce code R pour créer la chaîne d’entrée à partir de la requête SQL :

    ```R
    input <- "N'SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a LEFT OUTER JOIN ( SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND  a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'";
    q <- paste("EXEC PredictTipBatchMode @input = ", input, sep="");
    ```

4. Pour exécuter la procédure stockée à partir de R, appelez la méthode **sqlQuery** du package **RODBC** et utilisez la connexion SQL `conn` que vous avez définie précédemment :

    ```R
    sqlQuery (conn, q);
    ```

    Si vous recevez une erreur ODBC, recherchez les erreurs de syntaxe et si vous avez le nombre approprié de guillemets. 
    
    Si vous recevez une erreur d’autorisation, assurez-vous que le compte de connexion a la possibilité d’exécuter la procédure stockée.

## <a name="single-row-scoring"></a>Scoring à une seule ligne

Le mode de scoring individuel génère une prédiction à la fois, en transmettant un ensemble de valeurs individuelles à la procédure stockée en tant qu’entrée. Les valeurs correspondent aux fonctionnalités du modèle, que ce dernier utilise pour créer une prédiction ou générer un autre résultat comme une valeur de probabilité. Vous pouvez ensuite renvoyer cette valeur à l’application ou à l’utilisateur.

Lorsque vous appelez le modèle pour la prédiction ligne par ligne, vous transmettez un ensemble de valeurs qui représentent des caractéristiques pour chaque cas individuel. La procédure stockée renvoie ensuite une probabilité et une prédiction uniques. 

La procédure stockée *PredictTipSingleMode* illustre cette approche. Elle prend comme entrées plusieurs paramètres représentant des valeurs de caractéristiques (par exemple, le nombre de passagers et la distance du trajet), note ces caractéristiques à l’aide du modèle R stocké et génère la probabilité d’obtenir un pourboire.

1. Exécutez l’instruction Transact-SQL suivante pour créer la procédure stockée.

    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipSingleMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipSingleMode] @passenger_count int = 0,
    @trip_distance float = 0,
    @trip_time_in_secs int = 0,
    @pickup_latitude float = 0,
    @pickup_longitude float = 0,
    @dropoff_latitude float = 0,
    @dropoff_longitude float = 0
    AS
    BEGIN
      DECLARE @inquery nvarchar(max) = N'
        SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs, @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)'
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model FROM nyc_taxi_models);

      EXEC sp_execute_external_script @language = N'R',  @script = N'
            mod <- unserialize(as.raw(model));
            print(summary(mod))
            OutputDataSet<-rxPredict(
              modelObject = mod,
              data = InputDataSet,
              outData = NULL,
              predVarNames = "Score",
              type = "response",
              writeModelVars = FALSE,
              overwrite = TRUE);
            str(OutputDataSet)
            print(OutputDataSet)
            ',
      @input_data_1 = @inquery,
      @params = N'
      -- passthrough columns
      @model varbinary(max) ,
      @passenger_count int ,
      @trip_distance float ,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      -- mapped variables
      @model = @lmodel2 ,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance ,
      @trip_time_in_secs=@trip_time_in_secs ,
      @pickup_latitude=@pickup_latitude ,
      @pickup_longitude=@pickup_longitude ,
      @dropoff_latitude=@dropoff_latitude ,
      @dropoff_longitude=@dropoff_longitude
      WITH RESULT SETS ((Score float));
    END
    ```

2. Dans SQL Server Management Studio, vous pouvez utiliser la procédure **EXEC** [!INCLUDE[tsql](../../includes/tsql-md.md)] (ou **EXECUTE**) pour appeler la procédure stockée et la transmettre aux entrées requises. Par exemple, essayez d’exécuter cette instruction dans Management Studio :

    ```sql
    EXEC [dbo].[PredictTipSingleMode] 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

    Les valeurs transmises ici correspondent, respectivement, aux variables _passenger\_count_, _trip_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ et _dropoff\_longitude_.

3. Pour exécuter ce même appel à partir du code R, il vous suffit de définir une variable R qui contient l’appel de procédure stockée complet, comme ce qui suit :

    ```R
    q2 = "EXEC PredictTipSingleMode 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303 ";
    ```

    Les valeurs transmises ici correspondent, respectivement, aux variables _passenger\_count_, _trip\_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ et _dropoff\_longitude_.

4. Appelez `sqlQuery` (à partir du package **RODBC**), puis transmettez la chaîne de connexion et la variable de type chaîne contenant l’appel de procédure stockée.

    ```R
    # predict with stored procedure in single mode
    sqlQuery (conn, q2);
    ```

    >[!TIP]
    > Outils R pour Visual Studio (RTVS) offre une intégration parfaite avec SQL Server et R. Consultez cet article pour obtenir plus d’exemples d’utilisation de RODBC avec une connexion SQL Server : [Utiliser SQL Server et R](/visualstudio/rtvs/sql-server)

## <a name="next-steps"></a>Étapes suivantes

Après avoir appris à utiliser des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à rendre persistants des modèles R formés pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devriez pouvoir assez facilement créer des modèles basés sur ce jeu de données. Par exemple, vous pouvez essayer de créer des modèles supplémentaires comme ceux-ci :

+ Un modèle de régression qui prédit le montant du pourboire
+ Un modèle de classification multiclasse qui prédit si le pourboire est petit, moyen ou élevé

Vous pouvez également explorer ces exemples et ressources supplémentaires :

+ [Scénarios de science des données et modèles de solutions](data-science-scenarios-and-solution-templates.md)
+ [Analytique avancée en base de données](r-taxi-classification-introduction.md)
+ [How-to guides for data analysis and operationalization](/machine-learning-server/r/how-to-introduction) (Guides pratiques pour l’opérationnalisation et l’analyse des données)
+ [Additional Resources for Machine Learning Server and Microsoft R](/machine-learning-server/resources-more) (Ressources supplémentaires pour Machine Learning Server et Microsoft R)
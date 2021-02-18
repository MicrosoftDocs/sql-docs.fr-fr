---
title: 'Didacticiel R : Explorer des données'
description: Didacticiel illustrant comment visualiser et générer des synthèses statistiques à l’aide de fonctions R pour l’analyse dans la base de données sur SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 676fd125bf148a5139de1ecee7145c523ee6b4e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272940"
---
# <a name="view-and-summarize-sql-server-data-using-r-walkthrough"></a>Afficher et synthétiser des données SQL Server à l’aide de R (procédure pas à pas)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Cette leçon vous présente les fonctions du package **RevoScaleR** et vous guide pour effectuer les tâches suivantes :

> [!div class="checklist"]
> * Se connecter à SQL Server
> * Définir une requête qui contient les données dont vous avez besoin, ou spécifier une table ou une vue
> * Définir un ou plusieurs contextes de calcul à utiliser lors de l’exécution du code R
> * Définir éventuellement les transformations appliquées à la source de données quand celle-ci est lue

## <a name="define-a-sql-server-compute-context"></a>Définir un contexte de calcul SQL Server

Exécutez les instructions R suivantes dans un environnement R sur la station de travail cliente. Cette section part du principe qu’une [station de travail de science des données avec Microsoft R Client](../r/set-up-a-data-science-client.md) est utilisée, car elle comprend tous les packages RevoScaleR, ainsi qu’un ensemble basique et léger d’outils R. Par exemple, vous pouvez utiliser Rgui.exe pour exécuter le script R dans cette section.

1. Si le package **RevoScaleR** n’est pas encore chargé, exécutez cette ligne de code R :

    ```R
    library("RevoScaleR")
    ```

     Les guillemets sont ici facultatifs, mais recommandés.
     
     Si vous obtenez une erreur, vérifiez que votre environnement de développement R utilise une bibliothèque qui inclut le package RevoScaleR. Utilisez une commande telle que `.libPaths()` pour afficher le chemin actuel d’accès à la bibliothèque.

2. Créez la chaîne de connexion pour SQL Server, puis enregistrez-la dans une variable R, *connStr*.

   Vous devez remplacer l’espace réservé « your_server_name » par un nom d’instance SQL Server valide. Pour le nom du serveur, vous pouvez utiliser uniquement le nom de l’instance, ou vous devrez peut-être qualifier complètement le nom, en fonction de votre réseau.
    
   Pour l’authentification SQL Server, la syntaxe de la connexion est la suivante :

    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Uid=your-sql-login;Pwd=your-login-password"
    ```

    Pour l’authentification Windows, la syntaxe est un peu différente :
    
    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Trusted_Connection=True"
    ```

    Généralement, nous vous recommandons d’utiliser l’authentification Windows pour ne pas enregistrer de mots de passe dans votre code R.

3. Définissez les variables à utiliser en créant un nouveau *contexte de calcul*. Après avoir créé l’objet de contexte de calcul, vous pouvez l’utiliser pour exécuter le code R sur l’instance SQL Server.

    ```R
    sqlShareDir <- paste("C:\\AllShare\\",Sys.getenv("USERNAME"),sep="")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```

    - R utilise un répertoire temporaire pour la sérialisation bidirectionnelle des objets R entre votre station de travail et l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez spécifier le répertoire local qui est utilisé en tant que *sqlShareDir* ou accepter la valeur par défaut.
  
    - Utilisez *sqlWait* pour indiquer si vous souhaitez que R attende les résultats du serveur.  Pour plus d’informations sur l’attente ou non des travaux, consultez [Distributed and parallel computing in Machine Learning Server](/r-server/r/how-to-revoscaler-distributed-computing) (Calcul distribué et parallèle dans Machine Learning Server).
  
    - Utilisez l’argument *sqlConsoleOutput* pour indiquer que vous ne souhaitez pas voir la sortie de la console R.

4. Vous appelez le constructeur [RxInSqlServer](/r-server/r-reference/revoscaler/rxinsqlserver) pour créer l’objet de contexte de calcul avec les variables et les chaînes de connexion déjà définies, puis enregistrez le nouvel objet dans la variable R *sqlcc*.
  
    ```R
    sqlcc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```

5. Le contexte de calcul est local par défaut. Vous devez donc définir explicitement le contexte de calcul *actif*.

    ```R
    rxSetComputeContext(sqlcc)
    ```

    + [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) retourne sans le montrer le contexte de calcul précédemment actif afin que vous puissiez l’utiliser
    + [rxGetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) retourne le contexte de calcul actif
    
    Notez que le contexte de calcul spécifié affecte uniquement les opérations qui utilisent les fonctions disponibles dans le package **RevoScaleR** ; il n’affecte pas la façon dont les opérations R open source sont effectuées.

## <a name="create-a-data-source-using-rxsqlserver"></a>Créer une source de données à l’aide de RxSqlServer

Lorsque vous utilisez les bibliothèques Microsoft R telles que RevoScaleR et MicrosoftML, une *source de données* est un objet que vous créez à l’aide de fonctions RevoScaleR. L’objet de source de données spécifie un jeu de données à utiliser pour une tâche, telle que la formation de modèle ou l’extraction de fonctionnalité. Vous pouvez obtenir des données à partir de diverses sources, notamment SQL Server. Pour obtenir la liste des sources actuellement prises en charge, consultez [RxDataSource](/r-server/r-reference/revoscaler/rxdatasource).

Précédemment, vous avez défini une chaîne de connexion et enregistré ces informations dans une variable R. Vous pouvez réutiliser ces informations de connexion pour spécifier les données que vous souhaitez obtenir.

1. Enregistrez une requête SQL en tant que variable de chaîne. La requête définit les données pour la formation du modèle.

    ```R
    sampleDataQuery <- "SELECT TOP 1000 tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"
    ```

    Nous avons utilisé ici une clause TOP pour accélérer l’exécution des opérations, mais les lignes réelles retournées par la requête peuvent varier en fonction de l’ordre. Par conséquent, les résultats de votre synthèse peuvent être différents de ceux répertoriés ci-dessous. N’hésitez pas à supprimer la clause TOP.

2. Passez la définition de la requête comme argument à la fonction [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata).

    ```R
    inDataSource <- RxSqlServerData(
      sqlQuery = sampleDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )
    ```
    
    + L’argument  *colClasses* spécifie les types de colonne à utiliser pour le déplacement des données entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et R. Cela est important, car par rapport à R, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise des types de données différents et plus nombreux. Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).
  
    + L’argument *rowsPerRead* est important pour gérer l’utilisation de la mémoire et optimiser l’efficacité des calculs.  La plupart des fonctions analytiques améliorées dans[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] traitent les données en segments et accumulent les résultats intermédiaires. Elles retournent les calculs finaux une fois que toutes les données ont été lues.  En ajoutant le paramètre *rowsPerRead*, vous pouvez contrôler le nombre de lignes de données lues dans chaque segment pour le traitement.  Si la valeur de ce paramètre est trop grande, l’accès aux données peut être lent, car vous n’avez pas suffisamment de mémoire pour traiter efficacement un si grand segment de données.  Sur certains systèmes, si *rowsPerRead* a une valeur trop petite, les performances peuvent également être ralenties.

3. À ce stade, vous avez créé l’objet *inDataSource*, mais il ne contient pas de données. Les données ne sont extraites de la requête SQL dans l’environnement local qu’au moment où vous exécutez une fonction telle que [rxImport](/r-server/r-reference/revoscaler/rxdatastep) ou [rxSummary](/r-server/r-reference/revoscaler/rxsummary).

    Toutefois, maintenant que vous avez défini les objets de données, vous pouvez les utiliser comme arguments pour d’autres fonctions.

## <a name="use-the-sql-server-data-in-r-summaries"></a>Utiliser les données SQL Server dans des synthèses R

Dans cette section, vous allez tester plusieurs fonctions fournies dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] qui prennent en charge les contextes de calcul à distance. En appliquant des fonctions R à la source de données, vous pouvez explorer, synthétiser et représenter sous forme graphique les données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

1. Appelez la fonction [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) pour obtenir la liste des variables dans la source de données et leurs types de données.

    **rxGetVarInfo** est une fonction utile que vous pouvez appeler pour toute trame de données, ou un jeu de données dans un objet de données distant, afin d’obtenir des informations telles que les valeurs minimale et maximale, le type de données et le nombre de niveaux dans les colonnes de facteurs.
    
    Envisagez d’exécuter cette fonction après tout type d’entrée de données, de transformation de caractéristique ou d’ingénierie de caractéristiques. Ainsi, toutes les fonctionnalités que vous souhaitez utiliser dans votre modèle correspondent au type de données attendu et vous évitez les erreurs.
  
    ```R
    rxGetVarInfo(data = inDataSource)
    ```

    **Résultats**
    
    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: integer
    Var 4: trip_time_in_secs, Type: numeric, Storage: int64
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: pickup_longitude, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: dropoff_longitude, Type: numeric
    ```

2. Appelez à présent la fonction RevoScaleR [rxSummary](/r-server/r-reference/revoscaler/rxsummary) pour obtenir des statistiques plus détaillées sur des variables spécifiques.

    rxSummary repose sur la fonction `summary` R, mais offre des fonctionnalités et des avantages supplémentaires. rxSummary fonctionne dans plusieurs contextes de calcul et prend en charge la segmentation.  Vous pouvez également utiliser rxSummary pour transformer des valeurs ou créer des synthèses en fonction de niveaux de facteur.
    
    Dans cet exemple, vous synthétiser le prix selon le nombre de passagers.
    
    ```R
    start.time <- proc.time()
    rxSummary(~fare_amount:F(passenger_count,1,6), data = inDataSource)
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
      Elapsed Time=", round(used.time[3],2),
      " seconds to summarize the inDataSource.", sep=""))
    ```
    + Le premier argument de rxSummary spécifie la formule ou le terme à utiliser pour créer la synthèse. Ici, la fonction `F()` est utilisée pour convertir les valeurs de _passenger\_count_ en facteurs avant de créer la synthèse. Vous devez également spécifier la valeur minimale (1) et la valeur maximale (6) pour la variable de facteur _passenger\_count_.
    + Si vous ne spécifiez pas les statistiques à générer, par défaut, rxSummary génère Mean, StDev, Min, Max et le nombre d’observations valides et manquantes.
    + Cet exemple inclut également du code pour effectuer le suivi de l’heure à laquelle la fonction démarre et se termine, afin que vous puissiez comparer les performances.
  
    **Résultats**

    Si la fonction rxSummary s’exécute correctement, vous devez voir des résultats identiques à ceux présentés ici, suivis d’une liste de statistiques par catégorie. 

    ```R
    rxSummary(formula = ~fare_amount:F(passenger_count, 1,6), data = inDataSource)
    Data: inDataSource (RxSqlServerData Data Source)
    Number of valid observations: 1000
    ```

### <a name="bonus-exercise-on-big-data"></a>Exercice bonus sur le Big Data

Essayez de définir une nouvelle chaîne de requête avec toutes les lignes. Nous vous recommandons de configurer un nouvel objet de source de données pour cette expérience. Vous pouvez également essayer de modifier le paramètre *rowsToRead* pour voir comment il affecte le débit.

```R
bigDataQuery  <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"

bigDataSource <- RxSqlServerData(
      sqlQuery = bigDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )

start.time <- proc.time()
rxSummary(~fare_amount:F(passenger_count,1,6), data = bigDataSource)
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
  Elapsed Time=", round(used.time[3],2),
  " seconds to summarize the inDataSource.", sep=""))
```

> [!TIP]
> Pendant l’exécution de cette opération, vous pouvez utiliser un outil tel que [Process Explorer](/sysinternals/downloads/process-explorer) ou le générateur de profils SQL pour voir comment la connexion est établie et comment le code R est exécuté à l’aide des services SQL Server.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer des graphiques et des tracés à l’aide de R](walkthrough-create-graphs-and-plots-using-r.md)
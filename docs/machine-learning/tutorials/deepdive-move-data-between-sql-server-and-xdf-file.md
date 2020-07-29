---
title: Déplacer des données avec un fichier XDF
description: 'Tutoriel RevoScaleR 13 : Comment déplacer des données avec XDF et le langage R sur SQL Server.'
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 9c7e5ce4cbe995fd677acd406187dfaf264a7461
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85679999"
---
# <a name="move-data-between-sql-server-and-xdf-file-sql-server-and-revoscaler-tutorial"></a>Déplacer des données entre SQL Server et un fichier XDF (tutoriel SQL Server et RevoScaleR)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Il s’agit du tutoriel 13 de la [série de tutoriels RevoScaleR](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md) qui traite de l’utilisation des [fonctions RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans ce tutoriel, vous allez apprendre à utiliser un fichier XDF pour transférer des données entre des contextes de calcul locaux et distants. Le stockage des données dans un fichier XDF vous permet d’effectuer des transformations sur les données.

Quand vous aurez terminé, vous utiliserez les données du fichier pour créer une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La fonction [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) peut appliquer des transformations aux données et effectue la conversion entre les trames de données et les fichiers .xdf.
  
## <a name="create-a-sql-server-table-from-an-xdf-file"></a>Créer une table SQL Server à partir d’un fichier XDF

Pour cet exercice, vous allez réutiliser les données de fraude à la carte de crédit. Dans ce scénario, vous avez été invité à effectuer des analyses supplémentaires sur les utilisateurs dans les États de la Californie, de l’Oregon et de Washington. Pour être plus efficace, vous avez décidé de stocker les données de ces seuls États sur votre ordinateur local et vous utilisez uniquement les variables gender, cardholder, state et balance (sexe, détenteur de la carte, état et solde).

1. Réutilisez la variable `stateAbb` que vous avez créé pour identifier les niveaux à inclure, puis écrivez-les dans une nouvelle variable, `statesToKeep`.
  
    ```R
    statesToKeep <- sapply(c("CA", "OR", "WA"), grep, stateAbb)
    statesToKeep
    ```
    **Résultats**
    
    CA|OR|WA
    ----|----|----
    5|38|48
    
2. Définissez les données que vous souhaitez déplacer à partir de SQL Server, à l’aide d’une requête [!INCLUDE[tsql](../../includes/tsql-md.md)].  Plus tard, vous utiliserez cette variable en tant qu’argument *inData* pour **rxImport**.
  
    ```R
    importQuery <- paste("SELECT gender,cardholder,balance,state FROM",  sqlFraudTable,  "WHERE (state = 5 OR state = 38 OR state = 48)")
    ```
  
    Vérifiez que la requête ne contient aucun caractère masqué, tel qu’un saut de ligne ou une tabulation.
  
3. Ensuite, définissez les colonnes à utiliser lors de l’utilisation des données dans le langage R. Par exemple, dans le jeu de données le plus petit, vous n’avez besoin que de trois niveaux de facteur, car la requête retourne les données de seulement trois états.  Appliquez la variable `statesToKeep` pour identifier les niveaux corrects à inclure.
  
    ```R
    importColInfo <- list(
        gender = list( type = "factor",  levels = c("1", "2"), newLevels = c("Male", "Female")),
        cardholder = list(  type = "factor",  levels = c("1", "2"), newLevels = c("Principal", "Secondary")),
        state = list(   type = "factor",  levels = as.character(statesToKeep), newLevels = names(statesToKeep))
            )
    ```
  
4. Définissez le contexte de calcul sur **local**, car vous souhaitez toutes les données disponibles sur votre ordinateur local.
  
    ```R
    rxSetComputeContext("local")
    ```
    
    La fonction [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata) permet d’importer des données à partir d’une source de données prise en charge dans un fichier XDF local. Utiliser une copie locale des données peut être pratique lorsque vous souhaitez effectuer de nombreuses analyses différentes sur les données sans avoir à exécuter la même requête à maintes reprises.

5. Créez l’objet de source de données en passant les variables que vous avez définies en tant qu’arguments à **RxSqlServerData**.
  
    ```R
    sqlServerImportDS <- RxSqlServerData(
        connectionString = sqlConnString,
        sqlQuery = importQuery,
        colInfo = importColInfo)
    ```
  
6. Appelez **rxImport** pour écrire les données dans un fichier nommé `ccFraudSub.xdf`, situé dans le répertoire de travail actuel.
  
    ```R
    localDS <- rxImport(inData = sqlServerImportDS,
        outFile = "ccFraudSub.xdf",
        overwrite = TRUE)
    ```
  
    L’objet `localDs` retourné par la fonction **rxImport** est un objet de source de données **RxXdfData** léger qui représente le fichier de données `ccFraud.xdf`stocké localement sur le disque.
  
7. Appelez [rxGetVarInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) dans le fichier XDF pour vérifier que le schéma de données est le même.
  
    ```R
    rxGetVarInfo(data = localDS)
    ```

    **Résultats**
    
    ```R
    rxGetVarInfo(data = localDS)
    Var 1: gender, Type: factor, no factor levels available
    Var 2: cardholder, Type: factor, no factor levels available
    Var 3: balance, Type: integer, Low/High: (0, 22463)
    Var 4: state, Type: factor, no factor levels available
    ```

8. Vous pouvez maintenant appeler différentes fonctions R pour analyser l’objet **localDs** , comme vous le feriez avec les données sources sur SQL Server. Par exemple, vous pouvez résumer par sexe :
  
    ```R
    rxSummary(~gender + cardholder + balance + state, data = localDS)
    ```

## <a name="next-steps"></a>Étapes suivantes

Ce tutoriel conclut la série de tutoriels en plusieurs parties sur **RevoScaleR** et SQL Server. Elle vous a présenté de nombreux concepts liés aux données et au calcul, ce qui vous donne une base pour appliquer vos propres exigences en matière de données et de projet.

Pour approfondir vos connaissances sur **RevoScaleR**, vous pouvez revenir à la liste des tutoriels R pour parcourir les exercices que vous avez pu manquer. Vous pouvez également consulter les articles de procédure de la table des matières pour en savoir plus sur les tâches générales.

> [!div class="nextstepaction"]
> [Didacticiels R pour SQL Server](sql-server-r-tutorials.md)
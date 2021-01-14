---
title: 'Démarrage rapide : Fonctions R'
titleSuffix: SQL machine learning
description: Dans ce guide de démarrage rapide, vous apprendrez à utiliser les fonctions mathématiques et utilitaires de R avec l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0da73dd1202d4b5eab2c14077dfe54b04dfe9bd8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092664"
---
# <a name="quickstart-r-functions-with-sql-machine-learning"></a>Démarrage rapide : Fonctions R avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans ce guide de démarrage rapide, vous apprendrez à utiliser les fonctions mathématiques et utilitaires de R avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md). Les fonctions statistiques sont souvent complexes à implémenter dans T-SQL, mais elles peuvent l’être en R avec seulement quelques lignes de code.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans ce guide de démarrage rapide, vous apprendrez à utiliser les fonctions mathématiques et utilitaires de R avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Les fonctions statistiques sont souvent complexes à implémenter dans T-SQL, mais elles peuvent l’être en R avec seulement quelques lignes de code.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans ce guide de démarrage rapide, vous apprendrez à utiliser les fonctions mathématiques et utilitaires de R avec [SQL Server R Services](../r/sql-server-r-services.md). Les fonctions statistiques sont souvent complexes à implémenter dans T-SQL, mais elles peuvent l’être en R avec seulement quelques lignes de code.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans ce démarrage rapide, vous allez apprendre à utiliser des structures de données et des types de données en R dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Vous découvrirez comment déplacer des données entre R et SQL Managed Instance, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end

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

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Créer une procédure stockée pour générer des nombres aléatoires

Pour plus de simplicité, nous allons utiliser le package R `stats`, qui est installé et chargé par défaut. Le package contient des centaines de fonctions qui permettent d’effectuer des tâches statistiques courantes, notamment la fonction `rnorm`, qui génère un nombre spécifié de nombres aléatoires en utilisant la distribution normale, avec un écart type et une moyenne donnés.

Par exemple, le code R suivant retourne 100 nombres sur une moyenne de 50, compte tenu d’un écart type de 3.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Pour appeler cette ligne de code R à partir de T-SQL, ajoutez la fonction R dans le paramètre de script R de `sp_execute_external_script`, comme ceci :

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

Que se passe-t-il si vous voulez simplifier la génération d’un autre ensemble de nombres alétaoires ?

Rien de plus facile avec une combinaison avec T-SQL. Vous définissez une procédure stockée qui obtient les arguments de l’utilisateur, puis transmettez ces arguments dans le script R en tant que variables.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- La première ligne définit chacun des paramètres d’entrée SQL exigés quand la procédure stockée est exécutée.

- La ligne commençant par `@params` définit toutes les variables utilisées par le code R et les types de données SQL correspondants.

- Les lignes qui suivent immédiatement mappent les noms de paramètres SQL aux noms de variables R correspondants.

Maintenant que vous avez wrappé la fonction R dans une procédure stockée, vous pouvez facilement l’appeler et passer des valeurs différentes, comme ceci :

```sql
EXECUTE MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Utiliser des fonctions utilitaires R à des fins de résolution des problèmes

Le package **utils**, installé par défaut propose diverses fonctions utilitaires destinées à analyser l’environnement R actuel. Ces fonctions peuvent être utiles si vous constatez des différences de fonctionnement du code R dans SQL Server et les environnements extérieurs.

Par exemple, vous pouvez utiliser les fonctions de minutage système en R, comme `system.time` et `proc.time`, pour capturer l’heure utilisée par les processus R et analyser les problèmes de performances. Pour obtenir un exemple, consultez le didacticiel [Créer des fonctionnalités de données à l’aide de R et SQL Server (procédure pas à pas)](../tutorials/walkthrough-create-data-features.md) où les fonctions de minutage R sont incorporées à la solution.

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        start.time <- proc.time();
        
        # Run R processes
        
        elapsed_time <- proc.time() - start.time;'
```

Pour connaître d’autres fonctions utiles, consultez [Amélioration du niveau de performance avec les fonctions de profilage du code R](../r/using-r-code-profiling-functions.md).

## <a name="next-steps"></a>Étapes suivantes

Pour créer un modèle Machine Learning à l’aide de R avec l’apprentissage automatique SQL, suivez ce démarrage rapide :

> [!div class="nextstepaction"]
> [Créer et scorer un modèle prédictif dans R avec l’apprentissage automatique SQL](quickstart-r-train-score-model.md)
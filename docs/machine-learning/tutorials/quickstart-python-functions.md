---
title: 'Démarrage rapide : Fonctions Python'
titleSuffix: SQL machine learning
description: Dans ce guide de démarrage rapide, vous allez apprendre à utiliser les fonctions mathématiques et utilitaires de Python avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 646c1decb34adabe645a58d640b0f9adbb4768b7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101906"
---
# <a name="quickstart-python-functions-with-sql-machine-learning"></a>Démarrage rapide : Fonctions Python avec le Machine Learning SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Dans ce guide de démarrage rapide, vous allez découvrir comment utiliser les fonctions mathématiques et utilitaires Python avec [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Machine Learning Services d’Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) ou des [clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md). Les fonctions statistiques sont souvent complexes à implémenter dans T-SQL, mais elles peuvent l’être en Python avec seulement quelques lignes de code.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce démarrage rapide, vous avez besoin de ce qui suit.

- Une base de données SQL sur l’une de ces plateformes :
  - [Machine Learning Services SQL Server](../sql-server-machine-learning-services.md). Pour l’installer, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Clusters Big Data SQL Server. Voir comment [Activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Un outil permettant d’exécuter des requêtes SQL qui contiennent des scripts Python. Ce guide de démarrage rapide utilise [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Créer une procédure stockée pour générer des nombres aléatoires

Dans un souci de simplicité, nous allons utiliser le package Python `numpy`, qui est installé et chargé par défaut. Le package contient des centaines de fonctions qui permettent d’effectuer des tâches statistiques courantes, notamment la fonction `random.normal`, qui génère un nombre spécifié de nombres aléatoires en utilisant la distribution normale, avec un écart type et une moyenne donnés.

Par exemple, le code Python suivant retourne 100 nombres sur une moyenne de 50, compte tenu d’un écart type de 3.

```Python
numpy.random.normal(size=100, loc=50, scale=3)
```

Pour appeler cette ligne de Python à partir de T-SQL, ajoutez la fonction Python dans le paramètre de script Python de `sp_execute_external_script`. La sortie attend une trame de données, alors utilisez `pandas` pour la convertir.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=100, loc=50, scale=3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Que se passe-t-il si vous voulez simplifier la génération d’un autre ensemble de nombres alétaoires ? Vous définissez une procédure stockée qui obtient les arguments de l’utilisateur, puis transmettez ces arguments dans le script Python en tant que variables.

```sql
CREATE PROCEDURE MyPyNorm (
      @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=mynumbers, loc=mymean, scale=mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- La première ligne définit chacun des paramètres d’entrée SQL exigés quand la procédure stockée est exécutée.

- La ligne commençant par `@params` définit toutes les variables utilisées par le code Python, ainsi que les types de données SQL correspondants.

- Les lignes qui viennent juste après mappent les noms de paramètres SQL aux noms de variables Python correspondants.

Maintenant que vous avez inclus la fonction Python dans une procédure stockée, vous pouvez l’appeler facilement et passer des valeurs différentes, comme ceci :

```sql
EXECUTE MyPyNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-python-utility-functions-for-troubleshooting"></a>Utiliser les fonctions utilitaires Python pour résoudre les problèmes

Les packages Python fournissent diverses fonctions utilitaires pour l’examen de l’environnement Python actuel. Ces fonctions peuvent être utiles si vous constatez des différences de fonctionnement du code Python dans SQL Server et les environnements extérieurs.

Par exemple, vous pouvez utiliser les fonctions de minutage du système dans le package `time` pour mesurer la durée des processus Python et de l’analyse des problèmes de performances.

```sql
EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
import time
start_time = time.time()

# Run Python processes

elapsed_time = time.time() - start_time
'
    , @input_data_1 = N' ;';
```

## <a name="next-steps"></a>Étapes suivantes

Pour créer un modèle Machine Learning en Python avec le Machine Learning SQL, suivez ce démarrage rapide :

> [!div class="nextstepaction"]
> [Démarrage rapide : Création et scoring d’un modèle prédictif en Python](quickstart-python-train-score-model.md)
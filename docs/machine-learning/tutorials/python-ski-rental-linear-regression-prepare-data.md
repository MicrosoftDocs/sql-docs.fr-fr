---
title: 'Tutoriel Python : Préparer les données'
titleSuffix: SQL machine learning
description: Dans la deuxième partie de cette série de quatre tutoriels, vous utiliserez Python pour préparer des données afin de prédire les locations de skis avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1cce65d71e686876370b6a978d660d0f75a1d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351471"
---
# <a name="python-tutorial-prepare-data-to-train-a-linear-regression-model-with-sql-machine-learning"></a>Tutoriel Python : Préparer des données pour effectuer l’apprentissage d’un modèle de régression linéaire avec le Machine Learning SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer des données à partir d’une base de données à l’aide de Python. Plus loin dans cette série, vous utiliserez ces données pour effectuer l’apprentissage et le déploiement d’un modèle de régression linéaire dans Python avec SQL Server Machine Learning Services ou sur des clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer des données à partir d’une base de données à l’aide de Python. Plus loin dans cette série, vous utiliserez ces données pour effectuer l’apprentissage et le déploiement d’un modèle de régression linéaire dans Python avec SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer des données à partir d’une base de données à l’aide de Python. Dans la suite de cette série, vous utiliserez ces données pour entraîner et déployer un modèle de régression linéaire en Python avec Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Charger les données à partir de la base de données dans une trame de données **pandas**
> * Préparer les données dans Python en supprimant des colonnes

Dans la [première partie](python-ski-rental-linear-regression.md), vous avez appris à restaurer l’exemple de base de données.

Dans la [troisième partie](python-ski-rental-linear-regression-train-model.md), vous apprendrez à effectuer l’apprentissage d’un modèle de Machine Learning de régression linéaire dans Python.

Dans la [quatrième partie](python-ski-rental-linear-regression-deploy-model.md), vous allez apprendre à stocker le modèle dans une base de données, puis à créer des procédures stockées à partir des scripts Python que vous avez développés dans les parties 2 et 3. Les procédures stockées sont exécutées sur le serveur pour effectuer des prédictions en fonction de nouvelles données.

## <a name="prerequisites"></a>Prérequis

* La deuxième partie de ce tutoriel suppose que vous avez terminé la [première partie](python-ski-rental-linear-regression.md) et ses conditions préalables.

## <a name="explore-and-prepare-the-data"></a>Explorer et préparer les données

Pour utiliser les données dans Python, vous allez charger les données à partir de la base de données dans une trame de données pandas.

Créez un nouveau notebook Python dans Azure Data Studio et exécutez le script ci-dessous. 

Le script Python ci-dessous importe le jeu de données à partir de la table **dbo. rental_data** de votre base de données vers une trame de données pandas **df**.

Dans la chaîne de connexion, remplacez les détails de connexion, le cas échéant.

```python
import pyodbc
import pandas
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Connection string to your SQL Server instance
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=TutorialDB;UID=<username>;PWD=<password>)

query_str = 'SELECT Year, Month, Day, Rentalcount, Weekday, Holiday, Snow FROM dbo.rental_data'

df = pandas.read_sql(sql=query_str, con=conn_str)

print("Data frame:", df)

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Filter the columns to remove ones we don't want to use in the training
columns = [c for c in columns if c not in ["Year"]]
```

Vous devez obtenir des résultats similaires à ce qui suit :

```results
Data frame:      Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
0    2014      1   20          445        2        1     0
1    2014      2   13           40        5        0     0
2    2013      3   10          456        1        0     0
3    2014      3   31           38        2        0     0
4    2014      4   24           23        5        0     0
..    ...    ...  ...          ...      ...      ...   ...
448  2013      2   19           57        3        0     1
449  2015      3   18           26        4        0     0
450  2015      3   24           29        3        0     1
451  2014      3   26           50        4        0     1
452  2015     12    6          377        1        0     1

[453 rows x 7 columns]
```

## <a name="next-steps"></a>Étapes suivantes

Dans la deuxième partie de cette série de tutoriels, vous avez effectué les étapes suivantes :

* Charger les données à partir de la base de données dans une trame de données **pandas**
* Préparer les données dans Python en supprimant des colonnes

Pour effectuer l’apprentissage d’un modèle de Machine Learning qui utilise des données de la base de données TutorialDB, suivez la troisième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel Python : Effectuer l’apprentissage d’un modèle de régression linéaire](python-ski-rental-linear-regression-train-model.md)
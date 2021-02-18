---
title: 'Tutoriel : Préparer les données pour entraîner un modèle prédictif en R'
titleSuffix: SQL machine learning
description: Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données pour effectuer l’apprentissage d’un modèle prédictif dans R avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e1929f0db9fb1d70fc8c370297d2d6f1c4e74513
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348084"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutoriel : Préparer les données pour effectuer l’apprentissage d’un modèle prédictif dans R avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données à partir d’une base de données à l’aide de R. Plus tard dans cette série, vous allez utiliser ces données pour effectuer l’apprentissage d’un modèle prédictif et le déployer dans R avec SQL Server Machine Learning Services ou sur des clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données à partir d’une base de données à l’aide de R. Plus tard dans cette série, vous allez utiliser ces données pour effectuer l’apprentissage d’un modèle prédictif et le déployer dans R avec SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données à partir d’une base de données à l’aide de R. Plus tard dans cette série, vous allez utiliser ces données pour effectuer l’apprentissage d’un modèle prédictif et le déployer dans R avec SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données en R. Dans la suite de cette série, vous utiliserez ces données pour entraîner et déployer un modèle prédictif en R avec Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Restaurer un exemple de base de données dans une base de données
> * Charger les données de la base de données dans une trame de données R
> * Préparation des données en R en identifiant certaines colonnes comme catégoriques

Dans la [première partie](r-predictive-model-introduction.md), vous avez appris à restaurer l’exemple de base de données.

Dans la [troisième partie](r-predictive-model-train.md), vous apprendrez à effectuer l’apprentissage d’un modèle Machine Learning dans R.

Dans la [quatrième partie](r-predictive-model-deploy.md), vous apprendrez à stocker le modèle dans une base de données, puis à créer des procédures stockées à partir des scripts R que vous avez développés dans les parties 2 et 3. Les procédures stockées sont exécutées sur le serveur pour effectuer des prédictions en fonction de nouvelles données.

## <a name="prerequisites"></a>Prérequis

La deuxième partie de ce didacticiel part du principe que vous avez suivi la [**première partie**](r-predictive-model-introduction.md) et ses conditions préalables.

## <a name="load-the-data-into-a-data-frame"></a>Charger les données dans une trame de données

Pour utiliser les données en R, vous devez charger les données à partir de la base de données dans une trame de données (`rentaldata`).

Créez un fichier RScript dans RStudio et exécutez le script suivant. Remplacez **ServerName** par vos propres informations de connexion.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- "Driver=SQL Server;Server=ServerName;Database=TutorialDB;uid=Username;pwd=Password"


#Get the data from the table
library(RODBC)

ch <- odbcDriverConnect(connStr)

#Import the data from the table
rentaldata <- sqlFetch(ch, "dbo.rental_data")

#Take a look at the structure of the data and the top rows
head(rentaldata)
str(rentaldata)
```

Vous devez obtenir des résultats similaires à ce qui suit :

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Préparer les données

Dans cet exemple de base de données, la majeure partie de la préparation a déjà été effectuée, mais vous allez réaliser une dernière préparation ici.
Utilisez le script R suivant pour identifier trois colonnes en tant que *catégories*, en changeant les types de données pour *factor*.



```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);



#Visualize the dataset after the change
str(rentaldata);
```

Vous devez obtenir des résultats similaires à ce qui suit :

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Les données sont désormais prêtes pour l’entraînement.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne poursuivez pas ce tutoriel, supprimez la base de données TutorialDB.

## <a name="next-steps"></a>Étapes suivantes

Dans la deuxième partie de cette série de tutoriels, vous avez appris à effectuer les tâches suivantes :

* Chargement des exemples de données dans une trame de données R
* Préparation des données en R en identifiant certaines colonnes comme catégoriques

Pour créer un modèle Machine Learning qui utilise des données de la base de données TutorialDB, suivez la troisième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Créer un modèle prédictif dans R avec le Machine Learning SQL](r-predictive-model-train.md)

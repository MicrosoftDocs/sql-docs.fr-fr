---
title: 'Tutoriel : Préparer des données pour effectuer un clustering en R'
titleSuffix: SQL machine learning
description: Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données pour effectuer un clustering en R avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 358f34e281c5c5b1a750135117886247c5e504c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348059"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>Tutoriel : Préparer des données pour effectuer le clustering dans R avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données pour effectuer un clustering en R avec SQL Server Machine Learning Services ou sur Clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données pour effectuer un clustering en R avec SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données pour effectuer un clustering en R avec SQL Server 2016 R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la deuxième partie de cette série de quatre tutoriels, vous allez préparer les données d’une base de données pour effectuer un clustering en R avec Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Séparer les clients selon différentes dimensions à l’aide de R
> * Charger les données de la base de données dans une trame de données R

Dans la [première partie](r-clustering-model-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [troisième partie](r-clustering-model-build.md), vous apprendrez à créer et à effectuer l’apprentissage d’un modèle de clustering k-moyennes dans R.

Dans la [quatrième partie](r-clustering-model-deploy.md), vous apprendrez à créer une procédure stockée dans une base de données permettant d’effectuer un clustering en R en fonction de nouvelles données.

## <a name="prerequisites"></a>Prérequis

* La deuxième partie de ce tutoriel suppose que vous avez terminé la [**première partie**](r-clustering-model-introduction.md).

## <a name="separate-customers"></a>Séparer des clients

Créez un fichier RScript dans RStudio et exécutez le script suivant.
Dans la requête SQL, vous séparez les clients selon les dimensions suivantes :

* **orderRatio** = retourne le taux de commandes (nombre total de commandes partiellement ou entièrement retournées par rapport au nombre total de commandes)
* **itemsRatio** = retourne le taux d’éléments (nombre total d’éléments retournés par rapport au nombre d’éléments achetés)
* **monetaryRatio** = retourne le taux des montants (montant monétaire total des éléments retournés par rapport au montant acheté)
* **frequency** = fréquence de retour

Dans la fonction **connStr**, remplacez **ServerName** par vos propres informations de connexion.

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>Charger les données dans une trame de données

Maintenant, utilisez le script suivant pour retourner les résultats de la requête vers une trame de données R.

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

Vous devez obtenir des résultats similaires à ce qui suit :

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne poursuivez pas ce tutoriel, supprimez la base de données tpcxbb_1gb.

## <a name="next-steps"></a>Étapes suivantes

Dans la deuxième partie de cette série de tutoriels, vous avez appris à effectuer les tâches suivantes :

* Séparer les clients selon différentes dimensions à l’aide de R
* Charger les données de la base de données dans une trame de données R

Pour effectuer l’apprentissage d’un modèle de Machine Learning qui utilise ces données client, suivez la troisième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Créer un modèle prédictif dans R avec le Machine Learning SQL](r-clustering-model-build.md)

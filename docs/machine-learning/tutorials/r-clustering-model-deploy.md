---
title: 'Tutoriel : Déployer un modèle de clustering en R'
titleSuffix: SQL machine learning
description: Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering dans R avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1dc019023546a4d1912480bfe452b3067f36bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339619"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutoriel : Déployer un modèle de clustering dans R avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en R, dans une base de données à l’aide de SQL Server Machine Learning Services ou sur Clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en R, dans une base de données à l’aide de SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en R, dans une base de données à l’aide de SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en R, dans une base de données à l’aide d’Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Pour effectuer régulièrement le clustering, à mesure que de nouveaux clients s’inscrivent, vous devez être en mesure d’appeler le script R à partir de n’importe quelle application. Pour ce faire, vous pouvez déployer le script R dans une base de données en le plaçant à l’intérieur d’une procédure stockée SQL. Étant donné que votre modèle s’exécute dans la base de données, son apprentissage peut facilement être effectué en fonction des données stockées dans la base de données.

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Créer une procédure stockée qui génère le modèle
> * Effectuer le clustering
> * Utiliser les informations de clustering

Dans la [première partie](r-clustering-model-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](r-clustering-model-prepare-data.md), vous avez appris à préparer les données d’une base de données pour effectuer le clustering.

Dans la [troisième partie](r-clustering-model-build.md), vous avez appris à créer et à effectuer l’apprentissage d’un modèle de clustering k-moyennes dans R.

## <a name="prerequisites"></a>Prérequis

* La quatrième partie de cette série de tutoriels part du principe que vous avez satisfait les prérequis de la [**première partie**](r-clustering-model-introduction.md) et effectué les étapes décrites dans la [**deuxième partie**](r-clustering-model-build.md) et la [**troisième partie**](r-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Créer une procédure stockée qui génère le modèle

Exécutez le script T-SQL suivant pour créer la procédure stockée. La procédure recrée les étapes que vous avez développées dans les parties 2 et 3 de cette série de tutoriels :

* classer les clients en fonction de leur historique d’achat et de retour
* générer quatre clusters de clients à l’aide d’un algorithme K-Means

La procédure stocke les mappages de clusters clients qui en résultent dans la table de base de données **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>Effectuer le clustering

Maintenant que vous avez créé la procédure stockée, exécutez le script suivant pour effectuer le clustering.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Vérifiez qu’il fonctionne et que vous obtenez bien la liste des clients et leurs mappages de cluster.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Utiliser les informations de clustering

Étant donné que vous avez stocké la procédure de clustering dans la base de données, elle peut effectuer efficacement le clustering sur les données client stockées dans la même base de données. Vous pouvez exécuter la procédure chaque fois que vos données client sont mises à jour et utiliser les informations de clustering mises à jour.

Supposons que vous souhaitiez envoyer un e-mail promotionnel aux clients du cluster 0, groupe qui était inactif (vous pouvez voir comment les quatre clusters ont été décrits dans la [troisième partie](r-clustering-model-build.md#analyze-the-results) de ce tutoriel). Le code suivant sélectionne les adresses de messagerie des clients dans le cluster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Vous pouvez modifier la valeur **c.cluster** pour renvoyer les adresses de messagerie des clients se trouvant dans d’autres clusters.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Une fois ce tutoriel terminé, supprimez la base de données tpcxbb_1gb.

## <a name="next-steps"></a>Étapes suivantes

Dans la quatrième partie de cette série de tutoriels, vous avez appris à effectuer les tâches suivantes :

* Créer une procédure stockée qui génère le modèle
* Effectuer un clustering avec le Machine Learning SQL
* Utiliser les informations de clustering

Pour en savoir plus sur l’utilisation de R dans Machine Learning Services, consultez :

* [Exécuter des scripts R simples](quickstart-r-create-script.md)
* [Structures de données, types et objets R](quickstart-r-data-types-and-objects.md)
* [Fonctions R](quickstart-r-functions.md)

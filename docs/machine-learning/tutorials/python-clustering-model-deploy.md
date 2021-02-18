---
title: 'Tutoriel Python : Déployer un modèle de cluster'
titleSuffix: SQL machine learning
description: Dans la quatrième partie de cette série de tutoriels qui en contient 4, vous allez déployer un modèle de clustering dans Python avec l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 32b009f8155000dfc44f714cfa6f67fc8333813e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349055"
---
# <a name="python-tutorial-deploy-a-model-to-categorize-customers-with-sql-machine-learning"></a>Tutoriel Python : Déployer un modèle pour classer les clients par catégorie avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en Python, dans une base de données à l’aide de SQL Server Machine Learning Services ou sur Clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en Python, dans une base de données à l’aide de SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la quatrième partie de cette série de quatre tutoriels, vous allez déployer un modèle de clustering, développé en Python, dans une base de données à l’aide d’Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Pour effectuer régulièrement le clustering, à mesure que de nouveaux clients s’inscrivent, vous devez être en mesure d’appeler le script Python à partir de n’importe quelle application. Pour ce faire, vous pouvez déployer le script Python dans une base de données en le plaçant à l’intérieur d’une procédure stockée SQL. Étant donné que votre modèle s’exécute dans la base de données, son apprentissage peut facilement être effectué en fonction des données stockées dans la base de données.

Dans cette section, vous allez déplacer le code Python que vous venez d’écrire sur le serveur et déployer le clustering.

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Créer une procédure stockée qui génère le modèle
> * Effectuer le clustering sur le serveur
> * Utiliser les informations de clustering

Dans la [première partie](python-clustering-model.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](python-clustering-model-prepare-data.md), vous avez appris à préparer les données d’une base de données pour effectuer le clustering.

Dans la [troisième partie](python-clustering-model-build.md), vous avez appris à créer et à effectuer l’apprentissage d’un modèle de clustering dans Python.

## <a name="prerequisites"></a>Prérequis

* La troisième partie de ce tutoriel part du principe que vous avez satisfait les prérequis de la [**première partie**](python-clustering-model.md) et effectué les étapes décrites dans la [**deuxième partie**](python-clustering-model-prepare-data.md) et la [**troisième partie**](python-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Créer une procédure stockée qui génère le modèle

Exécutez le script T-SQL suivant pour créer la procédure stockée. La procédure recrée les étapes que vous avez développées dans les parties 1 et 2 de cette série de tutoriels :

* classer les clients en fonction de leur historique d’achat et de retour
* générer quatre clusters de clients à l’aide d’un algorithme K-Means

```sql
USE [tpcxbb_1gb]
GO

CREATE procedure [dbo].[py_generate_customer_return_clusters]
AS

BEGIN
    DECLARE

-- Input query to generate the purchase history & return metrics
     @input_query NVARCHAR(MAX) = N'
SELECT
  ss_customer_sk AS customer,
  CAST( (ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) ) AS FLOAT) AS orderRatio,
  CAST( (ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) ) AS FLOAT) AS itemsRatio,
  CAST( (ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) ) AS FLOAT) AS monetaryRatio,
  CAST( (COALESCE(returns_count, 0)) AS FLOAT) AS frequency
FROM
  (
    SELECT
      ss_customer_sk,
      -- return order ratio
      COUNT(distinct(ss_ticket_number)) AS orders_count,
      -- return ss_item_sk ratio
      COUNT(ss_item_sk) AS orders_items,
      -- return monetary amount ratio
      SUM( ss_net_paid ) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
  ) orders
  LEFT OUTER JOIN
  (
    SELECT
      sr_customer_sk,
      -- return order ratio
      count(distinct(sr_ticket_number)) as returns_count,
      -- return ss_item_sk ratio
      COUNT(sr_item_sk) as returns_items,
      -- return monetary amount ratio
      SUM( sr_return_amt ) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
  ) returned ON ss_customer_sk=sr_customer_sk
 '

EXEC sp_execute_external_script
      @language = N'Python'
    , @script = N'

import pandas as pd
from sklearn.cluster import KMeans

#get data from input query
customer_data = my_input_data

#We concluded in step 2 in the tutorial that 4 would be a good number of clusters
n_clusters = 4

#Perform clustering
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orderRatio","itemsRatio","monetaryRatio","frequency"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
    , @input_data_1 = @input_query
    , @input_data_1_name = N'my_input_data'
             with result sets (("Customer" int, "orderRatio" float,"itemsRatio" float,"monetaryRatio" float,"frequency" float,"cluster" float));
END;
GO
```

## <a name="perform-clustering"></a>Effectuer le clustering

Maintenant que vous avez créé la procédure stockée, exécutez le script suivant pour effectuer le clustering à l’aide de la procédure.

```sql
--Create a table to store the predictions in

DROP TABLE IF EXISTS [dbo].[py_customer_clusters];
GO

CREATE TABLE [dbo].[py_customer_clusters] (
    [Customer] [bigint] NULL
  , [OrderRatio] [float] NULL
  , [itemsRatio] [float] NULL
  , [monetaryRatio] [float] NULL
  , [frequency] [float] NULL
  , [cluster] [int] NULL
  ,
    ) ON [PRIMARY]
GO

--Execute the clustering and insert results into table
INSERT INTO py_customer_clusters
EXEC [dbo].[py_generate_customer_return_clusters];

-- Select contents of the table to verify it works
SELECT * FROM py_customer_clusters;
```

## <a name="use-the-clustering-information"></a>Utiliser les informations de clustering

Étant donné que vous avez stocké la procédure de clustering dans la base de données, elle peut effectuer efficacement le clustering sur les données client stockées dans la même base de données. Vous pouvez exécuter la procédure chaque fois que vos données client sont mises à jour et utiliser les informations de clustering mises à jour.

Supposons que vous souhaitiez envoyer un e-mail promotionnel aux clients du cluster 0, groupe qui était inactif (vous pouvez voir comment les quatre clusters ont été décrits dans la [troisième partie](python-clustering-model-build.md#analyze-the-results) de ce tutoriel). Le code suivant sélectionne les adresses de messagerie des clients dans le cluster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[py_customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Vous pouvez modifier la valeur **c.cluster** pour renvoyer les adresses de messagerie des clients se trouvant dans d’autres clusters.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Une fois ce tutoriel terminé, supprimez la base de données tpcxbb_1gb.

## <a name="next-steps"></a>Étapes suivantes

Dans la quatrième partie de cette série de tutoriels, vous avez effectué les étapes suivantes :

* Créer une procédure stockée qui génère le modèle
* Effectuer le clustering sur le serveur
* Utiliser les informations de clustering

Pour en savoir plus sur l’utilisation de Python pour l’apprentissage automatique SQL, consultez :

* [Démarrage rapide : Créer et exécuter des scripts Python simples](quickstart-python-create-script.md)
* [Autres tutoriels Python pour l’apprentissage automatique SQL](python-tutorials.md)
* [Installer des packages Python avec sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md)
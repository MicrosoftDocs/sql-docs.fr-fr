---
title: 'Créer, exporter des modèles Spark ML : MLeap'
titleSuffix: SQL Server Big Data Clusters
description: Utilisez PySpark pour entraîner et créer des modèles de machine learning avec Spark sur des clusters Big Data SQL Server. Exportez avec MLeap, puis scorez le modèle avec Java dans SQL Server.
author: RogPodge
ms.author: roliu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 4b403644de65f866d4bafb191356fd08212c706f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043599"
---
# <a name="create-export-and-score-spark-machine-learning-models-on-big-data-clusters-2019"></a>Créer, exporter et scorer les modèles de machine learning Spark sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

L’exemple suivant montre comment créer un modèle avec [Spark ML](https://spark.apache.org/docs/latest/ml-guide.html), exporter le modèle vers [MLeap](http://mleap-docs.combust.ml/) et scorer le modèle dans SQL Server avec son [extension de langage Java](../language-extensions/language-extensions-overview.md). Ceci s’effectue dans le contexte d’un cluster Big Data SQL Server 2019.

Le schéma ci-dessous illustre le travail effectué dans cet exemple :

![Exportation des scores de l’entraînement avec Spark](./media/spark-create-machine-learning-model/train-score-export-with-spark.png)

## <a name="prerequisites"></a>Prérequis

Tous les fichiers de cet exemple se trouvent dans [https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml).

Pour exécuter l’exemple, vous devez également disposer des prérequis suivants :

- Un [cluster Big Data SQL Server](deploy-get-started.md)

- [Outils Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **curl**
   - **Azure Data Studio**

## <a name="model-training-with-spark-ml"></a>Entraînement du modèle avec Spark ML

Pour cet exemple, les données de recensement (**AdultCensusIncome.csv**) sont utilisées pour générer un modèle de pipeline Spark ML.

1. Utilisez le fichier [mleap_sql_test/Setup.sh](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/setup.sh) pour télécharger le jeu de données à partir d’Internet et le placer sur HDFS dans votre cluster Big Data SQL Server. Ceci le rend accessible par Spark.

1. Téléchargez ensuite l’exemple de notebook [train_score_export_ml_models_with_spark.ipynb](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb). À partir d’une ligne de commande PowerShell ou bash, exécutez la commande suivante pour télécharger le notebook :

   ```PowerShell
   curl -o mssql_spark_connector.ipynb "https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb"
   ```

   Ce notebook contient des cellules avec les commandes nécessaires pour cette section de l’exemple.

1. Ouvrez le notebook dans Azure Data Studio et exécutez chaque bloc de code. Pour plus d’informations sur l’utilisation des notebooks, consultez [Guide pratique pour utiliser des notebooks avec SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).

Les données sont d’abord lues dans Spark et divisées en jeux de données d’entraînement et de test. Ensuite, le code entraîne un modèle de pipeline avec les données d’entraînement. Enfin, il exporte le modèle vers un bundle MLeap.

> [!TIP]
> Vous pouvez également passer en revue ou exécuter le code Python associé à ces étapes en dehors du notebook dans le fichier [mleap_sql_test/mleap_pyspark.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_pyspark.py).

## <a name="model-scoring-with-sql-server"></a>Scoring du modèle avec SQL Server

Maintenant que le modèle de pipeline Spark ML se trouve dans un format de [bundle MLeap](http://mleap-docs.combust.ml/core-concepts/mleap-bundles.html) de sérialisation courant, vous pouvez scorer le modèle en Java sans la présence de Spark.

Cet exemple utilise l’[extension de langage Java](../language-extensions/language-extensions-overview.md) dans SQL Server. Pour pouvoir scorer le modèle dans SQL Server, vous devez d’abord créer une application Java qui peut charger le modèle dans Java et le scorer. Vous pouvez trouver l’exemple de code pour cette application Java dans le [dossier mssql-mleap-app](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mssql-mleap-app).

Après avoir créé l’exemple, vous pouvez utiliser Transact-SQL pour appeler l’application Java et scorer le modèle avec une table de base de données. Vous pouvez voir cela dans le fichier source [mleap_sql_test/mleap_sql_tests.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_sql_tests.py).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md)
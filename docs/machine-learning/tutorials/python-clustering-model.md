---
title: 'Tutoriel Python : Catégoriser les clients'
titleSuffix: SQL machine learning
description: Dans la quatrième partie de cette série de tutoriels, vous allez effectuer un clustering de clients à l’aide de K-moyennes dans une base de données en Python avec l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/26/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ac99e5442054161f87d67244f039224e61a58b3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351490"
---
# <a name="python-tutorial-categorizing-customers-using-k-means-clustering-with-sql-machine-learning"></a>Tutoriel Python : Catégoriser des clients à l’aide de k-moyennes avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans ce tutoriel en quatre parties, vous allez utiliser Python pour développer et déployer un modèle de clustering k-moyennes dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [Clusters Big Data](../../big-data-cluster/machine-learning-services.md) pour catégoriser les données clients.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans ce tutoriel en quatre parties, vous allez utiliser Python pour développer et déployer un modèle de clustering k-moyennes dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) pour clusteriser les données clients.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans cette série de quatre tutoriels, vous allez utiliser Python pour développer et déployer un modèle de clustering k-moyennes dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) pour clusteriser les données client.
::: moniker-end

Dans la première partie de cette série, vous allez configurer les conditions préalables pour le tutoriel, puis restaurer un exemple de jeu de données dans une base de données. Plus loin dans cette série, vous utiliserez ces données pour effectuer l’apprentissage et le déploiement d’un modèle de clustering dans Python avec l’apprentissage automatique SQL.

Dans les parties 2 et 3 de cette série, vous allez développer des scripts Python dans un notebook Azure Data Studio pour analyser et préparer vos données et effectuer l’apprentissage d’un modèle Machine Learning. Ensuite, dans la quatrième partie, vous exécuterez ces scripts Python à l’intérieur d’une base de données à l’aide de procédures stockées.

Le *clustering* permet d’organiser les données dans des groupes dont les membres sont similaires. Pour cette série de tutoriels, imaginez que vous êtes une entreprise de vente au détail. Vous allez utiliser l’algorithme **K-moyennes** pour effectuer le clustering des clients dans un jeu de données d’achats et de retours de produits. Lorsque vous effectuez le clustering de clients, vous pouvez concentrer plus efficacement vos efforts marketing en ciblant des groupes spécifiques. Le clustering K-myennes est un algorithme *d’apprentissage non supervisé* qui recherche des modèles dans les données en fonction de similarités.

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Restaurer un exemple de base de données

Dans la [deuxième partie](python-clustering-model-prepare-data.md), vous allez préparer les données d’une base de données pour effectuer le clustering.

Dans la [troisième partie](python-clustering-model-build.md), vous apprendrez à créer et à effectuer l’apprentissage d’un modèle de clustering dans Python.

Dans la [quatrième partie](python-clustering-model-deploy.md), vous allez créer une procédure stockée dans une base de données capable d’effectuer un clustering en Python sur la base des nouvelles données.

## <a name="prerequisites"></a>Prérequis

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) avec option de langage Python. Suivez les instructions d’installation du [Guide d’installation de Windows](../install/sql-machine-learning-services-windows-install.md) ou du [Guide d’installation de Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) avec option de langage Python. Suivez les instructions d’installation du [Guide d’installation de Windows](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) pour restaurer l’exemple de base de données sur Azure SQL Managed Instance.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) Vous allez utiliser un notebook dans Azure Data Studio pour Python et SQL. Pour plus d’informations sur les notebooks, consultez [Guide pratique pour utiliser des notebooks dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Packages Python supplémentaires : les exemples de cette série de tutoriel utilisent des packages Python qui peuvent déjà être installés.

  Ouvrez une **invite de commandes** et accédez au chemin d’installation de la version de Python que vous utilisez dans Azure Data Studio. Par exemple : `cd %LocalAppData%\Programs\Python\Python37-32`. Exécutez ensuite les commandes suivantes pour installer les packages qui ne sont pas déjà installés.

  ```console
  pip install matplotlib
  pip install pandas
  pip install pyodbc
  pip install scipy
  pip install sklearn
  ```

## <a name="restore-the-sample-database"></a>Restaurer les exemples de base de données

L’exemple de jeu de données utilisé dans ce tutoriel a été enregistré dans un fichier de sauvegarde de base de données **.bak** que vous pouvez télécharger et utiliser. Ce jeu de données est dérivé du jeu de données [TPCX-BB](http://www.tpc.org/tpcx-bb/default5.asp) fourni par le [TPC (Transaction Processing Performance Council)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Si vous utilisez Machine Learning Services sur des clusters Big Data, consultez [Restaurer une base de données dans l’instance principale du cluster Big Data SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Téléchargez le fichier [tpcxbb_1gb. bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Suivez les instructions dans [Restaurer une base de données à partir d’un fichier de sauvegarde](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) dans Azure Data Studio, en utilisant les informations suivantes :

   * Importez à partir du fichier **tpcxbb_1gb.bak** que vous avez téléchargé
   * Nommez la base de données cible « tpcxbb_1gb ».

1. Vous pouvez vérifier que le jeu de données existe après la restauration de la base de données en interrogeant la table **dbo.customer** :

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Téléchargez le fichier [tpcxbb_1gb. bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Suivez les instructions de [Restauration d’une base de données sur une instance gérée](/azure/sql-database/sql-database-managed-instance-get-started-restore) dans SQL Server Management Studio, en utilisant les informations suivantes :

   * Importez à partir du fichier **tpcxbb_1gb.bak** que vous avez téléchargé
   * Nommez la base de données cible « tpcxbb_1gb ».

1. Vous pouvez vérifier que le jeu de données existe après la restauration de la base de données en interrogeant la table **dbo.customer** :

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne poursuivez pas ce tutoriel, supprimez la base de données tpcxbb_1gb.

## <a name="next-steps"></a>Étapes suivantes

Dans la première partie de cette série de tutoriels, vous avez effectué les étapes suivantes :

* Restaurer un exemple de base de données

Pour préparer les données pour le modèle Machine Learning, suivez la deuxième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel Python : Préparer des données pour effectuer un clustering](python-clustering-model-prepare-data.md)
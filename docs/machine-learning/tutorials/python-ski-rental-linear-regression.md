---
title: 'Tutoriel Python : Locations de ski'
titleSuffix: SQL machine learning
description: Dans cette série de tutoriels en quatre parties, vous allez générer un modèle de régression linéaire dans Python afin de prédire les locations de skis avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f63ffdc74fab82ca5bbf803e3dbc34c66ae5e629
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101979"
---
# <a name="python-tutorial-predict-ski-rental-with-linear-regression-with-sql-machine-learning"></a>Tutoriel Python : Prédire la location de skis avec la régression linéaire et le Machine Learning SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans cette série de tutoriels en quatre parties, vous allez utiliser Python et la régression linéaire dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md) pour prédire le nombre de locations de skis. Le tutoriel utilise un [notebook Python dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end
::: moniker range="=sql-server-2017"
Dans ce tutoriel en quatre parties, vous allez utiliser Python et la régression linéaire dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) pour prédire le nombre de locations de ski. Le tutoriel utilise un [notebook Python dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans cette série de quatre tutoriels, vous allez utiliser Python et la régression linéaire dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) pour prédire le nombre de locations de ski. Le tutoriel utilise un [notebook Python dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).
::: moniker-end

Imaginez que vous êtes une entreprise de location de ski et que vous souhaitez prédire le nombre de locations à une date ultérieure. Ces informations vous aideront à préparer votre inventaire, votre personnel et vos installations.

Dans la première partie de ce tutoriel, vous allez configurer les composants requis. Dans les parties 2 et 3, vous allez développer des scripts Python dans un notebook pour préparer vos données et effectuer l’apprentissage d’un modèle Machine Learning. Ensuite, dans la troisième partie, vous exécuterez ces scripts Python à l’intérieur de la base de données à l’aide de procédures stockées T-SQL.

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Importer un exemple de base de données

Dans la [deuxième partie](python-ski-rental-linear-regression-prepare-data.md), vous allez apprendre à charger les données à partir d’une base de données dans une trame de données Python et à préparer les données dans Python.

Dans la [troisième partie](python-ski-rental-linear-regression-train-model.md), vous apprendrez à effectuer la formation d’un modèle de régression linéaire dans Python.

Dans la [quatrième partie](python-ski-rental-linear-regression-deploy-model.md), vous allez apprendre à stocker le modèle dans une base de données, puis à créer des procédures stockées à partir des scripts Python que vous avez développés dans les parties 2 et 3. Les procédures stockées sont exécutées sur le serveur pour effectuer des prédictions en fonction de nouvelles données.

## <a name="prerequisites"></a>Prérequis

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* SQL Server Machine Learning Services - Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* SQL Server Machine Learning Services - Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services dans Azure SQL Managed Instance - Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) pour restaurer l’exemple de base de données sur Azure SQL Managed Instance.
::: moniker-end

* Environnement de développement intégré (IDE) Python : ce didacticiel utilise un notebook Python dans [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Pour en savoir plus, consultez [Comment utiliser les notebooks dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Outil de requête SQL : ce didacticiel part du principe que vous utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* Packages Python supplémentaires : les exemples de cette série de tutoriel utilisent les packages Python suivants, qui peuvent ne pas être installés par défaut :

  * pandas
  * pyodbc
  * sklearn

  Pour installer ces packages :
  1. Dans votre notebook Azure Data Studio, sélectionnez **Gérer les packages**.
  2. Dans le volet **Gérer les packages**, sélectionnez l’onglet **Ajouter nouveau**.
  3. Pour chacun des packages suivants, entrez le nom du package, cliquez sur **Rechercher**, puis cliquez sur **Installer**.

  Vous pouvez également ouvrir une **invite de commandes**, modifier le chemin d’installation de la version de Python que vous utilisez dans Azure Data Studio (par exemple, `cd %LocalAppData%\Programs\Python\Python37-32`), puis exécuter `pip install` pour chaque package.

## <a name="restore-the-sample-database"></a>Restaurer les exemples de base de données

L’exemple de base de données utilisé dans ce tutoriel a été enregistré dans un fichier de sauvegarde de base de données **.bak** que vous pouvez télécharger et utiliser.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Si vous utilisez Machine Learning Services sur des clusters Big Data, consultez [Restaurer une base de données dans l’instance principale du cluster Big Data SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Téléchargez le fichier [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Suivez les instructions dans [Restaurer une base de données à partir d’un fichier de sauvegarde](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) dans Azure Data Studio, en utilisant les informations suivantes :

   * Importer à partir du fichier **TutorialDB.bak** que vous avez téléchargé
   * Nommez la base de données cible « TutorialDB ».

1. Vous pouvez vérifier que la base de données restaurée existe en interrogeant la table **dbo.rental_data** :

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Téléchargez le fichier [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Suivez les instructions de [Restauration d’une base de données sur une instance gérée](/azure/sql-database/sql-database-managed-instance-get-started-restore) dans SQL Server Management Studio, en utilisant les informations suivantes :

   * Importer à partir du fichier **TutorialDB.bak** que vous avez téléchargé
   * Nommez la base de données cible « TutorialDB ».

1. Vous pouvez vérifier que la base de données restaurée existe en interrogeant la table **dbo.rental_data** :

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne poursuivez pas ce tutoriel, supprimez la base de données TutorialDB.

## <a name="next-steps"></a>Étapes suivantes

Dans la première partie de cette série de tutoriels, vous avez effectué les étapes suivantes :

* Installer les prérequis
* Importer un exemple de base de données

Pour préparer les données de la base de données TutorialDB, suivez la deuxième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel Python : Préparer les données pour la formation d’un modèle de régression linéaire](python-ski-rental-linear-regression-prepare-data.md)
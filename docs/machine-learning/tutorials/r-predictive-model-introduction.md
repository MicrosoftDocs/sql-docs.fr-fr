---
title: 'Tutoriel : Développer un modèle prédictif en R'
titleSuffix: SQL machine learning
description: Dans cette série de tutoriels en quatre parties, vous allez développer des données pour effectuer l’apprentissage d’un modèle prédictif dans R avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5376d2b7876914c28d656574a57f5fc80294c63c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348044"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutoriel : Développement d’un modèle prédictif en R avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans cette série de tutoriels en quatre parties, vous allez utiliser R et un modèle Machine Learning dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md) pour prédire le nombre de locations de skis.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans cette série de tutoriels en quatre parties, vous allez utiliser R et un modèle Machine Learning dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) pour prédire le nombre de locations de skis.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans cette série de tutoriels en quatre parties, vous allez utiliser R et un modèle Machine Learning dans [SQL Server R Services](../r/sql-server-r-services.md) pour prédire le nombre de locations de skis.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans cette série de quatre tutoriels, vous allez utiliser R et un modèle Machine Learning dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) pour prédire le nombre de locations de ski.
::: moniker-end

Imaginez que vous êtes une entreprise de location de ski et que vous souhaitez prédire le nombre de locations à une date ultérieure. Ces informations vous aideront à préparer votre inventaire, votre personnel et vos installations.

Dans la première partie de ce tutoriel, vous allez configurer les composants requis. Dans les parties 2 et 3, vous allez développer des scripts R dans un notebook pour préparer vos données et effectuer l’apprentissage d’un modèle Machine Learning. Ensuite, dans la troisième partie, vous exécuterez ces scripts R à l’intérieur d’une base de données à l’aide de procédures stockées T-SQL.

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Restaurer un exemple de base de données 

Dans la [deuxième partie](r-predictive-model-prepare-data.md), vous allez apprendre à charger les données à partir d’une base de données dans une trame de données Python et à préparer les données dans R.

Dans la [troisième partie](r-predictive-model-train.md), vous apprendrez à effectuer l’apprentissage d’un modèle Machine Learning dans R.

Dans la [quatrième partie](r-predictive-model-deploy.md), vous apprendrez à stocker le modèle dans une base de données, puis à créer des procédures stockées à partir des scripts R que vous avez développés dans les parties 2 et 3. Les procédures stockées sont exécutées sur le serveur pour effectuer des prédictions en fonction de nouvelles données.

## <a name="prerequisites"></a>Prérequis

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* SQL Server Machine Learning Services - Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* SQL Server Machine Learning Services - Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
* SQL Server 2016 R Services. Pour installer R Services, consultez le [Guide d’installation Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) pour restaurer l’exemple de base de données sur Azure SQL Managed Instance.
::: moniker-end

* R IDE : ce tutoriel utilise [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC : ce pilote est utilisé dans les scripts R que vous allez développer à l’aide de ce tutoriel. S’il n’est pas déjà installé, installez-le à l’aide de la commande R `install.packages("RODBC")`. Pour plus d’informations sur RODBC, consultez [CRAN - Package RODBC](https://CRAN.R-project.org/package=RODBC).

* Outil de requête SQL : ce didacticiel part du principe que vous utilisez [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Pour en savoir plus, consultez [Comment utiliser les notebooks dans Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

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
* Restaurer un exemple de base de données

Pour préparer les données pour le modèle Machine Learning, suivez la deuxième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Préparer les données pour effectuer l’apprentissage d’un modèle prédictif en R](r-predictive-model-prepare-data.md)
---
title: 'Didacticiel R : Prédire les tarifs des taxis de New York avec classification binaire'
titleSuffix: SQL machine learning
description: Dans cette série de tutoriels en cinq parties, apprenez à incorporer du code R dans des procédures stockées SQL Server et des fonctions T-SQL avec SQL Machine Learning pour prédire les tarifs des taxis de New York à l’aide de la classification binaire.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 5cae9925ab4c805028fcd73c47b41d829f6101ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354296"
---
# <a name="r-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Didacticiel R : Prédire les tarifs des taxis de New York avec classification binaire
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans ce tutoriel en cinq parties pour les programmeurs SQL, vous apprendrez à vous familiariser avec l’intégration de R dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017"
Dans ce tutoriel en cinq parties pour les programmeurs SQL, vous apprendrez à vous familiariser avec l’intégration de R dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2016"
Dans ce tutoriel en cinq parties pour les programmeurs SQL, vous apprendrez à vous familiariser avec l’intégration de R dans [SQL Server 2016 R Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
Dans ce tutoriel en cinq parties destiné aux programmeurs SQL, vous allez vous familiariser avec l’intégration de R à [Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Vous allez créer et déployer une solution Machine Learning basée sur R à l’aide d’un exemple de base de données sur SQL Server. Vous allez utiliser T-SQL, Azure Data Studio ou SQL Server Management Studio et une instance de base de données avec Machine Learning SQL et la prise en charge du langage R

Ce didacticiel en plusieurs parties vous présente les fonctions R utilisées dans un workflow de modélisation des données. Les parties incluent l’exploration des données, la création et l’apprentissage d’un modèle de classification binaire et le déploiement d’un modèle. Vous allez utiliser des exemples de données provenant de la Commission des services de taxis et de limousines de la ville de New York. Le modèle que vous allez créer prévoit si un trajet est susceptible de générer un pourboire en fonction de l’heure de la journée, de la distance parcourue et de l’emplacement de la prise en charge du passager.

Dans la première partie de cette série, vous allez installer les composants requis et restaurer l’exemple de base de données. Dans les parties 2 et 3, vous allez développer des scripts R pour préparer vos données et effectuer l’apprentissage d’un modèle Machine Learning. Ensuite, dans les parties quatre et cinq, vous exécuterez ces scripts R à l’intérieur de la base de données à l’aide de procédures stockées T-SQL.

Dans cet article, vous allez :

> [!div class="checklist"]
> + Installer les prérequis
> + Restaurer les exemples de base de données

Dans la [partie deux](r-taxi-classification-explore-data.md), vous explorez les exemples de données et générez des tracés.

Dans la [troisième partie](r-taxi-classification-create-features.md), vous apprendrez à créer des fonctionnalités à partir de données brutes à l’aide d’une fonction Transact-SQL. Ensuite, vous appellerez cette fonction à partir d’une procédure stockée pour créer une table qui contient les valeurs des caractéristiques.

Dans la [quatrième partie](r-taxi-classification-train-model.md), vous chargez les modules et appelez les fonctions nécessaires pour créer et entraîner le modèle à l’aide d’une procédure stockée SQL Server.

Dans la [cinquième partie](r-taxi-classification-deploy-model.md), vous apprendrez à rendre opérationnels les modèles que vous avez formés et enregistrés dans la quatrième partie.

> [!NOTE]
> Ce tutoriel est disponible au format R et Python. Pour obtenir la version de Python, consultez [Didacticiel Python : Prédire les tarifs des taxis de New York avec classification binaire](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Prérequis

::: moniker range="=sql-server-2016"
+ Installer [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md#verify-installation)
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
+ Installer [SQL Server Machine Learning Services avec R activé](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ Installer les [bibliothèques R](../package-management/r-package-information.md)

+ [Octroi d’autorisations d’exécution de scripts Python](../security/user-permission.md)

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ À compter de SQL Server 2019, le mécanisme d’isolation vous oblige à accorder les autorisations appropriées au répertoire dans lequel le fichier de traçage est stocké. Pour savoir comment définir ces autorisations, consultez la [section Autorisations de fichiers dans SQL Server 2019 sur Windows : Modifications de l’isolation dans Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

+ Restaurer la [Base de données de démonstration Taxis de New York](demo-data-nyctaxi-in-sql.md)

Toutes les tâches peuvent être effectuées à l’aide de procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] dans Azure Data Studio ou [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

Vous devez être familiarisé avec les opérations de base de données, telles que la création de bases de données et de tables, l’importation de données et la rédaction de requêtes SQL. Vous n’avez pas besoin de connaître R, car tout le code R est fourni.

## <a name="background-for-sql-developers"></a>Arrière-plan pour les développeurs SQL

Le processus de création d’une solution de Machine Learning est complexe. Il peut impliquer plusieurs outils et la coordination de plusieurs experts durant les différentes phases :

+ Extraction et nettoyage des données
+ Exploration des données et création de caractéristiques utiles pour la modélisation
+ Apprentissage et optimisation du modèle
+ Déploiement en production

Le développement et les tests du code réel fournissent de meilleurs résultats dans un environnement de développement dédié à R. Toutefois, une fois que le script est entièrement testé, vous pouvez facilement le déployer sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l’environnement familier d’Azure Data Studio ou [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. L’encapsulation de code externe dans les procédures stockées est le mécanisme principal permettant de rendre le code opérationnel dans SQL Server.

Une fois que le modèle a été enregistré dans la base de données, vous pouvez l’appeler pour vos prédictions dans [!INCLUDE[tsql](../../includes/tsql-md.md)] à l’aide de procédures stockées.

Que vous soyez un programmeur SQL ne connaissant pas R ou un développeur R ne connaissant pas SQL, ce tutoriel en cinq parties présente un workflow standard pour effectuer des analyses dans des bases de données avec R et SQL Server.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :

> [!div class="checklist"]
> + Installer les éléments requis
> + Restaurer l’exemple de base de données

> [!div class="nextstepaction"]
> [Didacticiel R : Explorer et visualiser des données](r-taxi-classification-explore-data.md)
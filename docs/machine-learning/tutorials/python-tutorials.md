---
title: Tutoriels sur Python
titleSuffix: SQL machine learning
description: Cet article décrit les tutoriels Python pour l’apprentissage automatique SQL. Découvrez comment exécuter des scripts et générer des modèles de Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4b8e997b6990d4d81e9667a41cf359208ca0f31e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341468"
---
# <a name="python-tutorials-for-sql-machine-learning"></a>Tutoriels Python pour l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Cet article décrit les tutoriels et les démarrages rapides Python pour [Machine Learning Services sur SQL Serveur](../sql-server-machine-learning-services.md) et les [clusters Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
Cet article décrit les tutoriels et les démarrages rapides Python pour [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Cet article décrit les tutoriels et les démarrages rapides Python pour [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

<a name="bkmk_pythontutorials"></a>

## <a name="python-tutorials"></a>Tutoriels sur Python

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
| Didacticiel | Description |
|-|-|
| [Prédire les locations de skis avec la régression linéaire](python-ski-rental-linear-regression.md) | Utilisez Python et la régression linéaire pour prédire le nombre de skis qui seront loués. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Classement des clients par catégorie à l’aide du clustering k-moyennes](python-clustering-model.md) | Utilisez Python pour développer et déployer un modèle de clustering K-moyennes en vue de classer les clients par catégorie. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Créer un modèle à l’aide de revoscalepy](use-python-revoscalepy-to-create-model.md) | Montre comment exécuter du code à partir d’un client Python distant avec SQL Server comme contexte de calcul. Le tutoriel crée un modèle en utilisant **rxLinMod** de la bibliothèque **revoscalepy**. |
| [Analytique données Python pour développeurs SQL](python-taxi-classification-introduction.md) | Cette procédure pas à pas détaille l’ensemble du processus de création d’une solution Python complète en utilisant T-SQL. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Didacticiel | Description |
|-|-|
| [Prédire les locations de skis avec la régression linéaire](python-ski-rental-linear-regression.md) | Utilisez Python et la régression linéaire pour prédire le nombre de skis qui seront loués. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Classement des clients par catégorie à l’aide du clustering k-moyennes](python-clustering-model.md) | Utilisez Python pour développer et déployer un modèle de clustering K-moyennes en vue de classer les clients par catégorie. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
::: moniker-end

## <a name="python-quickstarts"></a>Démarrages rapides Python

Si vous débutez avec l’apprentissage automatique SQL, vous pouvez aussi essayer les démarrages rapides Python.

| Démarrage rapide | Description |
|-|-|
| [Exécuter des scripts Python simples](quickstart-python-create-script.md) | Découvrez les principes de base de l’appel de Python dans T-SQL à l’aide de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Structures de données et objets avec Python](quickstart-python-data-structures.md) | Montre comment SQL utilise le package Pandas Python pour gérer les structures de données. |
| [Créer et scorer un modèle prédictif dans Python](quickstart-python-train-score-model.md) | Explique comment créer, entraîner et utiliser un modèle Python pour effectuer des prédictions à partir de nouvelles données. |

## <a name="next-steps"></a>Étapes suivantes

+ [Extension Python dans SQL Server](../concepts/extension-python.md)

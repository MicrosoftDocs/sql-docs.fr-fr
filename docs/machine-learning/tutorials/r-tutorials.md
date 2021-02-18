---
title: Tutoriels sur R
titleSuffix: SQL machine learning
description: Cet article décrit les tutoriels R pour le Machine Learning SQL. Découvrez comment exécuter des scripts et générer des modèles Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 8b463ded43f7906627e08d86eb1809b169e6125d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346246"
---
# <a name="r-tutorials-for-sql-machine-learning"></a>Tutoriels R pour le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Cet article décrit les tutoriels et les démarrages rapides R pour [Machine Learning Services sur SQL Serveur](../sql-server-machine-learning-services.md) et les [clusters Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
Cet article décrit les tutoriels et les démarrages rapides R pour [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2016"
Cet article décrit les tutoriels et les démarrages rapides R pour [SQL Server 2016 R Services](../r/sql-server-r-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Cet article décrit les tutoriels et les démarrages rapides Python pour [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

<a name="bkmk_sqltutorials"></a>

## <a name="r-tutorials"></a>Tutoriels sur R

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
| Didacticiel | Description |
|------|-------------|
| [Prédire la location de skis avec l’arbre de décision](r-predictive-model-introduction.md) | Utilisez R et un modèle d’arbre de décision pour prédire le nombre de futures locations de skis. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Classement des clients par catégorie à l’aide du clustering k-moyennes](r-clustering-model-introduction.md) | Utilisez R pour développer et déployer un modèle de clustering k-moyennes en vue de classer les clients par catégorie. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Analytique R en base de données pour les scientifiques de données](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) | Ce tutoriel explique aux développeurs R qui débutent avec le Machine Learning SQL comment effectuer des tâches courantes de science des données dans SQL. Chargez et visualisez les données, entraînez et enregistrez un modèle dans une base de données, et utilisez-le à des fins d’analyse prédictive. |
| [Analytique R en base de données pour les développeurs SQL](../tutorials/r-taxi-classification-introduction.md) | Créez et déployez une solution R complète à l’aide d’outils SQL uniquement. Se concentre sur le déplacement d’une solution en production. Vous découvrirez comment encapsuler du code R dans une procédure stockée, enregistrer un modèle R dans une base de données et effectuer des appels paramétrables au modèle R à des fins de prédiction. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Didacticiel | Description |
|------|-------------|
| [Prédire la location de skis avec l’arbre de décision](r-predictive-model-introduction.md) | Utilisez R et un modèle d’arbre de décision pour prédire le nombre de futures locations de skis. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
| [Classement des clients par catégorie à l’aide du clustering k-moyennes](r-clustering-model-introduction.md) | Utilisez R pour développer et déployer un modèle de clustering k-moyennes en vue de classer les clients par catégorie. Utilisez des notebooks dans Azure Data Studio pour préparer les données et entraîner le modèle, et T-SQL pour le déploiement du modèle. |
::: moniker-end

## <a name="r-quickstarts"></a>Démarrages rapides R

Si vous débutez avec le Machine Learning SQL, vous pouvez aussi essayer les démarrages rapides R.

| Démarrage rapide | Description |
|-|-|
| [Exécuter des scripts R simples](quickstart-r-create-script.md) | Découvrez les principes de base de l’appel de R dans T-SQL à l’aide de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Objets et structures de données avec R](quickstart-r-data-types-and-objects.md) | Montre comment SQL utilise R pour gérer les structures de données. |
| [Créer et scorer un modèle prédictif dans R](quickstart-r-data-types-and-objects.md) | Explique comment créer, entraîner et utiliser un modèle R pour effectuer des prédictions à partir de nouvelles données. |

## <a name="next-steps"></a>Étapes suivantes

+ [Extension Python dans SQL Server](../concepts/extension-r.md)

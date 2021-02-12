---
title: Faire des prédictions avec l’extension Machine Learning
description: Découvrez comment utiliser l’extension Machine Learning pour Azure Data Studio pour faire des prédictions avec un modèle ONNX dans votre base de données.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 6da9918b4ebb3ed571d82d23c123c8e477741c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048539"
---
# <a name="make-predictions-with-machine-learning-extension-for-azure-data-studio-preview"></a>Faire des prédictions avec l’extension Machine Learning pour Azure Data Studio (préversion)

Découvrez comment utiliser [l’extension Machine Learning pour Azure Data Studio](machine-learning-extension.md) pour faire des prédictions avec un modèle ONNX dans votre base de données. L’extension génère un script T-SQL à l’aide de [PREDICT](../../t-sql/queries/predict-transact-sql.md) pour effectuer des prédictions sur le jeu de données stocké dans votre table avec un modèle précédemment importé, qui réside dans un fichier local ou à partir de Azure Machine Learning.

> [!IMPORTANT]
> Faire des prédictions avec l’extension de Machine Learning ne prend actuellement en charge que les services[Machine Learning dans les instances managées Azure SQL Edge](/azure/azure-sql/managed-instance/machine-learning-services-overview) et [Azure SQL Edge avec ONNX](/azure/azure-sql-edge/onnx-overview).

## <a name="prerequisites"></a>Prérequis

- Installez et configurez l’[extension Machine Learning pour Azure Data Studio](machine-learning-extension.md). Vous devez spécifier les [chemins d’installation Python dans les paramètres d’extension](machine-learning-extension.md#settings).

- Les packages Python **onnxruntime**, **mlflow** et **mlflow-DBStore**. L’extension Machine Learning vous invite à installer le package, le cas échéant.

## <a name="make-predictions-from-onnx-model"></a>Effectuer des prédictions à partir d’un modèle ONNX

Suivez les étapes ci-dessous pour utiliser un modèle ONNX pour effectuer des prédictions.

1. Sélectionnez **Effectuer des prédictions**.

1. Si vous êtes invité à installer **onnxruntime**, **mlflow** et **mlflow-dbstore**, sélectionnez **Oui**.

1. Choisissez l’emplacement où se trouve votre modèle, puis sélectionnez **Suivant**. Vous pouvez utiliser :
    - **Modèles importés**. Choisissez cette valeur pour utiliser un modèle qui est déjà stocké dans votre base de données. Choisissez le **Modèle de base de données** et la **Table de modèle** où se trouve votre modèle, sélectionnez le modèle que vous souhaitez utiliser, puis cliquez sur **Suivant**.
    - **Chargement de fichiers**. Choisissez cette option pour utiliser un modèle à partir d’un fichier. Sélectionnez le fichier de modèle sous **Fichiers sources** puis sélectionnez **Suivant**.
    - **Azure Machine Learning**. Choisissez cette valeur pour utiliser un modèle à partir d’Azure Machine Learning. Tout d’abord, **Connectez-vous à Azure**. Sélectionnez ensuite votre **compte Azure**, **abonnement Azure**, **groupe de ressources Azure** et votre **espace de travail Azure ML**. Sélectionnez le modèle que vous souhaitez utiliser, puis sélectionnez **Suivant**.

1. Mapper les données sources à votre modèle.
    - Sélectionnez la **Base de données sources** et la **Table source** contenant le jeu de données pour lequel vous souhaitez appliquer la prédiction.
    - Mappez les colonnes sous **Mappage d’entrée de modèle** et **Sortie de modèle**. L’extension mappe automatiquement les colonnes qui ont le même nom et le même type de données.

1. Sélectionnez **Prédire**.

Azure Data Studio créera une nouvelle requête T-SQL avec [PREDICT](../../t-sql/queries/predict-transact-sql.md), que vous pouvez utiliser pour effectuer des prédictions sur vos données.

## <a name="next-steps"></a>Étapes suivantes

- [Extension Machine Learning dans Azure Data Studio](machine-learning-extension.md)
- [Gérer les packages dans la base de données](machine-learning-extension-manage-packages.md)
- [Importer ou afficher des modèles](machine-learning-extension-import-view-models.md)
- [Notebooks dans Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentation sur SQL Machine Learning](../../machine-learning/index.yml)
- [Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [Machine Learning et intelligence artificielle avec ONNX dans SQL Edge (préversion)](/azure/azure-sql-edge/onnx-overview)
---
title: Importer ou afficher des modèles avec l’extension Machine Learning
description: Découvrez comment utiliser l’extension Machine Learning pour Azure Data Studio pour importer un modèle ONNX ou afficher des modèles déjà importés dans votre base de données.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 3f55f793391e408d96b8c6d07a31c7e790d54853
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048529"
---
# <a name="import-or-view-models-with-machine-learning-extension-for-azure-data-studio-preview"></a>Importer ou afficher des modèles avec l’extension Machine Learning pour Azure Data Studio (préversion)

Découvrez comment utiliser [l’extension Machine Learning pour Azure Data Studio](machine-learning-extension.md) pour importer un modèle ONNX ou afficher des modèles déjà importés dans votre base de données.

> [!IMPORTANT]
> L’importation et l’affichage dans une base de données avec l’extension de Machine Learning ne prend actuellement en charge que les services[Machine Learning dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) et [Azure SQL Edge avec ONNX](/azure/azure-sql-edge/onnx-overview).

## <a name="prerequisites"></a>Prérequis

- Installez et configurez l’[extension Machine Learning pour Azure Data Studio](machine-learning-extension.md). Vous devez spécifier les [chemins d’installation Python dans les paramètres d’extension](machine-learning-extension.md#settings).

- Les packages Python **onnxruntime**, **mlflow** et **mlflow-DBStore**. L’extension Machine Learning vous invite à installer le package, le cas échéant.

## <a name="view-models"></a>Afficher les modèles

Suivez les étapes ci-dessous pour afficher les modèles ONNX stockés dans votre base de données.

1. Sélectionnez **Importer ou afficher des modèles**.

1. Si vous êtes invité à installer **onnxruntime**, **mlflow** et **mlflow-dbstore**, sélectionnez **Oui**.

1. Sélectionnez la **base de données de modèles** et **la table de modèles** où vos modèles sont stockés.

Une liste de vos modèles s’affiche. Vous pouvez modifier le nom et la description du modèle, ou supprimer un modèle de la liste.

## <a name="import-a-new-model"></a>Importer un nouveau modèle

Suivez les étapes ci-dessous pour importer un modèle ONNX dans votre base de données.

1. Sélectionnez **Importer ou afficher des modèles**.

1. Si vous êtes invité à installer **onnxruntime**, **mlflow** et **mlflow-dbstore**, sélectionnez **Oui**.

1. Sélectionnez **Importer des modèles**.

1. Sélectionnez la **Base de données de modèles** dans laquelle vous souhaitez stocker le modèle importé.

1. Sélectionnez la **Table de modèles** dans laquelle vous souhaitez stocker le modèle importé. Vous pouvez choisir une **Table existante** ou créer une **Nouvelle table**. Sélectionnez **Suivant**.

1. Choisissez l’emplacement de votre modèle, puis sélectionnez **Suivant**. Vous pouvez utiliser :
    - **Chargement de fichiers**. Choisissez cette option pour utiliser un modèle à partir d’un fichier. Sélectionnez le fichier de modèle sous **Fichiers sources** puis sélectionnez **Suivant**.
    - **Azure Machine Learning**. Choisissez cette valeur pour utiliser un modèle à partir de Azure Machine Learning. Tout d’abord, **Connectez-vous à Azure**. Sélectionnez ensuite votre **compte Azure**, **abonnement Azure**, **groupe de ressources Azure** et votre **espace de travail Azure ML**. Sélectionnez le modèle que vous souhaitez utiliser, puis sélectionnez **Suivant**.

1. Entrez le **Nom** et la **Description** du modèle, puis sélectionnez **Déployer** pour stocker le modèle dans votre base de données.

> [!NOTE]
> L’extension Machine Learning est actuellement en préversion. Par conséquent, le schéma de la table dans lequel les modèles sont stockés peut changer à l’avenir.

## <a name="next-steps"></a>Étapes suivantes

- [Extension Machine Learning dans Azure Data Studio](machine-learning-extension.md)
- [Gérer les packages dans la base de données](machine-learning-extension-manage-packages.md)
- [Effectuer des prédictions](machine-learning-extension-predictions.md)
- [Documentation sur SQL Machine Learning](../../machine-learning/index.yml)
- [Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [Machine Learning et intelligence artificielle avec ONNX dans SQL Edge (préversion)](/azure/azure-sql-edge/onnx-overview)

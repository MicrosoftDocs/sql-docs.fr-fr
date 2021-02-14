---
title: Extension Machine Learning
description: L’extension Machine Learning pour Azure Data Studio vous permet de gérer les packages, d’importer des modèles Machine Learning, d’effectuer des prédictions et de créer des notebooks pour exécuter des expériences pour vos bases de données SQL.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: 8eb7dd5f5a11c508af0d85e7559349c14e558adb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051950"
---
# <a name="machine-learning-extension-for-azure-data-studio-preview"></a>Extension Machine Learning pour Azure Data Studio (préversion)

L’extension Machine Learning pour [Azure Data Studio](../what-is-azure-data-studio.md) vous permet de gérer les packages, d’importer des modèles Machine Learning, d’effectuer des prédictions et de créer des notebooks pour exécuter des expériences pour vos bases de données SQL. Cette extension est actuellement en préversion.

## <a name="prerequisites"></a>Prérequis

Les prérequis suivants doivent être installés sur l’ordinateur sur lequel vous exécutez Azure Data Studio.

- [Python 3](https://www.python.org/downloads/). Une fois que vous avez installé Python, vous devez spécifier le chemin d’accès local à une installation Python sous [Paramètres d’extension](#settings). Si vous avez utilisé un [notebook noyau Python](../notebooks/notebooks-python-kernel.md) dans Azure Data Studio, l’extension utilisera le chemin d’accès du notebook par défaut.

- [Microsoft ODBC driver 17 for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md) pour Windows, macOS ou Linux.

- [R 3.5](https://www.r-project.org/) (facultatif). Les versions autres que R 3.5 ne sont pas prises en charge pour le moment. Une fois que vous avez installé R 3.5, vous devez activer R et spécifier le chemin d’accès local à une installation R sous [Paramètres d’extension](#settings). Cette opération n’est requise que si vous souhaitez gérer des packages R dans votre base de données.

### <a name="trouble-installing-python-3-from-within-ads"></a>Vous rencontrez des problèmes lors de l’installation de Python 3 depuis ADS ?

Si vous tentez d’installer Python 3, mais que vous recevez une erreur concernant TLS/SSL, ajoutez les deux composants facultatifs suivants :

_Exemple d’erreur :_
```
$: ~/0.0.1/bin/python3 -m pip install --user "jupyter>=1.0.0" --extra-index-url https://prose-python-packages.azurewebsites.net
WARNING: pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Looking in indexes: https://pypi.org/simple, https://prose-python-packages.azurewebsites.net
Requirement already satisfied: jupyter
```

_Installez les composants suivants :_

- [Homebrew](https://brew.sh) (facultatif). Installez homebrew, puis exécutez `brew update` à partir de la ligne de commande.

- *openssl* (facultatif). Exécutez ensuite `brew install openssl`.

## <a name="install-the-extension"></a>Installer l’extension

Pour installer l’extension Machine Learning dans Azure Data Studio, suivez les étapes ci-dessous.

1. Ouvrez le gestionnaire d’extensions dans Azure Data Studio. Vous pouvez sélectionner l’icône des extensions ou l’option Extensions dans le menu Affichage.

1. Sélectionnez l’extension **Machine Learning**, puis affichez les détails associés.

1. Sélectionnez **Installer**.

1. Sélectionnez **Recharger** pour activer l’extension. Cette opération n’est requise que la première fois que vous installez une extension.

<a name="settings"></a>

## <a name="extension-settings"></a>Paramètres d’extension

Pour modifier les paramètres de l’extension Machine Learning, procédez comme suit.

1. Ouvrez le gestionnaire d’extensions dans Azure Data Studio. Vous pouvez sélectionner l’icône des extensions ou l’option Extensions dans le menu Affichage.

1. Recherchez l’extension **Machine Learning** sous les extensions **activées**.

1. Sélectionnez l’icône **Gérer**.

1. Sélectionnez l’icône **Paramètres d’extension**.

Vos paramètres d’extension ressemblent à ceci :

:::image type="content" source="media/machine-learning-extension/ml-extension-settings.png" alt-text="Paramètres de l’extension Machine Learning":::

### <a name="enable-python"></a>Activer Python

Pour utiliser l’extension Machine Learning ainsi que la gestion des packages Python dans votre base de données, procédez comme suit.

> [!IMPORTANT]
> L’extension Machine Learning nécessite l’activation de Python et sa configuration de façon à utiliser la plupart des fonctionnalités, même si vous ne souhaitez pas utiliser la gestion des packages Python dans les fonctionnalités de la base de données.

1. Vérifiez que **Machine Learning : Activer Python** est activé. Ce paramètre est activé par défaut.

1. Indiquez le chemin d’accès à votre installation Python préexistante sous **Machine Learning : chemin Python**. Il peut s’agir du chemin d’accès complet à l’exécutable Python ou du dossier dans lequel se trouve l’exécutable. Si vous avez utilisé un [notebook noyau Python](../notebooks/notebooks-python-kernel.md) dans Azure Data Studio, l’extension utilisera le chemin d’accès du notebook par défaut.

### <a name="enable-r"></a>Activer R

Pour utiliser l’extension Machine Learning pour la gestion des packages R dans votre base de données, procédez comme suit.

1. Vérifiez que **Machine Learning : Activer R** est activé. Ce paramètre est désactivé par défaut.

1. Indiquez le chemin d’accès à votre installation R préexistante sous **Machine Learning : Chemin R**. Il doit s’agir du chemin d’accès complet à l’exécutable R. 

## <a name="use-the-machine-learning-extension"></a>Utiliser l’extension Machine Learning

Pour utiliser l’extension Machine Learning dans Azure Data Studio, procédez comme suit.

1. Ouvrez la viewlet **Connexions** dans Azure Data Studio.

1. Cliquez avec le bouton droit sur votre serveur, puis sélectionnez **Gérer**.

1. Sélectionnez **Machine Learning** dans le menu de gauche sous **Général**.

Suivez les liens sous **Étapes suivantes** pour découvrir comment utiliser l’extension Machine Learning pour gérer les packages, effectuer des prédictions et importer des modèles dans votre base de données.

## <a name="next-steps"></a>Étapes suivantes

- [Gérer les packages dans la base de données](machine-learning-extension-manage-packages.md)
- [Effectuer des prédictions](machine-learning-extension-predictions.md)
- [Importer ou afficher des modèles](machine-learning-extension-import-view-models.md)
- [Notebooks dans Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentation sur SQL Machine Learning](../../machine-learning/index.yml)
- [Machine Learning et intelligence artificielle avec ONNX dans SQL Edge (préversion)](/azure/azure-sql-edge/onnx-overview)
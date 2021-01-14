---
title: Créer des extensions
description: Vous pouvez ajouter des fonctionnalités à Azure Data Studio à l’aide d’une extension. Découvrez comment créer une extension et la publier dans la galerie d’extensions.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/26/2020
ms.openlocfilehash: d23e37065eb18a7a22d5717a95379161c10870a0
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151235"
---
# <a name="extend-functionality-by-creating-azure-data-studio-extensions"></a>Étendre les fonctionnalités en créant des extensions Azure Data Studio

Les extensions dans Azure Data Studio offrent un moyen simple d’ajouter des fonctionnalités à l'installation d’Azure Data Studio de base.

Les extensions sont fournies par l’équipe Azure Data Studio (Microsoft), ainsi que par la communauté tierce (vous !).

## <a name="author-an-extension"></a>Créer une extension

Si vous souhaitez étendre Azure Data Studio, vous pouvez créer votre propre extension et la publier dans la galerie d’extensions.

### <a name="write-an-extension"></a>Écrire une extension

#### <a name="prerequisites"></a>Prérequis

Pour développer une extension, [Node.js](https://nodejs.org/) doit être installé et disponible dans votre `$PATH`. Node. js comprend npm, le gestionnaire de packages Node.js, qui est utilisé pour installer le générateur d’extensions.

Pour créer une extension, vous pouvez utiliser le générateur d’extensions Azure Data Studio. Le [générateur d’extensions](https://www.npmjs.com/package/generator-azuredatastudio) Yeoman est un bon point de départ pour les projets d’extension. Pour démarrer le générateur, entrez alors la commande suivante depuis l’invite de commandes :

```console
npm install -g yo generator-azuredatastudio
yo azuredatastudio
```

Pour obtenir un guide détaillé expliquant comment bien démarrer avec votre modèle d’extension, consultez l’article [extension de mappage de touches](keymap-extension.md), qui vous guide tout au long de la création d’une extension.

### <a name="extensibility-references"></a>Références d’extensibilité

Pour en savoir plus sur l’extensibilité d’Azure Data Studio, consultez [Vue d’ensemble de l’extensibilité](../extensibility.md). Vous pouvez également voir des exemples d’utilisation de l’API dans des [exemples](https://github.com/Microsoft/azuredatastudio/tree/main/samples) existants.

## <a name="debug-an-extension"></a>Déboguer une extension

Vous pouvez déboguer votre nouvelle extension à l’aide de l’extension Visual Studio Code [Azure Data Studio Debug](https://github.com/kevcunnane/sqlops-debug).

Pour déboguer votre extension :

1. Ouvrez votre extension avec [Visual Studio Code](https://code.visualstudio.com/).
2. Installez l’extension Azure Data Studio Debug.
3. Appuyez sur **F5** ou sélectionnez l’icône **Déboguer**, puis sélectionnez **Démarrer**.
4. Une nouvelle instance d’Azure Data Studio démarre en mode spécial (hôte de développement d’extension). Cette nouvelle instance est désormais consciente de votre extension.

## <a name="create-an-extension-package"></a>Créer un package d’extension

Après avoir écrit votre extension, vous devez créer un package VSIX pour l’installer dans Azure Data Studio. Vous pouvez utiliser [vscode-vsce](https://github.com/Microsoft/vscode-vsce) (Extensions Visual Studio Code) pour créer le package VSIX.

```console
npm install -g vsce
cd myExtensionName
vsce package
# The myExtensionName.vsix file has now been generated
```

Avec un package VSIX, vous pouvez partager votre extension localement et en privé en partageant le fichier .vsix et en utilisant la commande Extensions **: Installez à partir du fichier VSIX** à partir de la palette de commandes pour installer l’extension dans Azure Data Studio.

## <a name="publish-an-extension"></a>Publier une extension

Pour publier votre nouvelle extension sur Azure Data Studio :

1. Ajoutez vote extension à la [galerie d’extensions](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json).
2. Actuellement, nous ne prenons pas en charge les extensions tierces des hôtes. Au lieu de télécharger l’extension, Azure Data Studio a la possibilité d’accéder à une page de téléchargement. Pour définir une page de téléchargement pour votre extension, définissez la valeur de la ressource **Microsoft.AzureDataStudio.DownloadPage**.
3. Créez une demande de tirage (pull request) sur une branche release/extensions.
4. Envoyez une demande de révision à l’équipe.

Votre extension sera revue et ajoutée à la galerie d’extensions.

### <a name="publish-extension-updates"></a>Publier des mises à jour d’extension

Le processus de publication de mises à jour est similaire à la publication de l’extension. Vérifiez que la version est mise à jour dans package.json.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir des instructions pas à pas afin de bien démarrer, consultez à un des tutoriels de création d’extensions suivants :

- [Tutoriel sur l’extension Keymap](keymap-extension.md)
- [Tutoriel sur l’extension de tableau de bord](dashboard-extension.md)
- [Tutoriel sur l’extension de Notebook](notebook-extension.md)
- [Tutoriel sur l’extension Jupyter Book](jupyter-book-extension.md)
- [Tutoriel sur l’extension de l’Assistant](wizard-extension.md)

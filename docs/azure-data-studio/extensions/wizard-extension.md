---
title: Créer une extension de l’Assistant
description: Ce tutoriel montre comment créer une extension de l’Assistant pour ajouter des fonctionnalités personnalisées à Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: ef9938d87de3963ab7757e0d4ec3af0e01d6f33a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048359"
---
# <a name="create-an-azure-data-studio-wizard-extension"></a>Créer une extension de l’Assistant Azure Data Studio

Ce tutoriel montre comment créer une **extension de l’Assistant Azure Data Studio**. L’extension apporte un Assistant permettant d’interagir avec les utilisateurs dans Azure Data Studio.

Dans cet article, vous apprendrez comment :
> [!div class="checklist"]
> - Installer le générateur d’extensions
> - Créer votre extension
> - Ajouter un Assistant personnalisé à votre extension
> - Tester votre extension
> - Empaqueter votre extension
> - Publier votre extension sur le marketplace

## <a name="prerequisites"></a>Prérequis

Azure Data Studio repose sur la même infrastructure que Visual Studio Code, ainsi les extensions pour Azure Data Studio sont générées à l’aide de Visual Studio Code. Pour commencer, vous avez besoin des composants suivants :

- [Node.js](https://nodejs.org) installé et disponible dans votre `$PATH`. Node. js comprend [npm](https://www.npmjs.com/), le gestionnaire de packages Node.js, qui est utilisé pour installer le générateur d’extensions.
- [Visual Studio Code](https://code.visualstudio.com) pour déboguer l’extension.
- [L’extension de débogage](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) d’Azure Data Studio (facultatif). Cela vous permet de tester votre extension sans avoir à l’empaqueter et l’installer dans Azure Data Studio.
- Vérifiez que `azuredatastudio` se trouve dans votre chemin d’accès. Pour Windows, veillez à choisir l'option `Add to Path` dans setup.exe. Pour Mac ou Linux, exécutez **Installer la commande 'azuredatastudio' dans PATH** à partir de la palette de commandes dans Azure Data Studio.

## <a name="install-the-extension-generator"></a>Installer le générateur d’extensions

Pour simplifier le processus de création d’extensions, nous avons créé un [générateur d’extensions](https://code.visualstudio.com/docs/extensions/yocode) à l’aide de Yeoman. Pour l’installer, exécutez la commande suivante à partir de l’invite de commandes :

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-wizard-extension"></a>Créer votre extension de l’Assistant

### <a name="introduction-to-wizards"></a>Présentation des Assistants

Les Assistants sont un type d’interface utilisateur qui présente des pages successives que les utilisateurs doivent renseigner afin d’accomplir une tâche. Les exemples les plus courants incluent les Assistants d’installation de logiciels et de résolution des problèmes. Par exemple :

:::image type="content" source="media/wizard-extension/dacpac-wizard.gif" alt-text="Assistant Dacpac":::

Étant donné que les Assistants s’avèrent souvent utiles quand vous utilisez des données et étendez les fonctionnalités d’Azure Data Studio, Azure Data Studio propose des API pour créer vos propres Assistants personnalisés. Nous allons vous guider dans la génération d’un modèle d’Assistant et sa modification pour créer votre propre Assistant personnalisé.

### <a name="run-the-extension-generator"></a>Exécuter le générateur d’extensions

Pour créer une extension :

1. Lancez le générateur d’extensions avec la commande suivante :

   `yo azuredatastudio`

2. Choisissez **Nouvel Assistant ou dialogue** dans la liste des types d’extensions. Ensuite, sélectionnez **Assistant**, suivi de **Modèle de prise en main**.

3. Suivez les étapes pour renseigner le nom de l’extension (pour ce didacticiel, utilisez **My Test Extension**) et ajoutez une description.

    :::image type="content" source="media/wizard-extension/extension-generator.png" alt-text="Générateur d’extensions":::

L’exécution des étapes précédentes crée un dossier. Ouvrez le dossier dans Visual Studio Code et vous êtes prêt à créer votre propre extension de l’Assistant !

### <a name="run-the-extension"></a>Exécuter l’extension

Voyons ce que nous obtenons avec le modèle d’Assistant en exécutant l’extension. Avant l’exécution, vérifiez que l’**extension de débogage Azure Data Studio** est installée dans Visual Studio Code.

Appuyez sur **F5** dans VS Code pour lancer Azure Data Studio en mode débogage avec l’extension en cours d’exécution. Ensuite, dans Azure Data Studio, exécutez la commande **Démarrer l’Assistant** à partir de la palette de commandes (Ctrl+Maj+P) dans la nouvelle fenêtre. Cette opération lance l’Assistant par défaut que donne cette extension :

:::image type="content" source="media/wizard-extension/wizard-template.gif" alt-text="Modèle d’Assistant":::

Ensuite, nous allons voir comment modifier cet Assistant par défaut.

### <a name="develop-the-wizard"></a>Développer l’Assistant

Les fichiers les plus importants pour bien démarrer avec le développement d’extensions sont `package.json`, `src/main.ts` et `vsc-extension-quickstart.md` :

- `package.json`: Il s’agit du fichier manifeste, dans lequel la commande **Lancer l’Assistant** est inscrite. Il s’agit aussi de l’emplacement auquel `main.ts` est déclaré comme point d’entrée du programme principal.
- `main.ts`: Contient le code permettant d’ajouter des éléments d’interface utilisateur à l’Assistant, comme des pages, du texte et des boutons.
- `vsc-extension-quickstart.md`: Contient la documentation technique pouvant contenir des informations de référence utiles lors du développement.

Modifions l’Assistant : nous allons ajouter une quatrième page vierge. Modifiez `src/main.ts` comme indiqué ci-dessous. Une page supplémentaire doit apparaître quand vous lancez l’Assistant.

:::image type="content" source="media/wizard-extension/wizard-main.png" alt-text="Assistant principal":::
Une fois que vous connaissez le modèle, voici quelques idées supplémentaires à tester :

- Ajouter un bouton d’une largeur de 300 à votre nouvelle page
- Ajouter un composant flexible dans lequel placer votre bouton
- Ajouter une action à votre bouton Par exemple, quand vous cliquez sur le bouton, lancez un dialogue d’ouverture de fichier ou ouvrez un éditeur de requête.

## <a name="package-your-extension"></a>Empaqueter votre extension

Pour partager votre extension avec d’autres personnes, vous devez l’empaqueter dans un fichier unique. Il peut être publié sur le marketplace d’extensions Azure Data Studio ou partagé avec votre équipe ou votre communauté. Pour ce faire, vous devez installer un autre package npm à partir de la ligne de commande :

```console
npm install -g vsce
```

Modifiez le fichier `README.md` à votre gré, puis accédez au répertoire de base de l’extension et exécutez `vsce package`. Vous pouvez éventuellement lier un référentiel à votre extension ou continuer sans référentiel. Pour en ajouter un, ajoutez une ligne similaire à votre fichier `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Une fois ces lignes ajoutées, un fichier my-test-extension-0.0.1.vsix est créé et prêt à être installé dans Azure Data Studio.

:::image type="content" source="media/wizard-extension/install-vsix.png" alt-text="Installer VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>Publier votre extension sur le marketplace

Le marketplace d’extensions Azure Data Studio n’est pas encore totalement implémenté, mais le processus actuel consiste à héberger l’extension VSIX quelque part (par exemple une page de publication GitHub), puis à envoyer une demande de tirage mettant à jour [ce fichier JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) avec vos informations sur l’extension.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> - Installer le générateur d’extensions
> - Créer votre extension
> - Ajouter un Assistant personnalisé à votre extension
> - Tester votre extension
> - Empaqueter votre extension
> - Publier votre extension sur le marketplace

Nous espérons qu’après avoir lu cela, vous serez inspiré pour créer votre propre extension pour Azure Data Studio. Nous prenons en charge les insights de tableau de bord (de jolis graphiques qui s’exécutent sur SQL Server), un certain nombre d’API spécifiques à SQL et un ensemble de points d’extension existants hérités de Visual Studio Code.

Si vous avez une idée mais que vous ne savez pas par où commencer, veuillez ouvrir un problème ou envoyer un tweet à l’équipe [azuredatastudio](https://twitter.com/azuredatastudio).

Vous pouvez toujours vous reporter au [guide pour les extensions Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview), car il couvre toutes les API et tous les modèles existants.

Pour savoir comment utiliser T-SQL dans Azure Data Studio, suivez le didacticiel de l’éditeur T-SQL :

> [!div class="nextstepaction"]
> [Utiliser l’éditeur Transact-SQL pour créer des objets de base de données](../tutorial-sql-editor.md)

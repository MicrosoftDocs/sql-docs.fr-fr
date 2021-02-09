---
title: Créer une extension Jupyter Notebook
description: Découvrez comment empaqueter les notebooks dans une extension à l’aide du générateur d’extensions.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 36ed1466decd88686647302e65cf24746393d1f3
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495732"
---
# <a name="create-a-jupyter-notebook-extension"></a>Créer une extension Jupyter Notebook

Ce tutoriel montre comment créer une extension Azure Data Studio Jupyter Notebook. L’extension fournit un exemple Jupyter Notebook que vous pouvez ouvrir et exécuter dans Azure Data Studio.

Dans cet article, vous apprendrez comment :

> [!div class="checklist"]
> - Créer un projet d’extension.
> - Installer le générateur d’extension.
> - Créer votre extension de notebook.
> - Exécuter votre extension.
> - Empaqueter votre extension.
> - Publier votre extension sur le marketplace.

## <a name="apis-used"></a>API utilisées

- `azdata.nb.showNotebookDocument`

### <a name="extension-use-cases"></a>Cas d’usage d’extension

Il existe plusieurs raisons de créer une extension de notebook :
- Partager une documentation interactive
- Enregistrer ce notebook et pouvoir y accéder en permanence
- Indiquer des problèmes de codage à des utilisateurs pour qu’ils les suivent
- Versionner et suivre les mises à jour de notebooks

## <a name="prerequisites"></a>Prérequis

Azure Data Studio repose sur la même infrastructure que Visual Studio Code, ainsi les extensions pour Azure Data Studio sont générées à l’aide de Visual Studio Code. Pour commencer, vous avez besoin des composants suivants :

- [Node.js](https://nodejs.org) installé et disponible dans votre `$PATH`. Node. js comprend [npm](https://www.npmjs.com/), le gestionnaire de packages Node.js, qui est utilisé pour installer le générateur d’extensions.
- [Visual Studio Code](https://code.visualstudio.com) pour déboguer l’extension.
- Vérifiez que `azuredatastudio` se trouve dans votre chemin d’accès. Pour Windows, veillez à choisir l’option **Ajouter au chemin d’accès** dans setup.exe. Pour Mac ou Linux, exécutez **Installer la commande 'azuredatastudio' dans PATH** à partir de la palette de commandes dans Azure Data Studio.

## <a name="install-the-extension-generator"></a>Installer le générateur d’extensions

Pour simplifier le processus de création d’extensions, nous avons créé un [générateur d’extensions](https://www.npmjs.com/package/generator-azuredatastudio) à l’aide de Yeoman. Pour l’installer, exécutez la commande suivante à partir de l’invite de commandes :

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-extension"></a>Créer votre extension

Pour créer une extension :

1. Démarrez le générateur d’extension avec la commande suivante :

   `yo azuredatastudio`

2. Choisissez **Nouveaux notebooks (individuels)** dans la liste des types d’extensions.

   :::image type="content" source="media/notebook-extension/notebook-extension-generator.png" alt-text="Générateur d’extensions de notebook":::

3. Suivez les étapes pour remplir le nom d’extension. Pour ce tutoriel, utilisez **Test Notebook**. Puis, remplissez dans le nom de l’éditeur. Pour ce tutoriel, utilisez **Microsoft**. Ajoutez une description.

C’est là que se trouve une branche. Vous pouvez ajouter des Jupyter Notebooks que vous avez déjà créés, ou vous pouvez utiliser des échantillons de notebook fournis par le biais du générateur.

Pour ce tutoriel, nous allons utiliser un échantillon de notebook Python :

   :::image type="content" source="media/notebook-extension/notebook-sample-generator.png" alt-text="Sélectionner un exemple Python":::

Si vous avez des notebooks qui vous intéressent, vous pouvez répondre que vous avez déjà des notebooks à expédier. Indiquez le chemin d’accès absolu de tous vos notebooks ou fichiers Markdown.

Les étapes précédentes permettent de créer un dossier contenant l’exemple de notebook. Ouvrez le dossier dans Visual Studio Code et vous êtes prêt à distribuer votre nouvelle extension de notebook.

## <a name="understand-your-extension"></a>Comprendre votre extension

Voici ce à quoi votre projet devrait ressembler actuellement :

   :::image type="content" source="media/notebook-extension/notebook-file-structure-generator.png" alt-text="Structure de fichiers d’extension":::

Le fichier `vsc-extension-quickstart.md` sert de référence pour les fichiers importants. Le fichier `README.md` est l’emplacement sur lequel vous pouvez fournir de la documentation pour votre nouvelle extension. Remarquez les fichiers `package.json`, `notebook.ts` et `pySample.ipynb`.

Si vous ne souhaitez pas publier de fichiers ou de dossiers, vous pouvez inclure leur nom dans le fichier `.vscodeignore`.

Intéressons-nous au fichier `notebook.ts` pour comprendre à quoi sert notre extension nouvellement formée.

```javascript
// This function is called when you run the command `Launch Notebooks: Test Notebook` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchNotebooks.test-notebook', () => {
        let notebooksToDisplay: Array<string> = processNotebooks();
        notebooksToDisplay.forEach(name => {
            azdata.nb.showNotebookDocument(vscode.Uri.file(name));
        });
    }));

    // Add other code here if you want to register another command.
}
```

Il s’agit de la fonction principale dans `notebook.ts` qui est appelée chaque fois que nous exécutons l’extension par le biais de la commande **Launch Notebooks : Test Notebook**. Nous créons notre nouvelle commande à l’aide de l’API `vscode.commands.registerCommand`. La définition suivante à l’intérieur des accolades est le code qui s’exécute chaque fois que nous appelons notre commande. Chaque notebook trouvé à partir de notre fonction `processNotebooks` est ouvert dans Azure Data Studio à l’aide de `azdata.nb.showNotebookDocument`.

Le fichier `package.json` joue également un rôle important dans l’inscription de notre commande, **Launch Notebooks : Test Notebook**.

```json
"activationEvents": [
        "onCommand:launchNotebooks.test-notebook"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchNotebooks.test-notebook",
                "title": "Launch Notebooks: Test Notebook"
            }
        ]
    }
```

Nous avons un événement d’activation pour la commande et nous avons également ajouté des points de contribution spécifiques. Ceux-ci apparaissent dans le marketplace d’extension, où les extensions sont publiées, quand les utilisateurs examinent la vôtre. Si vous souhaitez ajouter d’autres commandes, veillez à les ajouter au champ `activationEvents`. Pour plus d’options, consultez [Événements d’activation](https://code.visualstudio.com/api/references/activation-events).

## <a name="package-your-extension"></a>Empaqueter votre extension

Pour partager votre extension avec d’autres personnes, vous avez besoin de l’empaqueter dans un fichier unique. Votre extension peut être publiée sur le marketplace d’extensions Azure Data Studio ou partagée avec votre équipe ou votre communauté. Pour ce faire, vous devez installer un autre package npm à partir de la ligne de commande.

```console
npm install -g vsce
```

Modifiez le fichier `README.md` à votre convenance. Puis, accédez au répertoire de base de l’extension, puis exécutez `vsce package`. Vous pouvez éventuellement lier un référentiel à votre extension ou continuer sans référentiel. Pour en ajouter un, ajoutez une ligne similaire à votre fichier `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testnotebook.git"
}
```

Une fois ces lignes ajoutées, un fichier `my test-notebook-0.0.1.vsix` est créé et prêt à être installé et partagé avec le monde entier.

## <a name="run-your-extension"></a>Exécuter votre extension

Pour exécuter et tester votre extension, ouvrez Azure Data Studio et ouvrez la palette de commandes en sélectionnant **Ctrl+Shift+P**. Recherchez la commande **Extensions : Installez à partir d’un VSIX** et accédez au dossier contenant votre nouvelle extension.

   :::image type="content" source="media/notebook-extension/install-vsix.png" alt-text="Installer VSIX":::

Votre extension doit maintenant apparaître dans le panneau d’extension d’Azure Data Studio. Rouvrez la palette de commandes et recherchez la nouvelle commande que nous avons créée avec notre extension, **Launch Book : Test Book**. Lors de son exécution, le Jupyter Book que nous avons empaqueté doit s’ouvrir avec notre extension.

   :::image type="content" source="media/notebook-extension/notebook-launch-ads.png" alt-text="Notebook-command":::

Félicitations ! Vous avez créé votre première extension Jupyter Notebook et vous pouvez maintenant la distribuer.

## <a name="publish-your-extension-to-the-marketplace"></a>Publier votre extension sur le marketplace

Le marketplace d’extension Azure Data Studio est en construction. Pour publier, hébergez l’extension VSIX quelque part, par exemple, sur une page de version GitHub. Ensuite, envoyez une demande de tirage (pull request) qui met à jour [ce fichier JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) avec vos informations d’extension.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> - Créer un projet d’extension.
> - Installer le générateur d’extension.
> - Créer votre extension de notebook.
> - Créer votre extension.
> - Empaqueter votre extension.
> - Publier votre extension sur le marketplace.

Nous espérons qu’après avoir lu cet article, vous serez inspiré pour créer votre propre extension pour Azure Data Studio.

Si vous avez une idée mais que vous ne savez pas par où commencer, ouvrez un problème ou envoyer un tweet à l’équipe [azuredatastudio](https://twitter.com/azuredatastudio).

Pour plus d’informations, le guide d’extension [Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) couvre l’ensemble des API et modèles existants.

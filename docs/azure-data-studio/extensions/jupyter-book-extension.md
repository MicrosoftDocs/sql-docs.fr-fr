---
title: Créer une extension Jupyter Book
description: Découvrez comment empaqueter un Jupyter Book dans une extension à l’aide du générateur d’extensions.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: aaa1357686217a37ca68acb61d4cff36c47d7da6
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495767"
---
# <a name="create-a-jupyter-book-extension"></a>Créer une extension Jupyter Book

Ce tutoriel montre comment créer une extension Azure Data Studio Jupyter Book. L’extension fournit un exemple Jupyter Book que vous pouvez ouvrir et exécuter dans Azure Data Studio.

Dans cet article, vous apprendrez comment :

> [!div class="checklist"]
> - Créer un projet d’extension.
> - Installer le générateur d’extension.
> - Créer votre extension Jupyter Book.
> - Exécuter votre extension.
> - Empaqueter votre extension.
> - Publier votre extension sur le marketplace.

## <a name="apis-used"></a>API utilisées

- `bookTreeView.openBook`

### <a name="extension-use-cases"></a>Cas d’usage d’extension

Il existe plusieurs raisons de créer une extension Jupyter Book :

- Partager une documentation interactive organisée et divisée en sections
- Partager un book complet (similaire à un e-book mais distribué via Azure Data Studio)
- Versionner et suivre les mises à jour de Jupyter Book

La principale différence entre un Jupyter Book et une extension de notebook est qu’un Jupyter Book vous fournit une organisation. Des dizaines de notebooks peuvent être répartis dans les différents chapitres d’un Jupyter Book, mais l’extension des notebooks a pour but de fournir un petit nombre de notebooks individuels.

## <a name="prerequisites"></a>Prérequis

Azure Data Studio repose sur la même infrastructure que Visual Studio Code, ainsi les extensions pour Azure Data Studio sont générées à l’aide de Visual Studio Code. Pour commencer, vous avez besoin des composants suivants :

- [Node.js](https://nodejs.org) installé et disponible dans votre `$PATH`. Node. js comprend [npm](https://www.npmjs.com/), le gestionnaire de packages Node.js, qui est utilisé pour installer le générateur d’extensions.
- [Visual Studio Code](https://code.visualstudio.com) pour apporter des modifications à votre extension et la déboguer.
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

1. Choisissez **Nouveau Jupyter Book** dans la liste des types d’extensions.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-extension-generator.png" alt-text="Capture d’écran qui montre le générateur d’extensions.":::

1. Suivez les étapes pour remplir le nom d’extension. Pour ce tutoriel, utilisez **Test Book**. Puis, remplissez dans le nom de l’éditeur. Pour ce tutoriel, utilisez **Microsoft**. Ajoutez une description.

Vous choisissez de fournir un Jupyter Book existant, d’utiliser un exemple fourni ou de créer un Jupyter Book. Les trois options sont affichées.

### <a name="provide-an-existing-book"></a>Fournir un book existant

Si vous voulez fournir un book que vous avez déjà créé, indiquez le chemin absolu du dossier dans lequel se trouve son contenu. Vous pouvez alors commencer à vous familiariser avec l’extension et sa distribution.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-existing-book.png" alt-text="Capture d’écran montrant un book existant.":::

### <a name="use-the-sample-book"></a>Utilisation de l’exemple de book

Si vous n’avez pas de books ou notebooks existants, vous pouvez utiliser l’exemple fourni dans le générateur.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-sample-path.png" alt-text="Capture d’écran montrant un exemple de Jupyter Book.":::

L’exemple de livre montre à quoi ressemble un Jupyter Book simple. Pour en savoir plus sur la personnalisation d’un Jupyter Book, consultez la section suivante sur la création d’un book avec des notebooks existants.

### <a name="create-a-new-book"></a>Créer un book

Si vous avez des notebooks que vous souhaitez empaqueter dans un Jupyter Book, c’est possible. Le générateur vous demande si vous voulez des chapitres dans votre book et, le cas échéant, leur nombre et leurs titres. Vous pouvez voir à quoi ressemble le processus de sélection ici. Utilisez la barre d’espace pour sélectionner les notebooks à placer dans chaque chapitre.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-create-book.png" alt-text="Capture d’écran montrant la création d’un Jupyter Book.":::

Les étapes précédentes permettent de créer un dossier contenant votre nouveau Jupyter Book. Ouvrez le dossier dans Visual Studio Code et vous êtes prêt à distribuer votre extension Jupyter Book.

## <a name="understand-your-extension"></a>Comprendre votre extension

Voici ce à quoi votre projet devrait ressembler actuellement :

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-file-structure-generator.png" alt-text="Capture d’écran montrant une structure de fichier d’extension.":::

Le fichier `vsc-extension-quickstart.md` sert de référence pour les fichiers importants. Le fichier `README.md` est l’emplacement sur lequel vous pouvez fournir de la documentation pour votre nouvelle extension. Remarquez les fichiers `package.json`, `jupyter-book.ts`, `content` et `toc.yml`. Le dossier `content` contient tous les fichiers Markdown et fichiers de notebook. Le fichier `toc.yml` structure votre Jupyter Book. Il est généré automatiquement si vous avez choisi de créer un Jupyter Book personnalisé par le biais du générateur d’extensions.

Si vous aviez créé un book à l’aide du générateur et choisi d’utiliser des chapitres dans votre book, votre structure de dossiers serait légèrement différente. Au lieu d’avoir vos fichiers Markdown et Jupyter Notebook dans le dossier `content`, vous auriez des sous-dossiers correspondant aux titres que vous avez choisis pour vos chapitres.

Si vous ne souhaitez pas publier de fichiers ou de dossiers, vous pouvez inclure leur nom dans le fichier `.vscodeignore`.

Intéressons-nous au fichier `jupyter-book.ts` pour comprendre à quoi sert notre extension nouvellement formée.

```javascript
// This function is called when you run the command `Launch Book: Test Book` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchBook.test-book', () => {
        processNotebooks();
    }));

    // Add other code here if you want to register another command.
}
```

La fonction `activate` est l’action principale de votre extension. Toutes les commandes à inscrire doivent apparaître à l’intérieur de la fonction `activate`, de la même façon que notre commande `launchBook.test-book`. À l’intérieur de la fonction `processNotebooks`, nous trouvons notre dossier d’extension qui contient notre Jupyter Book et nous appelons `bookTreeView.openBook` en utilisant le dossier de notre extension comme paramètre.

Le fichier `package.json` joue également un rôle important dans l’inscription de la commande de notre extension.

```json
"activationEvents": [
        "onCommand:launchBook.test-book"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchBook.test-book",
                "title": "Launch Book: Test Book"
            }
        ]
    }
```

L’événement d’activation, `onCommand`, déclenche la fonction que nous avons inscrite lors de l’appel de la commande. Il existe quelques autres événements d’activation possibles pour une personnalisation accrue. Pour plus d’informations, consultez [Événements d’activation](https://code.visualstudio.com/api/references/activation-events).

## <a name="package-your-extension"></a>Empaqueter votre extension

Pour partager votre extension avec d’autres personnes, vous avez besoin de l’empaqueter dans un fichier unique. Votre extension peut être publiée sur le marketplace d’extensions Azure Data Studio ou partagée avec votre équipe ou votre communauté. Pour ce faire, vous devez installer un autre package npm à partir de la ligne de commande.

```console
npm install -g vsce
```

Modifiez le fichier `README.md` à votre convenance. Puis, accédez au répertoire de base de l’extension, puis exécutez `vsce package`. Vous pouvez éventuellement lier un référentiel à votre extension ou continuer sans référentiel. Pour en ajouter un, ajoutez une ligne similaire à votre fichier `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testbook.git"
}
```

Une fois ces lignes ajoutées, un fichier `my test-book-0.0.1.vsix` est créé et prêt à être installé dans Azure Data Studio.

## <a name="run-your-extension"></a>Exécuter votre extension

Pour exécuter et tester votre extension, ouvrez Azure Data Studio et ouvrez la palette de commandes en sélectionnant **Ctrl+Shift+P**. Recherchez la commande **Extensions : Installez à partir d’un VSIX** et accédez au dossier contenant votre nouvelle extension. Celle-ci doit maintenant apparaître dans le panneau de votre extension dans Azure Data Studio.

   :::image type="content" source="media/jupyter-book-extension/install-vsix.png" alt-text="Capture d’écran montrant l’installation de VSIX.":::

Rouvrez la palette de commandes et recherchez la commande que nous avons inscrite, **Launch Book : Test Notebook**. Lors de son exécution, le Jupyter Book que nous avons empaqueté doit s’ouvrir avec notre extension.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-launch-ads.png" alt-text="Capture d’écran montrant la commande notebook.":::

Félicitations ! Vous avez créé votre première extension Jupyter Book et vous pouvez maintenant la distribuer. Pour plus d’informations sur Jupyter Book, consultez [Books with Jupyter](https://jupyterbook.org/intro.html).

## <a name="publish-your-extension-to-the-marketplace"></a>Publier votre extension sur le marketplace

Le marketplace d’extension Azure Data Studio est en construction. Pour publier, hébergez l’extension VSIX quelque part, par exemple, sur une page de version GitHub. Envoyez une demande de tirage qui met à jour [ce fichier JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) avec vos informations d’extension.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> - Créer un projet d’extension.
> - Installer le générateur d’extension.
> - Créer votre extension Jupyter Book.
> - Empaqueter votre extension.
> - Publier votre extension sur le marketplace.

Nous espérons que la lecture de cet article sur Jupyter Book vous aura donné envie de partager vos idées avec la communauté Azure Data Studio.

Si vous avez une idée mais que vous ne savez pas par où commencer, ouvrez un problème ou envoyer un tweet à l’équipe [azuredatastudio](https://twitter.com/azuredatastudio).

Pour plus d’informations, le guide d’extension [Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) couvre l’ensemble des API et modèles existants.

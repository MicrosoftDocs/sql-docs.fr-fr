---
title: Créer une extension de mappage de touches
description: Ce tutoriel montre comment créer une extension de mappage de touches pour ajouter des fonctionnalités personnalisées à Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 369685672921a62f44bb81387db0b987aa2e44dc
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495749"
---
# <a name="create-an-azure-data-studio-keymap-extension"></a>Créer une extension de mappage de touches Azure Data Studio

Ce didacticiel montre comment créer une extension Azure Data Studio. L’extension crée des liaisons de clé SSMS familières dans Azure Data Studio.

Dans cet article, vous apprendrez comment :
> [!div class="checklist"]
> - Créer un projet d’extension
> - Installer le générateur d’extensions
> - Créer votre extension
> - Ajouter des combinaisons de touches personnalisées à votre extension
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

Pour simplifier le processus de création d’extensions, nous avons créé un [générateur d’extensions](https://code.visualstudio.com/docs/extensions/yocode) à l’aide de Yeoman. Pour l’installer, exécutez le code dans l’invite de commandes ci-dessous :

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-keymap-extension"></a>Créer votre extension de mappage de touches

Pour créer une extension :

1. Lancez le générateur d’extensions avec la commande suivante :

   `yo azuredatastudio`

2. Choisissez **Nouveau mappage de touches** dans la liste des types d’extension :

   :::image type="content" source="media/keymap-extension/extension-generator.png" alt-text="Générateur d’extensions":::

3. Suivez les étapes pour renseigner le nom de l’extension (pour ce didacticiel, utilisez **ssmskeymap2**) et ajoutez une description.

L’exécution des étapes précédentes crée un dossier. Ouvrez le dossier dans Visual Studio Code et vous êtes prêt à créer votre propre extension de mappage de touches !

### <a name="add-a-keyboard-shortcut"></a>Ajouter un raccourci clavier

**Étape 1 : Rechercher les raccourcis à remplacer**

Maintenant que notre extension est prête à l’emploi, ajoutez des raccourcis clavier SSMS (ou combinaisons de touches) dans Azure Data Studio. J’ai utilisé la liste de raccourcis clavier de RedGate et [l’aide-mémoire d’Andy Mallon](https://am2.co/2018/02/updated-cheat-sheet/) à titre d’inspiration.

Les principaux éléments que je n’ai pas trouvés sont les suivants :

- Exécuter une requête avec le plan d’exécution réel activé. Le raccourci est **Ctrl+M** dans SSMS et n’a pas de liaison dans Azure Data Studio.
- Avec **Ctrl+Maj+E** comme deuxième méthode d’exécution d’une requête. Les commentaires utilisateur ont indiqué que cela manquait.
- Faire en sorte que **Alt+F1** exécute `sp_help`. Nous l’avons ajouté dans Azure Data Studio, mais étant donné que cette liaison était déjà en cours d’utilisation, nous l'avons mappée sur **Alt+F2** à la place.
- Activer/désactiver le mode plein écran (**Maj+Alt+Entrée**).
- **F8** pour afficher la vue **Explorateur d’objets** / **Serveurs.**

Il est facile de trouver et de remplacer ces combinaisons de touches. Exécutez *Ouvrir les raccourcis clavier* pour afficher l’onglet **Raccourcis clavier** dans Azure Data Studio, recherchez la *requête*, puis choisissez **Modifier la combinaison de touches**. Une fois que vous avez fini de modifier la combinaison de touches, vous pouvez voir le mappage mis à jour dans le fichier keybindings.json (exécutez *Ouvrir les raccourcis clavier* pour le voir).

:::image type="content" source="media/keymap-extension/keyboard-shortcuts.png" alt-text="Raccourcis clavier":::

:::image type="content" source="media/keymap-extension/key-bindings-json.png" alt-text="Extension Keybindings.json":::

**Étape 2 : Ajouter des raccourcis à l’extension**

Pour ajouter des raccourcis à l’extension, ouvrez le fichier *package.json* (dans l’extension) et remplacez la section `contributes` par le code ci-dessous :

```json
"contributes": {
  "keybindings": [
    {
      "key": "shift+cmd+e",
      "command": "runQueryKeyboardAction"
    },
    {
      "key": "ctrl+cmd+e",
      "command": "workbench.view.explorer"
    },
    {
      "key": "alt+f1",
      "command": "workbench.action.query.shortcut1"
    },
    {
      "key": "shift+alt+enter",
      "command": "workbench.action.toggleFullScreen"
    },
    {
      "key": "f8",
      "command": "workbench.view.connections"
    },
    {
      "key": "ctrl+m",
      "command": "runCurrentQueryWithActualPlanKeyboardAction"
    }
  ]
}
```

## <a name="test-your-extension"></a>Tester votre extension

Vérifiez que `azuredatastudio` se trouve dans votre chemin d’accès en exécutant la commande Installer azuredatastudio dans le CHEMIN D’ACCÈS dans Azure Data Studio.

Assurez-vous que l’extension de débogage Azure Data Studio est installée dans Visual Studio Code.

Appuyez sur **F5** pour lancer Azure Data Studio en mode débogage avec l’extension en cours d’exécution :

:::image type="content" source="media/keymap-extension/install-extension.png" alt-text="Installer l’extension":::

:::image type="content" source="media/keymap-extension/test-extension.png" alt-text="extension de test":::

Les mappages de clés sont une des extensions les plus rapides à créer. Ainsi, votre nouvelle extension devrait maintenant fonctionner correctement et être prête pour le partage.

## <a name="package-your-extension"></a>Empaqueter votre extension

Pour partager votre extension avec d’autres personnes, vous avez besoin de l’empaqueter dans un fichier unique. Ce fichier peut être publié sur le marketplace d’extensions Azure Data Studio ou partagé avec votre équipe ou votre communauté. Pour ce faire, vous devez installer un autre package npm à partir de la ligne de commande :

```console
npm install -g vsce
```

Accédez au répertoire de base de l’extension, puis exécutez `vsce package`. J’ai dû ajouter quelques lignes supplémentaires pour empêcher l'outil *vsce* de se plaindre :

```json
"repository": {
    "type": "git",
    "url": "https://github.com/kevcunnane/ssmskeymap.git"
},
"bugs": {
    "url": "https://github.com/kevcunnane/ssmskeymap/issues"
},
```

Une fois cette opération effectuée, mon fichier ssmskeymap-0.1.0.vsix a été créé et est prêt à être installé et partagé avec le monde entier !

:::image type="content" source="media/keymap-extension/extensions.png" alt-text="Installer":::

## <a name="publish-your-extension-to-the-marketplace"></a>Publier votre extension sur le marketplace

Le marketplace d’extension Azure Data Studio est en cours de construction, mais le processus actuel consiste à héberger l’extension VSIX quelque part (par exemple, une page de version GitHub), puis à envoyer une demande de mise à jour de la demande de tirage de [ce fichier JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) avec les informations de votre extension.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> - Créer un projet d’extension
> - Installer le générateur d’extensions
> - Créer votre extension
> - Ajouter des combinaisons de touches personnalisées à votre extension
> - Tester votre extension
> - Empaqueter votre extension
> - Publier votre extension sur le marketplace

Nous espérons qu’après avoir lu cela, vous serez inspiré pour créer votre propre extension pour Azure Data Studio. Nous prenons en charge les insights de tableau de bord (de jolis graphiques qui s’exécutent sur SQL Server), un certain nombre d’API spécifiques à SQL et un ensemble de points d’extension existants hérités de Visual Studio Code.

Si vous avez une idée mais que vous ne savez pas par où commencer, ouvrez un problème ou envoyer un tweet à l’équipe [azuredatastudio](https://twitter.com/azuredatastudio).

Vous pouvez toujours vous reporter au [guide pour les extensions Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview), car il couvre toutes les API et tous les modèles existants.

Pour savoir comment utiliser T-SQL dans Azure Data Studio, suivez le didacticiel de l’éditeur T-SQL :

> [!div class="nextstepaction"]
> [Utiliser l’éditeur Transact-SQL pour créer des objets de base de données](../tutorial-sql-editor.md)

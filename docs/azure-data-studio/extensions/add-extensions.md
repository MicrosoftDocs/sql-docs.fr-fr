---
title: Ajouter des extensions
description: Découvrez comment ajouter des fonctionnalités à Azure Data Studio en sélectionnant et en installant des extensions parmi celles fournies par Microsoft et les fournisseurs tiers.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 10/03/2019
ms.openlocfilehash: 3cc965ffd169d53e59706ca4ba68f2bcdddb5aa9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052119"
---
# <a name="extend-the-functionality-of-azure-data-studio"></a>Étendre les fonctionnalités d’Azure Data Studio

Les extensions dans Azure Data Studio offrent un moyen simple d’ajouter des fonctionnalités à l'installation d’Azure Data Studio de base.

Les extensions sont fournies par l’équipe Azure Data Studio (Microsoft), ainsi que par la communauté tierce (vous !). Pour plus d’informations sur la création d’extensions, consultez [Création d’une extension](./extension-authoring.md).

## <a name="add-azure-data-studio-extensions"></a>Ajouter des extensions Azure Data Studio

1. Accédez aux extensions disponibles en sélectionnant l’icône Extensions, ou en sélectionnant **Extensions** dans le menu **Affichage**. Vous pouvez utiliser la commande **Affichage : Afficher les extensions**, disponible dans la **palette de commandes** (F1 ou `Ctrl+Shift+P`).

    ![icône du gestionnaire d’extensions](media/add-extensions/extension-manager-icon.png)

    Vous pouvez également accéder rapidement au gestionnaire d’extensions en appuyant sur `Ctrl+Shift+X` (Windows/Linux) ou sur `Command+Shift+X` (Mac).

2. Sélectionnez une extension disponible pour afficher ses détails.

    ![détails de l’extension](media/add-extensions/extension-details.png)

3. Sélectionnez l’extension de votre choix et **installez-la**.

4. Une fois l’extension installée, sélectionnez **Recharger** pour l’activer dans Azure Data Studio (opération uniquement requise lors de la première installation d’une extension).

Si vous rencontrez des problèmes d’accès au gestionnaire des extensions sur Azure Data Studio, vous pouvez télécharger l’extension dont vous avez besoin sur notre [wiki GitHub](https://github.com/microsoft/azuredatastudio/wiki/List-of-Extensions).

## <a name="manage-extensions"></a>Gérer les extensions

### <a name="list-installed-extensions"></a>Lister les extensions installées

La vue Extensions par défaut affiche les extensions qui sont actuellement activées, toutes les extensions qui sont recommandées pour vous, ainsi qu’un conteneur réduit de toutes les extensions actuellement désactivées. La commande **Extensions : Afficher les extensions installées**, disponible dans la **palette de commandes** ou le menu déroulant **Autres actions** `(...)`, affiche une liste de toutes les extensions installées, y compris les extensions désactivées.

### <a name="uninstall-an-extension"></a>Désinstaller une extension

Pour désinstaller une extension, cliquez sur l’icône d’engrenage à droite d’une entrée d’extension, puis choisissez **Désinstaller** dans le menu déroulant. Cette opération désinstalle l’extension sélectionnée et vous invite à recharger Azure Data Studio.

 ![liste déroulante d’extension](media/add-extensions/extension-gear-dropdown.png)

### <a name="disable-an-extension"></a>Désactiver une extension

Vous pouvez désactiver temporairement une extension au lieu de la supprimer définitivement. Vous pouvez désactiver une extension dans toutes les sessions Azure Data Studio (**Désactiver**) ou uniquement pour votre espace de travail actuel (**Désactiver (Espace de travail)** ). Vous pouvez également désactiver toutes les extensions actuellement installées par le biais de la **palette de commandes** avec les commandes **Extensions : Désactiver toutes les extensions** et **Extensions : Désactiver toutes les extensions (Espace de travail)** .

### <a name="enable-an-extension"></a>Activation d’une extension

Si une extension a été désactivée, elle figure dans la section **Désactivées** de la liste d’extensions et est marquée comme **_Désactivée_ *. Vous pouvez la réactiver avec la commande* Activer** ou **Activer (Espace de travail**) du menu déroulant. La **palette de commandes** permet également d’activer toutes les extensions avec les commandes **Extensions : Activer toutes les extensions** et **Extensions : Activer toutes les extensions (Espace de travail)** .

![activation d’une extension](media/add-extensions/extensions-enable.png)

### <a name="updating-an-extension"></a>Mise à jour d’une extension

Azure Data Studio recherche et installe automatiquement les mises à jour des extensions installées. Si vous souhaitez désactiver la fonctionnalité de mise à jour automatique, vous pouvez le faire avec la commande **Extensions : Désactiver la mise à jour automatique des extensions**.

Pour mettre manuellement à jour une extension, vous pouvez rechercher les mises à jour d’extension avec la commande **Extensions : Affichez les extensions obsolètes**, qui effectue une recherche dans votre liste d’extensions à l’aide du filtre `@outdated`. Cette opération affiche les mises à jour disponibles pour toutes les extensions installées. Cliquez sur le bouton **Mettre à jour** sur une extension obsolète et la mise à jour sera installée. Vous serez alors invité à recharger Azure Data Studio. Vous pouvez également mettre à jour toutes vos extensions obsolètes simultanément avec la commande **Extensions : Mettre à jour toutes les extensions**.

La commande **Extensions : Rechercher les mises à jour des extensions** est une autre façon de vérifier quelles extensions ont des mises à jour disponibles.

## <a name="install-from-a-vsix"></a>Installer à partir d’un VSIX

Vous pouvez installer manuellement une extension Azure Data Studio empaquetée dans un fichier `.vsix` à l’aide de la commande **Installer depuis un VSIX** qui figure dans la liste déroulante des commandes de la vue Extensions, ou en sélectionnant la commande **Extensions : Installer depuis un VSIX** dans la palette de commandes et en pointant sur le fichier `.vsix` de l’extension.

## <a name="access-installed-azure-data-studio-extensions"></a>Accéder aux extensions Azure Data Studio installées

Chaque extension améliore votre expérience d’Azure Data Studio d’une manière différente. Ainsi, le point d’entrée pour les extensions peut varier. Reportez-vous à la documentation propre à l’extension installée pour plus d’informations sur la façon dont vous pouvez accéder à ses fonctionnalités.

## <a name="extensions-view-filters"></a>Filtres de la vue Extensions

La zone de recherche de la vue Extensions prend en charge les filtres pour vous aider à trouver et à gérer les extensions. Les commandes **Afficher les extensions installées** et **Afficher les extensions recommandées** utilisent des filtres tels que `@installed` et `@recommended` dans la zone de recherche.

Vous pouvez consulter la liste complète de tous les filtres et commandes de tri en tapant @ dans la zone de recherche des extensions et en parcourant les suggestions :

![Tri des extensions](media/add-extensions/extension-sort.png)

Voici les filtres de la vue Extensions :

- `@builtin` : afficher les extensions fournies avec Azure Data Studio, groupées par type (langages de programmation, thèmes, etc.).
- `@disabled` : afficher les extensions installées désactivées.
- `@enabled` : afficher les extensions installées activées. Les extensions peuvent être activées ou désactivées individuellement.
- `@installed` : afficher les extensions installées.
- `@outdated` : afficher les extensions installées obsolètes. Une version plus récente est disponible sur la Place de marché.
- `@recommended` : afficher les extensions recommandées, regroupées comme propres à l’espace de travail ou pour utilisation générale.
- `@category` : afficher les extensions appartenant à la catégorie spécifiée. Vous trouverez ci-dessous quelques-unes des catégories prises en charge. Pour obtenir la liste complète, tapez @category et suivez les options de la liste de suggestions :
    - `@category:themes`
    - `@category:formatters`
    - `@category:snippets` Ces filtres peuvent également être combinés. Par exemple, `@installed @category:themes` affiche tous les thèmes installés.

Si aucun filtre n’est fourni, la vue Extensions affiche les extensions actuellement installées et recommandées.

### <a name="sorting"></a>Tri

Vous pouvez trier les extensions avec le filtre `@sort`, qui peut prendre les valeurs suivantes :

- `installs` : trier selon le nombre d’installations à partir de la Galerie d’extensions, par ordre décroissant.
- `rating` : trier selon l’évaluation dans la Galerie d’extensions (1-5 étoiles), par ordre décroissant.
- `name` : trier par ordre alphabétique par nom d’extension.

## <a name="common-questions"></a>Questions courantes

### <a name="where-are-extensions-installed"></a>Où les extensions sont-elles installées ?

Les extensions sont installées dans un dossier d’extensions par utilisateur. Selon votre plateforme, l’emplacement se trouve dans le dossier suivant :

- Windows `%USERPROFILE%\.azuredatastudio\extensions`
- macOS `~/.azuredatastudio/extensions`
- Linux `~/.azuredatastudio/extensions`

---
title: Terminal intégré
description: Découvrez comment ouvrir un terminal intégré à Azure Data Studio. Un terminal intégré peut être plus pratique qu’un terminal séparé.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 054bfd71962a5775374e8faa567dc24242d846c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048339"
---
# <a name="integrated-terminal"></a>Terminal intégré

Dans Azure Data Studio, vous pouvez ouvrir un terminal intégré, commençant à la racine de votre espace de travail. Cela peut être pratique, car vous n’avez pas besoin de changer de fenêtre ou de modifier l’état d’un terminal existant pour exécuter une tâche de ligne de commande rapide.

Pour ouvrir le terminal :

* Utilisez le raccourci clavier **Ctrl+'** avec le caractère accent grave.
* Utilisez la commande de menu **Affichage** | **Terminal intégré**.
* À partir de la **palette de commandes** (**Ctrl+Maj+P**), utilisez la commande **Affichage : activer/désactiver le terminal intégré**.

![Terminal](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> Vous pouvez toujours ouvrir un interpréteur de commandes externe à l’aide de la commande **Ouvrir dans une invite de commandes** de l’Explorateur (**Ouvrir dans Terminal** sur Mac ou Linux) si vous préférez travailler en dehors d’Azure Data Studio.

## <a name="managing-multiple-terminals"></a>Gestion de plusieurs terminaux

Vous pouvez créer plusieurs terminaux ouverts à différents emplacements et naviguer facilement entre eux. Vous pouvez ajouter des instances de terminal en appuyant sur l’icône le plus en haut à droite du panneau **TERMINAL** ou en déclenchant la commande **Ctrl+Maj+`** . Cela crée une autre entrée dans la liste déroulante qui peut être utilisée pour basculer entre elles.

![Plusieurs terminaux](media/integrated-terminal/terminal-multiple-instances.png)

Pour supprimer des instances de terminal, appuyez sur le bouton de corbeille.

> [!TIP]
> Si vous utilisez plusieurs terminaux de manière intensive, vous pouvez ajouter des combinaisons de touches pour les commandes `focusNext`, `focusPrevious` et `kill` décrites dans la [section Combinaisons de touches](#key-bindings) pour permettre la navigation entre elles en utilisant uniquement le clavier.

## <a name="configuration"></a>Configuration

L’interpréteur de commandes utilisait des valeurs par défaut de `$SHELL` sur Linux et macOS, PowerShell sur Windows 10 et cmd.exe sur les versions antérieures de Windows. Elles peuvent être remplacées manuellement en définissant `terminal.integrated.shell.*` dans les [paramètres](settings.md). Les arguments peuvent être passés à l’interpréteur de commandes Terminal sur Linux et macOS à l’aide des paramètres `terminal.integrated.shellArgs.*`.

### <a name="windows"></a>Windows

La configuration correcte de votre interpréteur de commandes sur Windows consiste à localiser le bon exécutable et à mettre à jour le paramètre. Vous trouverez ci-dessous une liste des exécutables d’interpréteur de commandes courants et leurs emplacements par défaut :

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> Pour pouvoir être utilisé en tant que terminal intégré, l’exécutable d’interpréteur de commandes doit être une application console pour permettre la redirection de `stdin/stdout/stderr`.

> [!TIP]
> L’interpréteur de commandes du terminal intégré s’exécute avec les autorisations d’Azure Data Studio. Si vous devez exécuter une commande d’interpréteur de commandes avec élévation de privilèges (administrateur) ou des autorisations différentes, vous pouvez utiliser des utilitaires de plateforme tels que `runas.exe` dans un terminal.

### <a name="shell-arguments"></a>Arguments de l’interpréteur de commandes

Vous pouvez passer des arguments à l’interpréteur de commandes lorsqu’il est lancé.

Par exemple, pour permettre l’exécution de bash en tant qu’interpréteur de commandes de connexion (qui exécute `.bash_profile`), passez l’argument `-l` (avec des guillemets doubles) :

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>Paramètres d’affichage du terminal

Vous pouvez personnaliser la police et la hauteur de ligne du terminal intégré avec les paramètres suivants :

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a name="terminal-key-bindings"></a><a id="key-bindings"></a>Combinaisons de touches du terminal

La commande **Affichage : Activer/désactiver le terminal intégré** est liée à **Ctrl+`** pour activer et désactiver rapidement l’affichage du panneau du terminal intégré.

Vous trouverez ci-dessous les raccourcis clavier permettant de naviguer rapidement au sein du terminal intégré :

|Clé|Commande|  
|---|---|  
|**Ctrl+\`**|Afficher le terminal intégré|  
|**Ctrl+Maj+\`**|Créer un nouveau terminal|  
|**Ctrl+Haut**|Faire défiler vers le haut|  
|**Ctrl+Bas**|Faire défiler vers le bas|  
|**Ctrl+Page préc**|Faire défiler la page vers le haut|  
|**Ctrl+Page suiv**|Faire défiler la page vers le bas|  
|**Ctrl+Origine**|Faire défiler vers le sommet|  
|**Ctrl+Fin**|Faire défiler à la fin|  
|**Ctrl+K**|Effacer le terminal|  

D’autres commandes de terminal sont disponibles et peuvent être liées à vos raccourcis clavier préférés.

Il s'agit de :

* `workbench.action.terminal.focus`: Focalisation sur le terminal. Fonctionne comme l’activation/la désactivation, mais en se focalisant sur le terminal au lieu de le masquer s’il est visible.
* `workbench.action.terminal.focusNext`: Se focalise sur l’instance de terminal suivante.
* `workbench.action.terminal.focusPrevious`: Se focalise sur l’instance de terminal précédente.
* `workbench.action.terminal.kill`: Supprime l’instance de terminal actuelle.
* `workbench.action.terminal.runSelectedText`: Exécute le texte sélectionné dans l’instance de terminal.
* `workbench.action.terminal.runActiveFile`: Exécute le fichier actif dans l’instance de terminal.

### <a name="run-selected-text"></a>Exécuter le texte sélectionné

Pour utiliser la commande `runSelectedText`, sélectionnez du texte dans un éditeur et exécutez la commande **Terminal : Exécuter le texte sélectionné dans le terminal actif** à l’aide de la **Palette de commandes** (**Ctrl+Maj+P**). Le terminal tente d’exécuter le texte sélectionné :

![Exécuter le texte sélectionné](media/integrated-terminal/terminal_run_selected.png)

Si aucun texte n’est sélectionné dans l’éditeur actif, la ligne sur laquelle se trouve le curseur est exécutée dans le terminal.

### <a name="copy--paste"></a>Copier et coller

Les combinaisons de touches pour copier et coller suivent les normes de la plateforme :

* Linux : **Ctrl+Maj+C** et **Ctrl+Maj+V**
* Mac : **Cmd+C** et **Cmd+V**
* Windows : **Ctrl+C** et **Ctrl+V**

### <a name="find"></a>Rechercher

Le terminal intégré dispose d’une fonctionnalité de recherche de base qui peut être déclenchée avec **Ctrl+F**.

Si vous souhaitez que **Ctrl+ F** accède à l’interpréteur de commandes au lieu de lancer le widget Rechercher sur Linux et Windows, vous devez supprimer la combinaison de touches comme suit :

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>Renommer les sessions de terminal

Les sessions de terminal intégrées peuvent maintenant être renommées à l’aide de la commande **Terminal : Renommer** (`workbench.action.terminal.rename`). Le nouveau nom s’affiche dans la liste déroulante de sélection de terminal.

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>Forcer les combinaisons de touches à traverser le terminal

Bien que la focalisation porte sur le terminal intégré, de nombreuses combinaisons de touches ne fonctionneront pas, car les séquences de touches sont transmises à et consommées par le terminal lui-même. Le paramètre `terminal.integrated.commandsToSkipShell` peut être utilisé pour contourner ce problème. Il contient un tableau de noms de commande dont les combinaisons de touches échappent au traitement par l’interpréteur de commandes et sont à la place traitées par le système de combinaisons de touches d’Azure Data Studio. Par défaut, toutes les combinaisons de touches de terminal sont incluses, en plus d’une sélection de combinaisons de touches couramment utilisées.


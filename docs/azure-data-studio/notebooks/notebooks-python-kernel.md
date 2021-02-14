---
title: Notebooks avec noyau Python dans Azure Data Studio
description: Ce tutoriel vous montre comment créer et exécuter un notebook Python.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: garyericson
ms.author: garye
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: e1859c5e1af823e067210c5b69247702e8c78f89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040309"
---
# <a name="create-and-run-a-python-notebook"></a>Créer et exécuter un notebook Python

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel montre comment créer et exécuter un notebook dans Azure Data Studio à l’aide du noyau Python.

## <a name="prerequisites"></a>Prérequis

- [Azure Data Studio installé](../download-azure-data-studio.md)

## <a name="create-a-notebook"></a>Créer un notebook

Les étapes suivantes montrent comment créer un fichier de notebook dans Azure Data Studio :

1. Ouvrez Azure Data Studio. Si vous êtes invité à vous connecter à un serveur SQL Server, vous pouvez le faire ou cliquer sur **Annuler**.

1. Sélectionnez **Nouveau notebook** dans le menu **Fichier**.

1. Sélectionnez **Python 3** comme **Noyau**. **Attacher à** a la valeur « localhost ».

   :::image type="content" source="media/notebooks-python-kernel/set-kernel-and-attach-to-python.png" alt-text="Définir le noyau":::

Vous pouvez enregistrer le notebook à l’aide de la commande **Enregistrer** ou **Enregistrer sous** du menu **Fichier**.

Pour ouvrir un notebook, vous pouvez utiliser la commande **Ouvrir le fichier** du menu **Fichier**, sélectionner **Ouvrir le fichier** dans la page **Bienvenue**, ou utiliser la commande **Fichier : Ouvrir** dans la palette de commandes.

## <a name="change-the-python-kernel"></a>Changer le noyau Python

La première fois que vous vous connectez au noyau Python dans un notebook, la page **Configurer Python pour Notebooks** s’affiche. Vous pouvez sélectionner l’une des options suivantes :

- **Nouvelle installation de Python** pour installer une nouvelle copie de Python pour Azure Data Studio
- **Utiliser l’installation existante de Python** pour indiquer à Azure Data Studio le chemin à une installation existante de Python

Pour voir l’emplacement et la version du noyau Python actif, créez une cellule de code et exécutez les commandes Python suivantes :

```python
import os
import sys
print(sys.version_info)
print(os.path.dirname(sys.executable))
```

Pour vous connecter à une autre installation de Python :

1. Dans le menu **Fichier**, sélectionnez **Préférences**, puis **Paramètres**.
1. Faites défiler jusqu’à **Configuration de notebook** sous **Extensions**.
1. Sous **Utiliser l’installation existante de Python**, décochez l’option « Chemin local d’une installation précédente de Python utilisée par Notebooks ».
1. Redémarrez Azure Data Studio.

Lorsque Azure Data Studio démarre et que vous vous connectez au noyau Python, la page **Configurer Python pour Notebooks** s’affiche. Vous pouvez choisir de créer une installation Python ou de spécifier le chemin d’une installation existante.

## <a name="run-a-code-cell"></a>Exécuter une cellule de code

Vous pouvez créer des cellules contenant du code SQL que vous pouvez exécuter sur place en cliquant sur le bouton **Exécuter la cellule** (flèche noire ronde) à gauche de la cellule. Les résultats sont affichés dans le notebook après la fin de l’exécution de la cellule.

Par exemple :

1. Ajoutez une nouvelle cellule de code Python en sélectionnant la commande **+ Code** dans la barre d’outils.

   :::image type="content" source="media/notebooks-python-kernel/notebook-toolbar-python.png" alt-text="Barre d’outils du notebook":::

1. Copiez et collez l’exemple suivant dans la cellule, puis cliquez sur **Exécuter la cellule**. Cet exemple effectue une opération mathématique simple et le résultat apparaît en dessous.

   ```python
   a = 1
   b = 2
   c = a/b
   print(c)
   ```

   :::image type="content" source="media/notebooks-python-kernel/run-notebook-cell-python.png" alt-text="Exécuter la cellule du notebook":::

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur les notebooks :

- [Étendre Python avec Kqlmagic](./notebooks-kqlmagic.md)
- [Guide pratique pour utiliser les notebooks dans Azure Data Studio](./notebooks-guidance.md)
- [Créer et exécuter un notebook SQL Server](./notebooks-sql-kernel.md)

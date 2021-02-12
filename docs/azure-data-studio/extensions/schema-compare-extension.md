---
title: Extension de comparaison de schéma
description: Découvrez comment installer et utiliser l’extension Comparer les schémas d’Azure Data Studio pour comparer facilement deux bases de données et en modifier une de manière sélective pour qu’elle corresponde à l’autre.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: 88f96d75150fc1b3e69d9cf158a50e02110dbae0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048476"
---
# <a name="schema-compare-extension"></a>Extension de comparaison de schéma

L’extension de comparaison de schémas fournit une expérience facile à utiliser pour comparer deux définitions de base de données et appliquer les différences de la source à la cible.

## <a name="features"></a>Fonctionnalités

* Comparer les schémas de deux bases de données ou fichiers dacpac
* Afficher les résultats sous la forme d’un ensemble d’actions qui doivent être effectuées sur la cible pour qu’elle corresponde à la source
* Exclure de manière sélective des actions listées dans les résultats
* Définir les options qui contrôlent l’étendue de la comparaison
* Appliquer les modifications à la cible ou générer un script avec le même effet
* Enregistrer la comparaison

![Comparaison de schémas : exemple de comparaison](media/schema-compare-extension/schema-compare.png)

## <a name="why-would-i-use-the-schema-compare-extension"></a>Pourquoi utiliser l’extension de comparaison de schémas ?

Il peut être fastidieux de gérer et de synchroniser manuellement différentes versions de base de données. L’extension de comparaison de schémas simplifie le processus de comparaison des bases de données et vous donne un contrôle total lors de leur synchronisation &mdash; vous pouvez filtrer de manière sélective des différences et des catégories de différences spécifiques avant d’appliquer les modifications. L’extension de comparaison de schémas est un outil fiable qui vous permet d’économiser du temps et du code.

![Comparaison de schémas : boîte de dialogue des options](media/schema-compare-extension/schema-compare-options.png)

## <a name="install-the-extension"></a>Installer l’extension

1. Sélectionnez l’icône Extensions pour voir les extensions disponibles.

    ![icône du gestionnaire d’extensions](media/add-extensions/extension-manager-icon.png)

2. Recherchez l’extension **Comparaison de schémas** et sélectionnez-la pour afficher ses détails. Sélectionnez **Installer** pour ajouter l’extension.

3. Une fois l’extension installée, sélectionnez **Recharger** pour l’activer dans Azure Data Studio (opération uniquement requise lors de la première installation d’une extension).

## <a name="launch-a-schema-compare"></a>Lancer une comparaison de schémas

1. Pour ouvrir la boîte de dialogue Comparaison de schémas, **cliquez avec le bouton droit** sur une base de données dans l’Explorateur d'objets, puis sélectionnez **Comparaison de schémas**. La base de données que vous sélectionnez est définie en tant que base de données source dans la comparaison.

    ![menu de lancement de la comparaison de schémas](media/schema-compare-extension/schema-compare-launch.png)

2. Sélectionnez l’un des boutons représentant des points de suspension (...) pour changer la source et la cible de votre comparaison de schémas, puis sélectionnez OK.

    ![comparaison de schémas, sélection de la source/cible](media/schema-compare-extension/schema-compare-select-source-target.png)

3. Pour personnaliser votre comparaison, sélectionnez le bouton **Options** dans la barre d’outils.

4. Cliquez sur **Comparer** pour afficher les résultats de la comparaison.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la comparaison de schéma, consultez [Utiliser la comparaison de schéma pour comparer différentes définitions de base de données](../../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).
Signalez les problèmes et les demandes de fonctionnalités [ici.](https://github.com/microsoft/azuredatastudio/issues)
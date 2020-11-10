---
description: 'Solution de contournement pour autoriser la copie à partir de la fenêtre Rechercher et remplacer '
title: Autoriser la copie sur la fenêtre Rechercher et remplacer
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: ea5ae3f9fa941e723d3bdf10de0ec900310cc28d
ms.sourcegitcommit: 985e2e8e494badeac6d6b652cd35765fd9c12d80
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328773"
---
# <a name="workaround-to-enable-copying-from-find-and-replace-window"></a>Solution de contournement pour autoriser la copie à partir de la fenêtre Rechercher et remplacer

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Une solution de contournement est nécessaire pour autoriser la copie à partir de la fenêtre Rechercher et remplacer.  Si vous ne parvenez pas à effectuer une copie à partir de la fenêtre Rechercher et remplacer dans SQL Server Management Studio, suivez la [solution de contournement](#workaround) ci-dessous.

## <a name="error-message"></a>Message d’erreur

Quand vous tentez de copier du texte à partir de la fenêtre Rechercher et remplacer dans SQL Server Management Studio, vous obtenez un message d’erreur.

> Impossible de couper ou copier les documents non enregistrés entre le Presse-papiers et le projet Fichiers divers. Vous devez les enregistrer avant de les couper ou copier.

![Boîte de dialogue d’erreur pour : Impossible de couper ou copier les documents non enregistrés entre le Presse-papiers et le projet Fichiers divers. Vous devez les enregistrer avant de les couper ou copier.](../media/troubleshoot/unable-copy-find-replace-window.png)

## <a name="workaround"></a>Solution de contournement

Pour autoriser la copie de texte à partir de la fenêtre Rechercher et remplacer, effectuez les étapes suivantes :

1. Dans le menu **Outils** , ouvrez **Options**.

2. Sous **Environnement**>**Documents** , décochez l’élément « Afficher les fichiers divers dans l’Explorateur de solutions ».

3. Fermez et rouvrez SQL Server Management Studio.

![Fenêtre Options avec l’option « Afficher les fichiers divers dans l’Explorateur de solutions » décochée](../media/troubleshoot/fix-copy-find-replace-window.png)


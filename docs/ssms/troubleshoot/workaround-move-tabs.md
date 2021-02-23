---
description: Solution de contournement pour déplacer les onglets
title: Permettre aux onglets de se déplacer sans entraîner d’incident dans SSMS
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
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654630"
---
# <a name="workaround-to-move-tabs"></a>Solution de contournement pour déplacer les onglets

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Une solution de contournement peut être nécessaire pour activer le déplacement des onglets de l’éditeur de requête, que ce soit dans la fenêtre ou pour ancrer un onglet précédemment supprimé.  Si vous avez appliqué toutes les mises à jour Windows disponibles et que vous ne parvenez pas à manipuler les onglets de l’éditeur de requête sans rencontrer d’incident, adoptez la [solution de contournement](#workaround) ci-dessous.

## <a name="applicable-environments"></a>Environnements applicables
Les mises à jour Windows du .NET Framework ont introduit un problème connu, qui entraîne un incident d’application pour SQL Server Management Studio (SSMS) lors de l’ancrage d’onglets ou du fractionnement de la fenêtre.  Les dernières informations se trouvent dans les [commentaires des utilisateurs sur SQL Server](https://feedback.azure.com/forums/908035/suggestions/42651556).

## <a name="workaround"></a>Solution de contournement

Si l’incident persiste après l’application de toutes les mises à jour Windows disponibles, procédez comme suit pour atténuer le problème :

1. Fermez toutes les instances de SQL Server Management Studio (SSMS).

2. Recherchez votre fichier d’application SSMS (exe).  Celui-ci se trouve généralement dans `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE`.

3. Ouvrez le fichier `Ssms.exe.config` dans le Bloc-notes en tant qu’administrateur.

4. Recherchez le nœud `AppContextSwitchOverrides` et ajoutez ces deux propriétés à la valeur.
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![Modifiez ssms.exe.config.](../media/troubleshoot/execonfig-edit.png)

5. Enregistrez le fichier config et rouvrez SSMS.

---
title: Gérer la mise en forme du code
description: Découvrez les trois styles de mise en retrait du texte et comment les utiliser, et apprenez à créer des URL interactives.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- indenting code [SQL Server]
- displaying URLs
- code formatting [SQL Server Management Studio]
- collapsing text
- formats [SQL Server], code formatting in SQL Server Management Studio
- hiding text
- formats [SQL Server]
- text [SQL Server], code formats
- automatic indentation
- converting text to lower case
- Query Editor [SQL Server Management Studio], managing code formats
- URL displayed in code [SQL Server Management Studio]
- converting text to upper case
- text [SQL Server]
- unindenting code
ms.assetid: ddbac4d2-6bdc-4467-a352-e869ec880eed
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 550f34f022262b24cd7992c7b3ebe056b946f4cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354619"
---
# <a name="manage-code-formatting"></a>Gérer la mise en forme du code

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Dans l'éditeur, vous pouvez mettre en forme votre code avec des mises en retrait, du texte caché, des URL, etc. Vous pouvez également mettre en forme automatiquement votre code à l'aide de la fonction de mise en retrait intelligente.  
  
## <a name="indenting"></a>Mise en retrait  
 Vous avez le choix entre trois styles de mise en retrait du texte. Vous pouvez également spécifier le nombre d'espaces correspondant à une mise en retrait ou une tabulation, et déterminer si l'Éditeur de code applique la mise en retrait au moyen de tabulations ou d'espaces.  
  
#### <a name="to-choose-an-indenting-style"></a>Pour choisir un style de mise en retrait  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Cliquez sur le dossier, puis sélectionnez **Tous les langages** pour définir la mise en retrait pour tous les langages.  
  
4.  Cliquez sur **Tabulations**.  
  
5.  Cliquez sur l'une des options suivantes :  
  
    -   **Aucun**. Le curseur se positionne au début de la ligne suivante.  
  
    -   **Bloquer**. Le curseur aligne la ligne suivante sur la ligne précédente.  
  
    -   **Intelligente** (par défaut). Le service du langage détermine le style de mise en retrait approprié à utiliser.  
  
    > [!NOTE]  
    >  Certains langages n'offrent pas toutes ces options de mise en retrait.  
  
#### <a name="to-change-indent-tab-settings"></a>Pour modifier les paramètres des tabulations de mise en retrait  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Sélectionnez le dossier pour **Tous les langages** pour définir la mise en retrait pour tous les langages.  
  
4.  Cliquez sur **Tabulations**.  
  
5.  Pour spécifier des caractères de tabulation pour les opérations de tabulation et de mise en retrait, cliquez sur **Conserver les tabulations**. Pour spécifier des espaces, sélectionnez **Insérer des espaces**.  
  
     Si vous sélectionnez **Insérer des espaces**, vous pouvez entrer le nombre d’espaces représenté par chaque tabulation ou mise en retrait, respectivement sous **Taille des tabulations** ou **Taille du retrait**.  
  
#### <a name="to-indent-code"></a>Pour mettre du code en retrait  
  
1.  Sélectionnez le texte que vous souhaitez mettre en retrait.  
  
2.  Appuyez sur TAB ou cliquez sur le bouton **Retrait** dans la barre d’outils Standard.  
  
#### <a name="to-unindent-code"></a>Pour annuler la mise en retrait du code  
  
1.  Sélectionnez le texte dont vous souhaitez annuler la mise en retrait.  
  
2.  Appuyez sur Maj+Tab ou cliquez sur le bouton **Annuler la mise en retrait** dans la barre d’outils Standard.  
  
#### <a name="to-automatically-indent-all-of-your-code"></a>Pour mettre automatiquement en retrait la totalité du code  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Cliquez sur **Tous les langages**.  
  
4.  Cliquez sur **Tabulations**.  
  
5.  Cliquez sur **Intelligente**.  
  
> [!NOTE]  
>  L’option **Intelligente** n’est pas disponible pour certains langages.  
  
#### <a name="to-convert-white-space-to-tabs"></a>Pour convertir un espace en tabulations  
  
1.  Sélectionnez le texte qui contient l'espace que vous souhaitez convertir en tabulations.  
  
2.  Dans le menu **Edition** , pointez sur **Avancée**, puis cliquez sur **Remplacer les espaces par des tabulations**.  
  
#### <a name="to-convert-tabs-to-spaces"></a>Pour convertir des tabulations en espaces  
  
1.  Sélectionnez le texte qui contient les tabulations que vous souhaitez convertir en espaces.  
  
2.  Dans le menu **Edition** , pointez sur **Avancée**, puis cliquez sur **Remplacer les tabulations par des espaces**.  
  
 Le comportement de ces commandes dépend des paramètres de tabulation définis dans la boîte de dialogue **Options** . Par exemple, si le paramètre de tabulation a pour valeur quatre, **Remplacer les espaces par des tabulations** crée une tabulation pour tout groupe de quatre espaces contigus alors que **Remplacer les tabulations par des espaces** crée quatre espaces pour chaque tabulation.  
  
## <a name="converting-text-to-upper-and-lower-case"></a>Conversion de texte en majuscules et en minuscules  
 Des commandes vous permettent de convertir du texte en majuscules ou en minuscules.  
  
#### <a name="to-switch-text-to-upper-or-lower-case"></a>Pour convertir du texte en majuscules ou minuscules  
  
1.  Sélectionnez le texte à convertir.  
  
2.  Pour convertir du texte en majuscules, appuyez sur Ctrl+Maj+U ou cliquez sur **Mettre en majuscules** dans le sous-menu **Avancée** du menu **Edition** .  
  
3.  Pour convertir du texte en minuscules, appuyez sur Ctrl+Maj+L ou cliquez sur **Mettre en minuscules** dans le sous-menu **Avancée** du menu **Edition** .  
  
> [!NOTE]  
>  Pour obtenir la liste complète des touches de raccourci clavier, consultez [Raccourcis clavier dans SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="displaying-and-linking-to-urls"></a>Affichage et liaison à des URL  
 Vous pouvez créer et gérer des URL interactives dans votre code. Par défaut, les URL :  
  
-   sont soulignées ;  
  
-   entraînent la transformation du pointeur de la souris en une main lorsque vous passez par-dessus ;  
  
-   ouvrent le site correspondant lorsque vous cliquez une fois dessus, pour autant que l'URL soit valide.  
  
#### <a name="to-display-a-clickable-url"></a>Pour afficher une URL interactive  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Cliquez sur **Éditeur de texte**.  
  
3.  Pour modifier l’option pour un seul langage, cliquez sur le dossier de celui-ci, puis sur **Général**. Pour modifier l’option pour tous les langages, cliquez sur **Tous les langages** , puis sur **Général**.  
  
4.  Sélectionnez **Activer la navigation dans les URL par simple clic**.  
  
  

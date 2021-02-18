---
title: Fenêtres Résultats de la recherche
description: Il existe deux fenêtres Résultats de la recherche contenant les correspondances trouvées par les opérations Rechercher dans les fichiers et Remplacer dans les fichiers. Découvrez comment afficher ces fenêtres ainsi que le fichier de code et la ligne correspondants.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vs.findresults2
helpviewer_keywords:
- Find Results Windows dialog box
ms.assetid: 3b68dbb7-26d6-4bc9-bd2c-c27e5dc385c3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2baef28ceee31cd1a6fc3aa59a2a8c55f64d210b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271651"
---
# <a name="find-results-windows"></a>Fenêtres Résultats de la recherche
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Les deux fenêtres Résultats de la recherche affichent les correspondances trouvées par une recherche effectuée à l'aide des onglets **Rechercher dans les fichiers** ou **Remplacer dans les fichiers** de la boîte de dialogue **Rechercher et remplacer** . La commande **Options de résultat** pour **Rechercher dans les fichiers** et **Remplacer dans les fichiers** vous permet de choisir la fenêtre Résultats de la recherche dans laquelle seront répertoriées, le cas échéant, les occurrences trouvées.  
  
 La fenêtre Résultats de la recherche sélectionnée s'ouvre automatiquement dès qu'une correspondance est trouvée. Pour afficher manuellement une fenêtre Résultats de la recherche, cliquez sur **Autres fenêtres** dans le menu **Affichage** , puis cliquez sur **Résultats de la recherche 1** ou **Résultats de la recherche 2**.  
  
 Pour afficher le fichier de code et aller directement à la ligne où une occurrence a été trouvée, double-cliquez sur n'importe quelle ligne dans la liste des résultats. Le fichier source s'affiche dans l'éditeur de code et le point d'insertion est placé au début du texte contenant la correspondance. Un symbole apparaît dans la marge des indicateurs de l'éditeur pour signaler la ligne comprenant la correspondance et le texte complet s'affiche dans la barre d'état.  
  
## <a name="toolbar-buttons"></a>Boutons de barre d'outils  
 Vous pouvez utiliser les boutons de barre d'outils suivants pour parcourir la liste des résultats et accéder directement aux lignes de votre code qui contiennent les correspondances.  
  
 **indicateur de page + flèche vers le haut**  
 Pour atteindre la ligne où la correspondance sélectionnée a été trouvée.  
  
 **page + flèche vers la gauche**  
 Pour atteindre la ligne où se trouve la correspondance précédente.  
  
 **page + flèche vers la droite**  
 Pour atteindre la ligne où se trouve la correspondance suivante.  
  
 **Effacer tout**  
 Supprimer toutes les correspondances de la liste **Résultats** .  
  
## <a name="shortcut-keys"></a>Touches de raccourci  
 Vous pouvez utiliser les touches de raccourci suivantes pour naviguer rapidement dans les correspondances trouvées.  
  
 Ctrl+Origine  
 Pour défiler vers le haut dans la fenêtre Résultats de la recherche.  
  
 CTRL+FIN  
 Pour défiler vers le bas dans la fenêtre Résultats de la recherche.  
  
 Pg. préc  
 Pour défiler vers le haut jusqu'au prochain groupe de correspondances.  
  
 Pg. suiv  
 Pour défiler vers le bas jusqu'au prochain groupe de correspondances.  
  
 Flèche haut  
 Pour sélectionner la correspondance précédente.  
  
 Bas  
 Pour sélectionner la correspondance suivante.  
  
## <a name="search-result-entries"></a>Entrées des résultats de la recherche  
 Chaque entrée de la liste des résultats fournit les informations suivantes :  
  
-   Le chemin d'accès complet  
  
-   Nom de fichier  
  
-   Numéro de ligne  
  
-   Le texte intégral de la ligne contenant la correspondance  
  
> [!TIP]  
>  Vous pouvez utiliser **Recherche rapide** pour parcourir une liste de résultats relativement longue. Ouvrez et ancrez la fenêtre Résultats de la recherche, puis cliquez sur le bouton triangulaire **Affichage** sous l'onglet **Rechercher** et basculez vers **Recherche rapide**. Sélectionnez **Fenêtre active** dans le champ **Regarder dans**, entrez une chaîne dans la zone **Rechercher** , puis cliquez sur **Suivant**. Vous pouvez ainsi rechercher dans la liste des résultats les correspondances trouvées dans des dossiers ou fichiers spécifiques ou les correspondances figurant dans des lignes de code qui contiennent un autre terme clé.  
  
## <a name="summary-lines"></a>Lignes de résumé  
 Chaque ensemble de résultats de recherche commence par une ligne indiquant les paramètres de la recherche et se termine par une ligne de statistiques. Par exemple, la liste de résultats d'une recherche **Rechercher dans les fichiers** , effectuée dans tous les documents ouverts pour trouver toutes les chaînes correspondant à l'expression régulière "`var[1-3]&par`", pourrait commencer par cette ligne de paramètres de recherche :  
  
 `Find all "var[1-3]&par" Regular Expression, All Open Documents`  
  
 et pourrait se terminer par cette ligne de statistiques :  
  
 `Total found: 57  Matching files: 23  Total files searched: 59`  
  
 La liste de résultats d'une recherche **Remplacer dans les fichiers** effectuée dans tous les documents ouverts pour remplacer toutes les chaînes correspondant à l'expression régulière "`var[1-3]&par`" par la chaîne "`sample`", pourrait commencer par cette ligne de paramètres de recherche :  
  
 `Replace "var[1-3]&par", "sample", Regular Expression, All Open Documents`  
  
 et pourrait se terminer par cette ligne de statistiques :  
  
 `Total replaced: 57  Matching files: 23  Total files searched: 59`  

---
description: Sélection d’objet avancée (AccessToSQL)
title: Sélection d’objet avancée (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4d2b367f-8ac7-4534-b66f-10300ef64ebc
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: fee1620c3352ff8756f2d69dc9df67f9e9bdf54a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041959"
---
# <a name="advanced-object-selection--accesstosql"></a>Sélection d’objet avancée (AccessToSQL)
La boîte de dialogue **section d’objets avancés** vous permet de filtrer des objets de base de données en utilisant des chaînes et des sous-chaînes dans le nom de l’objet, puis de sélectionner ou désélectionner ces objets. SSMA effectue des opérations de conversion et de migration sur les objets sélectionnés.  
  
Pour accéder à cette boîte de dialogue, cliquez avec le bouton droit dans un Explorateur de métadonnées, puis sélectionnez **sélection d’objet avancée**.  
  
Lorsque vous ouvrez la boîte de dialogue pour la première fois, cliquez sur **afficher les éléments** de sous-catégories pour afficher tous les objets qui ont des métadonnées chargées dans le projet. Vous pouvez ensuite entrer des chaînes pour filtrer les éléments. Par exemple, entrez la chaîne « Company » pour afficher tous les éléments dont le nom contient cette chaîne.  
  
Avant d’utiliser cette boîte de dialogue, vous souhaiterez peut-être forcer SSMA à charger toutes les métadonnées en convertissant les schémas ou en enregistrant le projet.  
  
## <a name="options"></a>Options  
**Vérifier tous les éléments**  
Ajoute une coche en regard de tous les éléments. Ces éléments seront immédiatement sélectionnés dans l’Explorateur de métadonnées.  
  
**Décocher tous les éléments**  
Supprime la coche en regard de tous les éléments. Ces éléments seront immédiatement effacés dans l’Explorateur de métadonnées.  
  
**Mode liste**  
Affiche les éléments filtrés dans une liste.  
  
**Mode d’affichage de table**  
Affiche les éléments filtrés dans une table.  
  
**Éléments chargés uniquement affichés**  
Active ou désactive l’affichage des catégories ou des éléments. Lorsque ce bouton est sélectionné, SSMA affiche tous les éléments qui correspondent aux critères de filtre et ceux qui ont été précédemment chargés. Lorsque ce bouton n’est pas sélectionné, SSMA affiche les dossiers de catégorie.  
  
**Filter**  
Entrez la chaîne que vous souhaitez utiliser pour filtrer les éléments. Par exemple, pour rechercher tous les éléments disponibles qui contiennent la chaîne « ID » dans le nom de l’élément, entrez la chaîne « ID » dans la zone de **filtre** .  
  
Si les éléments correspondent aux critères de filtre, les catégories ou les éléments s’affichent au fur et à mesure que vous tapez la chaîne. Pour afficher les éléments correspondants, nous vous recommandons de cliquer sur le bouton **afficher uniquement les éléments chargés** .  
  
**Effacer le filtre**  
Efface la zone de **filtre** .  
  

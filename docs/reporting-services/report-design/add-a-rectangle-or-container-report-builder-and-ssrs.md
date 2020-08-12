---
title: Ajouter un rectangle ou un conteneur (Générateur de rapports) | Microsoft Docs
description: Séparez ou accentuez des zones d’un rapport, ou fournissez un arrière-plan pour un ou plusieurs éléments de rapport en utilisant un rectangle personnalisé dans le Générateur de rapports.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10061"
- sql13.rtp.rptdesigner.rectangleproperties.general.f1
ms.assetid: f905c35f-754d-4d02-80f3-85e29ddda826
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 013232ad5f6013f60d19e9934c40828b60bb67d0
ms.sourcegitcommit: 93e4fd75e8fe0cc85e7949c9adf23b0e1c275465
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84255532"
---
# <a name="add-a-rectangle-or-container-report-builder-and-ssrs"></a>Ajouter un rectangle ou un conteneur (Générateur de rapports et SSRS)
  Ajoutez un rectangle à un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] quand vous souhaitez qu’un élément graphique sépare des zones du rapport, mette en évidence des zones d’un rapport ou constitue un arrière-plan pour un ou plusieurs éléments du rapport. Les rectangles sont également utilisés comme conteneurs qui aident à contrôler la façon dont les régions de données sont rendues dans un rapport. Vous pouvez personnaliser l'apparence d'un rectangle en modifiant ses propriétés, telles que les couleurs d'arrière-plan et de bordure. Pour plus d’informations sur l’utilisation d’un rectangle comme conteneur, consultez [Rectangles et lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rectangles-and-lines-report-builder-and-ssrs.md) et [Afficher les mêmes données dans une matrice et sur un graphique &#40;Générateur de rapports&#41;](../../reporting-services/report-design/display-the-same-data-on-a-matrix-and-a-chart-report-builder.md).    
   
## <a name="to-add-a-rectangle"></a>Pour ajouter un rectangle    
    
1.  Sous l'onglet **Insérer** , dans le groupe **Éléments de rapport** , cliquez sur **Rectangle**.    
    
2.  Dans l'aire de conception, cliquez sur l'endroit où doit venir se positionner le coin supérieur gauche du rectangle, puis faites glisser la souris jusqu'à l'endroit où doit venir se positionner son coin supérieur droit.    
    
     Notez qu'à mesure que vous déplacez le curseur, des lignes d'alignement apparaissent lorsque le curseur est aligné avec d'autres objets dans l'aire de conception. Ces lignes d'alignement vous aident à aligner des objets.    
    
## <a name="to-create-a-container"></a>Pour créer un conteneur    
    
1.  Ajoutez un élément de rapport de type rectangle au rapport.    
    
2.  Faites glisser d'autres éléments de rapport dans le rectangle.    
    
    > [!NOTE]    
    >  Un rectangle est uniquement un conteneur pour les éléments que vous créez dans le rectangle ou que vous faites glisser dans le rectangle. Si vous dessinez un rectangle autour d'un élément qui existe déjà dans l'aire de conception, le rectangle n'agit pas comme son conteneur.    
    
## <a name="to-change-rectangle-properties-such-as-color-style-or-weight"></a>Pour modifier les propriétés d'un rectangle, telles que la couleur, le style ou l'épaisseur    
    
1.  Sélectionnez le rectangle, puis cliquez sur les options de couleur, de style ou d'épaisseur de ligne dans la section **Bordure** de l'onglet Accueil.    
    
2.  Cliquez sur la flèche en regard du bouton **Bordure** pour déterminer les côtés du rectangle à modifier.    
    
    > [!NOTE]    
    >  Si vous affectez au style de ligne la valeur **Double** et que la largeur de ligne est de 1½ pt ou plus étroite, il est possible que la ligne n’apparaisse pas en double quand vous exécutez le rapport dans le Générateur de rapports, dans le Concepteur de rapports ou dans le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . Elle apparaît en double quand vous exportez le rapport à d’autres formats, comme Microsoft Word et Acrobat PDF.    
    
## <a name="see-also"></a>Voir aussi    
 [Rectangles et lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rectangles-and-lines-report-builder-and-ssrs.md)     
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)    
    
  

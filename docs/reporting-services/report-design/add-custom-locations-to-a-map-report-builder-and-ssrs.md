---
title: Ajouter des emplacements personnalisés à une carte (Générateur de rapports) | Microsoft Docs
description: Découvrez comment ajouter des emplacements personnalisés à une carte que vous avez ajoutée à un rapport paginé dans le Générateur de rapports.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- MICROSOFT.REPORTDESIGNER.MAPPOINT.POINTTEMPLATE
ms.assetid: 7d36faae-5bcc-446a-9eba-f42349cafacb
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf6610f1abb446d27825e8d2e95677100a69184d
ms.sourcegitcommit: 93e4fd75e8fe0cc85e7949c9adf23b0e1c275465
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84255672"
---
# <a name="add-custom-locations-to-a-map-report-builder-and-ssrs"></a>Ajouter des emplacements personnalisés à une carte (Générateur de rapports et SSRS)
  Après avoir ajouté une carte à un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , vous pouvez ajouter vos propres emplacements de points.  
  
 Les propriétés d'affichage de tous les points d'une couche sont contrôlées en définissant des options pour les propriétés des points de la couche. Pour un point incorporé sélectionné, vous pouvez remplacer les propriétés d'affichage.  
  
> [!NOTE]  
>  Lorsque vous remplacez les propriétés d'affichage de couche pour le point incorporé, les modifications que vous apportez ne sont pas réversibles.  
  
 Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-point-layer"></a>Pour ajouter une couche de points  
  
1.  Sur l'aire de conception du rapport, cliquez sur la carte pour la sélectionner et afficher le volet Carte.  
  
2.  Dans la barre d’outils, cliquez sur **Ajouter une couche**.  
  
3.  Dans la liste déroulante, cliquez sur **Ajouter une couche de points**. Une couche de points sans points est ajoutée à la carte. Par défaut, la couche de points est prête pour que vous ajoutiez des points incorporés.  
  
## <a name="to-add-a-custom-point"></a>Pour ajouter un point personnalisé  
  
1.  Sur l'aire de conception du rapport, cliquez sur la carte pour la sélectionner et afficher le volet Carte.  
  
2.  Dans le volet Carte, cliquez avec le bouton droit sur une couche de points de type **Incorporé**, puis cliquez sur **Ajouter un point**. Le curseur se transforme en croix.  
  
3.  Pour ajouter un point, cliquez sur un emplacement sur la carte. Un point incorporé est ajouté à la couche sélectionnée à l'emplacement où vous cliquez.  
  
## <a name="to-customize-the-display-for-an-embedded-point"></a>Pour personnaliser l'affichage pour un point incorporé  
  
1.  Cliquez avec le bouton droit sur le point, puis cliquez sur **Propriétés des points**. La boîte de dialogue **Propriétés des points incorporés de la carte** s’ouvre.  
  
2.  Cliquez sur **Remplacer les options de point pour cette couche**. Plusieurs pages de propriétés s'affichent dans le volet gauche.  
  
3.  Cliquez sur les pages et définissez les propriétés d'affichage que vous souhaitez appliquer à ce point.  
  
## <a name="see-also"></a>Voir aussi  
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md)  
  
  

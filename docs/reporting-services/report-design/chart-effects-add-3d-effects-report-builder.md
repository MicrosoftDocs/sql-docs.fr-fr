---
title: Ajouter des effets 3D à un graphique (Générateur de rapports) | Microsoft Docs
description: Donnez de la profondeur et augmentez l’impact visuel de votre rapport paginé avec des effets en trois dimensions dans le Générateur de rapports.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: ab9625d8-6557-4a4d-8123-eefa7c066ff5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1dfc09cadd1b98116f2145e95fc6e96759d53013
ms.sourcegitcommit: f898aa83561e94626024916932568ab05e73b656
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84012667"
---
# <a name="chart-effects---add-3d-effects-report-builder"></a>Effets de graphiques - Ajouter des effets 3D (Générateur de rapports)
  Des effets 3D (en trois dimensions) peuvent être utilisés pour donner de la profondeur et augmenter l’impact visuel des graphiques de vos rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Par exemple, si vous voulez insister sur un secteur particulier d'un graphique à secteurs éclatés, vous pouvez faire pivoter le graphique et changer sa perspective pour que les lecteurs remarquent ce secteur en premier. Lorsque des effets 3D sont appliqués à votre graphique, les couleurs de dégradé et les styles de hachurage sont désactivés.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-apply-3d-effects-to-a-range-area-line-scatter-or-polar-chart"></a>Pour appliquer des effets 3D à un graphique d'étendue, en aires, en courbes, en nuage de points ou polaire  
  
1.  Cliquez avec le bouton droit en un point quelconque de la zone de graphique, puis sélectionnez **Effets 3D**. La boîte de dialogue **Propriétés de la zone de graphique** s’affiche.  
  
2.  Dans **Options 3D**, sélectionnez l’option **Afficher en 3D** .  
  
3.  (Facultatif) Dans **Options 3D**, vous pouvez définir diverses propriétés liées aux angles et options de scène 3D. Pour plus d’informations sur ces propriétés, consultez [Effets 3D, de relief et autres dans un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-3d-bevel-and-other-report-builder.md).  
  
4.  Cliquez sur **OK**.  
  
## <a name="to-apply-3d-effects-to-a-funnel-chart"></a>Pour appliquer des effets 3D à un graphique en entonnoir  
  
1.  Cliquez avec le bouton droit en un point quelconque de la zone de graphique, puis sélectionnez **Effets 3D**. La boîte de dialogue **Propriétés de la zone de graphique** s’affiche.  
  
2.  Dans **Options 3D**, sélectionnez l’option **Afficher en 3D** . Cliquez sur **OK**.  
  
3.  (Facultatif) Pour personnaliser l'aspect visuel d'un graphique en entonnoir, vous pouvez en modifier les propriétés dans le volet Propriétés.  
  
    1.  Ouvrez le volet Propriétés.  
  
    2.  Dans l'aire de conception, cliquez en un point quelconque de l'entonnoir. Les propriétés de la série du graphique en entonnoir sont affichées dans le volet Propriétés.  
  
    3.  Dans la section **Général** , développez le nœud **CustomAttributes** .  
  
    4.  Pour la propriété **Funnel3DDrawingStyle** , indiquez si l’entonnoir s’affichera sous une forme circulaire ou carrée.  
  
    5.  Pour la propriété **Funnel3DRotationAngle** , définissez une valeur comprise entre -10 et 10. Celle-ci déterminera l'inclinaison appliquée au graphique en entonnoir 3D.  
  
## <a name="to-apply-3d-effects-to-a-pie-chart"></a>Pour appliquer des effets 3D à un graphique à secteurs  
  
1.  Cliquez avec le bouton droit en un point quelconque de la zone de graphique, puis sélectionnez Effets 3D. La boîte de dialogue **Propriétés de la zone de graphique** s’affiche.  
  
2.  Dans **Options 3D**, sélectionnez l’option **Afficher en 3D** . Cliquez sur **OK**.  
  
3.  (Facultatif) Dans **Rotation**, tapez une valeur entière qui représente la rotation horizontale du graphique à secteurs.  
  
4.  (Facultatif) Dans **Inclinaison**, tapez une valeur entière qui représente la rotation d’inclinaison verticale du graphique à secteurs.  
  
5.  Cliquez sur **OK**.  
  
## <a name="to-apply-3d-effects-to-a-bar-or-column-chart"></a>Pour appliquer des effets 3D à un graphique à barres ou histogrammes  
  
1.  Cliquez avec le bouton droit en un point quelconque de la zone de graphique, puis sélectionnez **Effets 3D**. La boîte de dialogue **Propriétés de la zone de graphique** s’affiche.  
  
2.  Sélectionnez l’option **Afficher en 3D** . Cliquez sur **OK**.  
  
3.  (Facultatif) Sélectionnez l’option **Activer le clustering des séries** . Si le graphique contient plusieurs séries de barres ou d'histogrammes, cette option les affiche regroupées. Par défaut, les différentes séries de barres ou d'histogrammes sont représentées côte à côte.  
  
4.  Cliquez sur **OK**.  
  
5.  (Facultatif) Pour ajouter des effets de cylindre à un graphique à barres ou histogrammes, procédez comme suit :  
  
    1.  Ouvrez le volet Propriétés.  
  
    2.  Dans l'aire de conception, cliquez sur l'une des barres de la série. Les propriétés de la série sont affichées dans le volet Propriétés.  
  
    3.  Dans la section **Général** , développez le nœud **CustomAttributes** .  
  
    4.  Pour la propriété **DrawingStyle** , spécifiez la valeur **Cylindre** .  
  
## <a name="see-also"></a>Voir aussi  
 [Effets 3D, de relief et autres dans un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-3d-bevel-and-other-report-builder.md)   
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  

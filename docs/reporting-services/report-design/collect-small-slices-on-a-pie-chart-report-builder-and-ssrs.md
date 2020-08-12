---
title: Regrouper des petits secteurs sur un graphique à secteurs (Générateur de rapports) | Microsoft Docs
description: Découvrez comment regrouper de nombreux petits secteurs d’un graphique à secteurs en un seul dans les rapports paginés du Générateur de rapports.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 21c2b8cb-b9ca-4bc0-bf49-50ba432562f6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5a4dc5671d5a23448f3549e383b26097026cc021
ms.sourcegitcommit: 6c2232c4d2c1ce5710296ce97b909f5ed9787f66
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84462063"
---
# <a name="collect-small-slices-on-a-pie-chart-report-builder-and-ssrs"></a>Regrouper des petits secteurs sur un graphique à secteurs (Générateur de rapports et SSRS)
Les graphiques à secteurs contenant trop de secteurs peuvent être confus. Apprenez à regrouper de nombreux petits secteurs d’un graphique à secteurs en un seul dans les rapports paginés [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].
 
 Pour regrouper plusieurs petits secteurs en un, vous devez en premier lieu décider si le seuil de regroupement des petits secteurs est exprimé sous forme de pourcentage du graphique à secteurs ou sous forme de valeur fixe. 
 
 Le [Didacticiel : ajouter un graphique à secteurs à un rapport (Générateur de rapports)](../tutorial-add-a-pie-chart-to-your-report-report-builder.md) vous guide pas à pas dans la procédure de regroupement de petits secteurs en un seul, si vous voulez essayer avec des exemples de données.
 
 ![report-builder-pie-chart-other-slice](../../reporting-services/report-design/media/report-builder-pie-chart-other-slice.png)
  
 Vous pouvez également regrouper des petits secteurs en un deuxième graphique à secteurs appelé à partir d'un secteur regroupé du premier graphique à secteurs. Le deuxième graphique à secteurs est alors tracé à droite du graphique à secteurs d'origine.  
  
 Le regroupement de plusieurs secteurs en un n'est pas possible pour les graphiques en entonnoir ou en pyramide.  
  
 
## <a name="to-collect-small-slices-into-a-single-slice-on-a-pie-chart"></a>Pour regrouper plusieurs petits secteurs en un sur un graphique à secteurs  
  
1.  Ouvrez le volet Propriétés.  
  
2.  Dans l'aire de conception, cliquez sur un secteur du graphique. Les propriétés de la série sont affichées dans le volet Propriétés.  
  
3.  Dans la section **Général** , développez le nœud **CustomAttributes** .  
  
4.  Définissez la propriété CollectedStyle sur **SingleSlice**.  

    ![report-builder-pie-chart-single-slice-property](../../reporting-services/media/report-builder-pie-chart-single-slice-property.png)
  
5.  Définissez la valeur de seuil de regroupement et le type de seuil. Vous trouverez ci-dessous quelques manières parmi les plus courantes de définir des seuils de regroupement.  
  
    -   **Par pourcentage.** Par exemple, pour regrouper en un secteur les secteurs de votre graphique inférieurs à 10 %, procédez comme suit :  
  
         Définissez la propriété CollectedThresholdUsePercent sur **True**.  
  
         Définissez la propriété CollectedThreshold sur **10**.  
  
        > [!NOTE]  
        >  Si vous définissez la propriété CollectedStyle sur **SingleSlice**, la propriété CollectedThreshold sur une valeur supérieure à **100** et la propriété CollectedThresholdUsePercent sur **True**, le graphique lève une exception, car il ne peut pas calculer de pourcentage. Pour résoudre ce problème, définissez la propriété CollectedThreshold sur une valeur inférieure à **100**.  
  
    -   **Par valeur des données.** Par exemple, pour regrouper en un secteur les secteurs de votre graphique inférieurs à 5000, procédez comme suit :  
  
         Définissez la propriété CollectedThresholdUsePercent sur **False**.  
  
         Définissez la propriété CollectedThreshold sur **5000**.  
  
6.  Définissez la propriété CollectedLabel sur une chaîne représentant l’étiquette de texte à afficher dans le secteur regroupé.  
  
7.  (Facultatif) Définissez les propriétés CollectedSliceExploded, CollectedColor, CollectedLegendText et CollectedToolTip. Ces propriétés modifient l'apparence, la couleur, le texte de l'étiquette, le texte de la légende et l'aspect des info-bulles du secteur obtenu.  
  
## <a name="to-collect-small-slices-into-a-secondary-callout-pie-chart"></a>Pour regrouper des petits secteurs dans un deuxième graphique à secteurs secondaire  
  
1.  Suivez les étapes 1 à 3 ci-dessus.  
  
2.  Définissez la propriété CollectedStyle sur **CollectedPie**.  
  
3.  Définissez la propriété CollectedThresholdproperty sur une valeur représentant le seuil de regroupement des petits secteurs en un seul secteur. Lorsque la propriété CollectedStyle est définie sur **CollectedPie**, la propriété CollectedThresholdUsePercentproperty est toujours définie sur **True**, et le seuil de regroupement est toujours exprimé en pourcentage.  
  
4.  (Facultatif) Définissez les propriétés CollectedColor, CollectedLabel, CollectedLegendText et CollectedToolTip. Toutes les autres propriétés nommées « Collected » ne s'appliquent pas au regroupement de secteurs.  
  
> [!NOTE]  
>  Le graphique à secteurs secondaire étant calculé selon les petits secteurs, il s'affiche uniquement sous forme d'aperçu. Il n'apparaît pas dans l'aire de conception.  
  
> [!NOTE]  
>  Vous ne pouvez pas mettre en forme le graphique à secteurs secondaire. C'est pourquoi il est fortement recommandé de privilégier la première approche pour le regroupement de secteurs.  
  
## <a name="see-also"></a>Voir aussi  
* [Tutoriel : Ajouter un graphique à secteurs à un rapport (Générateur de rapports)](../tutorial-add-a-pie-chart-to-your-report-report-builder.md)
*  [Graphiques à secteurs (Générateur de rapports et SSRS)](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md)   
*  [Mise en forme des points de données sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
*  [Afficher des étiquettes de points de données à l’extérieur d’un graphique à secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)   
*  [Afficher des valeurs en pourcentage dans un graphique à secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)     
  

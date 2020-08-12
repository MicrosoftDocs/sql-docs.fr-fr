---
title: Mettre en forme les étiquettes des axes en tant que dates ou devises (Générateur de rapports) | Microsoft Docs
description: Spécifiez un intervalle de date ou de temps pour un axe des X en mettant en forme les étiquettes de l’axe et en définissant le type d’intervalle de l’axe sur un intervalle valide.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: e9a01a74-2f51-4b35-be3a-a6138568f6cf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b65c752c479c86f88679cb01bd328dc2d22cf3be
ms.sourcegitcommit: 5b7457c9d5302f84cc3baeaedeb515e8e69a8616
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83689429"
---
# <a name="format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs"></a>Mettre en forme les étiquettes des axes en tant que dates ou devises (Générateur de rapports et SSRS)
Quand vous affichez des valeurs DateTime correctement mises en forme sur un axe dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , un graphique affiche automatiquement ces valeurs comme des jours. Pour spécifier un intervalle de date/d'heure pour l'axe des X, tel qu'un intervalle de mois ou d'heures, vous devez mettre en forme les étiquettes de l'axe et affecter une date ou un intervalle de temps valide au type de l'intervalle d'axe.  
  
> [!NOTE]  
>  Dans les histogrammes et les nuages de points, l'axe horizontal (ou axe des X) est l'axe des abscisses. Dans les graphiques à barres, l'axe vertical (ou axe des Y) est l'axe des abscisses.  
  
 Pour mettre correctement en forme les intervalles de temps, les valeurs affichées sur l’axe des X doivent prendre la valeur d’un type de données <xref:System.DateTime> . Si votre champ a un type de données <xref:System.String>, le graphique ne calcule pas les intervalles comme dates ou heures. Pour plus d’informations, consultez [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md).  
  
 Lorsqu'une valeur numérique est ajoutée à l'axe des Y, par défaut, le graphique ne met pas en forme le nombre avant de l'afficher. Si votre champ numérique est un chiffre de ventes, envisagez de mettre en forme les nombres comme devises pour améliorer la lisibilité du graphique.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-format-x-axis-labels-as-monthly-intervals"></a>Pour mettre en forme les étiquettes de l'axe des X comme intervalles mensuels  
  
1.  Cliquez avec le bouton droit sur l’axe horizontal (axe des X) du graphique, puis sélectionnez **Propriétés de l’axe horizontal**.  
  
2.  Dans la boîte de dialogue **Propriétés de l’axe horizontal** , sélectionnez **Nombre**.  
  
3.  Dans la liste **Catégorie** , sélectionnez **Date**. Dans la liste **Type** , sélectionnez un format de date à appliquer aux étiquettes de l’axe des X.  
  
4.  Sélectionnez **Options de l’axe**.  
  
5.  Dans **Intervalle**, tapez **1**. Dans la propriété **Type d’intervalle** , sélectionnez **Mois**.  
  
    > [!NOTE]  
    >  Si vous ne spécifiez pas de type d'intervalle, le graphique calcule des intervalles en termes de jours.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-format-y-axis-labels-using-a-currency-format"></a>Pour mettre en forme les étiquettes de l'axe des Y à l'aide d'un format monétaire  
  
1.  Cliquez avec le bouton droit sur l’axe vertical (axe des Y) du graphique, puis sélectionnez **Propriétés de l’axe vertical**.  
  
2.  Dans la boîte de dialogue **Propriétés de l’axe vertical** , sélectionnez **Nombre**.  
  
3.  Dans la liste **Catégorie** , sélectionnez **Devise**. Dans la liste **Symbole** , sélectionnez un format monétaire à appliquer aux étiquettes de l’axe des Y.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Spécifier une échelle logarithmique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)   
 [Spécifier un intervalle d’axe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)  
  
  

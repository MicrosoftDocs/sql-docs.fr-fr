---
title: Tracer des données sur un axe secondaire (Générateur de rapports) | Microsoft Docs
description: Découvrez les utilisations du type d’axe secondaire pour comparer deux plages de données distinctes dans le Générateur de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 094f39bf-3634-4852-9fc3-3adec4b266e5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f9a02dcb3088dd81bf2c2ca6b96c90e5040b1e37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014751"
---
# <a name="plot-data-on-a-secondary-axis-report-builder-and-ssrs"></a>Tracer des données sur un axe secondaire (Générateur de rapports et SSRS)

Le graphique a deux types d'axes : principal et secondaire. L'axe secondaire est utile lors de la comparaison de deux jeux de valeurs avec deux plages de données distinctes qui partagent une catégorie commune.  
  
 Par exemple, supposons que vous avez un graphique qui calcule vos revenus et vos impôts pour l’année 2008. Dans ce cas, la période 2008 est commune aux deux jeux de valeurs. Toutefois, lorsque les deux séries sont tracées sur le même axe des Y, nous ne pouvons pas établir de comparaison utile, car l'échelle de l'axe des Y est optimisée pour les plus grandes valeurs du dataset. Si nous affichons les revenus sur l'axe principal et les impôts sur l'axe secondaire, nous pouvons afficher chaque série sur son propre axe des Y avec sa propre échelle de valeurs. Les séries partagent encore un axe des abscisses commun.  
  
 Dans les cas où plus de deux séries doivent être comparées, envisagez une approche différente pour comparer et afficher plusieurs séries dans un graphique. Pour plus d’informations, consultez [Plusieurs séries sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
 Un exemple de ce graphique est disponible sous forme d'exemple de rapport. Pour plus d'informations sur le téléchargement de cet exemple de rapport et d'autres rapports, consultez [Exemples de rapports du Générateur de rapports et du Concepteur de rapports](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-plot-a-series-on-the-secondary-axis"></a>Pour tracer une série sur l'axe secondaire  
  
1.  Cliquez avec le bouton droit sur la série dans le graphique ou sur un champ dans la zone **Valeurs** que vous souhaitez afficher sur l’axe secondaire, puis cliquez sur **Propriétés de la série**. La boîte de dialogue **Propriétés de la série** s'affiche.  
  
2.  Cliquez sur **Axes et zone de graphique**, et sélectionnez l'axe secondaire que vous voulez activer, l'axe des ordonnées ou l'axe des abscisses.  

## <a name="next-steps"></a>Étapes suivantes

[Mise en forme des étiquettes des axes sur un graphique](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
[Spécifier un intervalle d’axe](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

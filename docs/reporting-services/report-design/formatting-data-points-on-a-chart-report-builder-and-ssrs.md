---
title: Mise en forme des points de données sur un graphique (Générateur de rapports) | Microsoft Docs
description: Découvrez différents types de mise en forme des points de données des graphiques de vos rapports dans le Générateur de rapports.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10248"
- sql13.rtp.rptdesigner.serieslabelproperties.general.f1
ms.assetid: 08ec3818-f63a-4e89-b52c-750e47f48b85
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0d9f3036e7d8c580e770ded7bbbec6e5f29b2750
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596912"
---
# <a name="formatting-data-points-on-a-chart-report-builder-and-ssrs"></a>Mise en forme des points de données sur un graphique (Générateur de rapports et SSRS)
Dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , un point de données est la plus petite entité individuelle sur un graphique. Sur les graphiques qui ne sont pas à base de formes, les points de données sont représentés selon le type de graphique. Par exemple, une série à base de lignes comprend un ou plusieurs points de données reliés. Sur les graphiques à base de formes, les points de données sont représentés par des coupes ou segments individuels qui s'ajoutent à l'ensemble du graphique. Par exemple, sur un graphique à secteurs, chaque secteur est un point de données. Pour plus d’informations, consultez [Types de graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md).  
  
 Un ou plusieurs points de données forment une série. Par défaut, toutes les options de mise en forme sont appliquées à tous les points de données de la série. Si vous souhaitez spécifier des propriétés pour des points de données individuels, vous pouvez spécifier un champ ou une expression pour la série qui met en forme le point de données individuel au moment de l'exécution en fonction du dataset.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="adding-tooltips-and-drillthrough-actions-to-data-points"></a>Ajout d'info-bulles et d'actions d'extraction aux points de données  
 Vous pouvez ajouter des info-bulles à chaque point de données en affectant une valeur à la propriété **ToolTip** pour la série. En affichant des info-bulles, vous permettez à vos utilisateurs de consulter toutes les informations relatives au point de données, telles que le nom du groupe, la valeur du point de données et le pourcentage du point de données par rapport au total de la série. Pour plus d’informations, consultez [Afficher des info-bulles dans une série &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md).  
  
 Vous pouvez également spécifier une action d'extraction pour les points de données de la série afin d'afficher un autre rapport ou une autre URL. Vous pouvez transmettre des paramètres pour afficher les informations relatives au point de données sélectionné. Pour plus d’informations, consultez [Ajouter une action d’extraction à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md).  
  
## <a name="highlighting-individual-data-points-in-a-series"></a>Mise en surbrillance de points de données individuels dans une série  
 Sur tout graphique qui n’est pas à base de formes, vous pouvez mettre en surbrillance des points de données individuels en spécifiant une expression pour la propriété Color. Par exemple, pour mettre en surbrillance la valeur de point de données la plus élevée dans une série nommée `MyField` avec une couleur différente de celle des autres points de données, l'expression serait semblable à ce qui suit :  
  
 `=Iif(Fields!MyField.Value >= Max(Fields!MyField.Value, "MyDataSet"), "Red", "Green")`  
  
 Dans cet exemple, la valeur la plus élevée pour `MyField` a la couleur rouge et tous les autres points de données ont la couleur verte. Lorsque vous spécifiez une couleur pour la série à l'aide de la propriété **Remplissage** de la série, le graphique substitue les couleurs spécifiées dans la palette. Pour plus d’informations, consultez [Mise en forme des couleurs des séries d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
## <a name="positioning-data-point-labels-on-a-chart"></a>Positionnement d'étiquettes de points de données sur un graphique  
 Pour tous les types de graphiques, vous pouvez afficher des étiquettes de points de données quand vous cliquez avec le bouton droit sur le graphique, puis que vous sélectionnez **Afficher les étiquettes de données**. La position des étiquettes de points de données est spécifiée selon le type de graphique :  
  
-   Sur un graphique à barres, vous pouvez repositionner l'étiquette de point de données à l'aide de l'attribut personnalisé **BarLabelStyle** . Il existe quatre positions possibles : Extérieur, Gauche, Centre et Droite. Lorsque le style de l'étiquette de la barre est défini sur Extérieur, les étiquettes sont positionnées en dehors de la barre, dans la mesure où elles tiennent dans la zone du graphique. Si l'étiquette ne peut pas être placée en dehors de la barre et dans la zone du graphique, elle est placée à l'intérieur de la barre.  
  
-   Sur un graphique à secteurs, vous pouvez repositionner l'étiquette de point de données à l'aide de l'attribut personnalisé **PieLabelStyle** . Il y a de nombreux facteurs à prendre en compte pour le positionnement des étiquettes de points de données autour d'un graphique à secteurs, y compris la taille du graphique à secteurs, l'espace disponible entre le graphique à secteurs et sa légende, et la taille des étiquettes. Pour plus d’informations, consultez [Afficher des étiquettes de points de données à l’extérieur d’un graphique à secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md).  
  
-   Sur un graphique en pyramide ou en entonnoir, vous pouvez repositionner les étiquettes de points de données à l'aide des attributs personnalisés **FunnelLabelStyle** et **PyramidLabelStyle** . Vous pouvez définir ces attributs dans le volet **Propriétés** lorsque vous avez sélectionné un graphique en pyramide ou en entonnoir.  
  
-   Sur les graphiques empilés, les étiquettes de points de données sont toujours positionnées à l'intérieur de la série et la propriété **Position** de l'étiquette de la série est ignorée.  
  
-   Sur tous les autres types de graphique, vous pouvez repositionner l'étiquette de point de données à l'aide de la propriété **Position** de l'étiquette de la série. Par défaut, le graphique calcule automatiquement la position des étiquettes de points de données pour éviter les chevauchements d'étiquettes. Lorsque vous définissez une valeur pour **Position**, toutes les étiquettes de points de données sont positionnées de la même manière, ce qui peut provoquer un chevauchement des étiquettes. Envisagez cette approche uniquement lorsque vous avez peu de points de données.  
  
 Pour plus d’informations, consultez [Placer des étiquettes dans un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/position-labels-in-a-chart-report-builder-and-ssrs.md).  
  
## <a name="adding-keywords-for-data-point-labels-tooltips-and-legend-text"></a>Ajout de mots clés pour les étiquettes de point de données, les info-bulles et le texte de légende  
 Vous pouvez utiliser des mots clés sensibles à la casse et spécifiques au graphique pour représenter un élément qui existe dans le graphique. Ces mots clés sont uniquement applicables aux info-bulles, textes de légende personnalisés et propriétés d'étiquette de point de données. Dans de nombreux cas, un mot clé de graphique a une expression simple équivalente, mais le mot clé est plus rapide et plus facile à taper. Les mots clés de graphique sont répertoriés dans la liste suivante :  
  
|Mot clé de graphique|Description|Applicable au type de graphique|Exemple d'expression simple équivalente|  
|-------------------|-----------------|------------------------------|------------------------------------------------|  
|#VALY|Valeur Y du point de données.|Tous|`=Fields!MyDataField.Value`|  
|#VALY2|Valeur Y n° 2 du point de données.|Graphique d'étendue, graphique à bulles|None|  
|#VALY3|Valeur Y n° 3 du point de données.|Graphique boursier, graphique en chandelier|None|  
|#VALY4|Valeur Y n° 4 du point de données.|Graphique boursier, graphique en chandelier|None|  
|#SERIESNAME|Nom de la série.|Tous|None|  
|#LABEL|Étiquette de point de données.|Tous|None|  
|#AXISLABEL|Étiquette de point de données d'axe.|Graphique à base de formes|`=Fields!MyDataField.Value`|  
|#INDEX|Index de point de données.|Tous|None|  
|#PERCENT|Pourcentage de la valeur Y du point de données.|Tous|`=FormatPercent(Fields!MyDataField.Value/Sum(Fields!MyDataField.Value, "MyDataSet"),2)`|  
|#TOTAL|Total de toutes les valeurs Y de la série.|Tous|`=Sum(Fields!MyDataField.Value)`|  
|#LEGENDTEXT|Texte qui correspond au texte de l'élément de légende.|Tous|None|  
|#AVG|Moyenne de toutes les valeurs Y de la série.|Tous|`=Avg(Fields!MyDataField.Value)`|  
|#MIN|Minimum de toutes les valeurs Y de la série.|Tous|`=Min(Fields!MyDataField.Value)`|  
|#MAX|Maximum de toutes les valeurs Y de la série.|Tous|`=Max(Fields!MyDataField.Value)`|  
|#FIRST|Première de toutes les valeurs Y de la série.|Tous|`=First(Fields!MyDataField.Value)`|  
  
 Pour mettre en forme le mot clé, mettez une chaîne de mise en forme [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] entre parenthèses. Par exemple, pour spécifier la valeur du point de données dans une info-bulle sous la forme d’un nombre à deux décimales, incluez la chaîne de format « N2 » entre accolades, telle que « #VALY{N2} » pour la propriété **ToolTip** de la série. Pour plus d'informations sur les chaînes de mise en forme [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , consultez [Mise en forme des types](/previous-versions/) sur le site MSDN. Pour plus d’informations sur la mise en forme des nombres dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Mise en forme des nombres et des dates &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur l’ajout de mots clés à un graphique, consultez [Afficher des info-bulles dans une série &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md), [Changer le texte d’un élément de légende &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
## <a name="increasing-readability-in-a-chart-with-multiple-data-points"></a>Augmentation de la lisibilité d'un graphique avec plusieurs points de données  
 Si votre graphique comporte plusieurs séries, la lisibilité des points de données du graphique peut s'en trouver limitée. Lorsque vous ajoutez plusieurs séries à un graphique, envisagez d'utiliser une technique qui permet de lire et comprendre chaque série dans le graphique. Pour plus d’informations, consultez [Plusieurs séries sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md).  
  
 Pour plus de simplicité, lorsque vous utilisez un graphique à base de formes, envisagez d'ajouter un seul champ de données et un champ de catégorie. Pour plus d’informations, consultez [Graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/shape-charts-report-builder-and-ssrs.md). Si votre graphique nécessite plusieurs champs de données et champs de catégorie, pensez à utiliser un autre type de graphique. Vous pouvez cliquer avec le bouton droit sur la série, puis sélectionner **Modifier le type du graphique**.  
  
## <a name="inserting-data-point-markers"></a>Insertion de marqueurs de point de données  
 Un marqueur de point de données est un indicateur visuel utilisé pour attirer l'attention sur chaque point de données d'une série. Sur un graphique à nuages de points, le marqueur est utilisé pour déterminer la forme et la taille des points de données individuels. La taille du marqueur est calculée en fonction du type de graphique. Vous pouvez modifier la taille, la couleur ou le style du marqueur. Les marqueurs ne sont pas disponibles pour les graphiques à base de formes, les graphiques d'étendue ou pour les sous-types empilés.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Afficher des info-bulles dans une série &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [Afficher des étiquettes de points de données à l’extérieur d’un graphique à secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)  
  
 [Afficher des valeurs en pourcentage dans un graphique à secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Mettre en forme les étiquettes des axes en tant que dates ou devises &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Tutoriel : Ajouter un graphique à secteurs à un rapport &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  

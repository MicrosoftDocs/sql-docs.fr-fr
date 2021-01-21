---
title: Mise en forme des nombres et des dates (Générateur de rapports) | Microsoft Docs
description: Découvrez comment mettre en forme les nombres et les dates dans les régions de données du Générateur de rapports en sélectionnant un format dans la page Nombre de la boîte de dialogue Propriétés de la région de données.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.placeholderproperties.number.f1
- "10127"
- sql13.rtp.rptdesigner.textboxproperties.number.f1
- "10130"
- "10286"
- sql13.rtp.rptdesigner.serieslabelproperties.number.f1
- "10285"
- sql13.rtp.rptdesigner.axisproperties.number.f1
ms.assetid: 6de1a725-9f06-4708-be26-2d55e442e344
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9df64577305006d406be0f10e060a7ac60ed10df
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596895"
---
# <a name="formatting-numbers-and-dates-report-builder-and-ssrs"></a>Mise en forme des nombres et des dates (Générateur de rapports et SSRS)
  Vous pouvez mettre en forme les nombres et les dates dans les régions de données en sélectionnant un format dans la page **Nombre** de la boîte de dialogue **Propriétés** de la région de données correspondante.  
  
 Pour spécifier des chaînes de format dans un élément de rapport de zone de texte, sélectionnez l’élément que vous souhaitez mettre en forme, cliquez dessus avec le bouton droit, sélectionnez **Propriétés de la zone de texte**, puis cliquez sur **Nombre**. En procédant de la même manière, vous pouvez mettre en forme des cellules individuelles d'une région de données de table ou de matrice, car ces cellules sont des zones de texte individuelles.  
  
 Une région de données de graphique affiche généralement des dates le long de l'axe des abscisses (x) et des valeurs le long de l'axe des ordonnées (y). Pour spécifier la mise en forme d’un graphique, cliquez avec le bouton droit sur un axe et sélectionnez **Propriétés de l’axe**. Sur l'axe des ordonnées, vous ne pouvez spécifier que des formats pour les nombres. Pour plus d’informations, consultez [Mise en forme des points de données sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  
  
 Pour spécifier la mise en forme d’une région de données de jauge, cliquez avec le bouton droit sur l’échelle de la jauge et sélectionnez **Propriétés de l’échelle radiale** ou **Propriétés de l’échelle linéaire**.  
  
> [!NOTE]  
>  Si certaines options de mise en forme que vous souhaitez utiliser apparaissent en grisé, cela signifie qu'elles ne sont pas compatibles avec le type de données du champ, défini dans la source de données. Par exemple, si le champ contient des valeurs numériques mais que son type de données est String, vous ne pouvez pas appliquer les options de mise en forme de données numériques, comme les devises ou les décimaux.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="considerations-for-formatting-numbers-and-dates"></a>Éléments à prendre en considération pour mettre en forme des nombres et des dates  
 Avant de mettre en forme des nombres et des dates dans votre rapport, prenez en considération les éléments suivants :  
  
-   Par défaut, les nombres sont mis en forme de manière à refléter les paramètres culturels de l'ordinateur client. Utilisez des chaînes de mise en forme pour spécifier l'affichage des nombres, de sorte que la mise en forme soit cohérente quel que soit l'endroit où se trouve la personne qui consulte le rapport.  
  
-   Les formats fournis dans la page **Nombre** constituent un sous-ensemble des chaînes de format numérique standard [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Pour mettre en forme un nombre ou une date à l'aide d'un format personnalisé qui ne figure pas dans la boîte de dialogue, vous pouvez utiliser les chaînes de format [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour les nombres ou les dates. Pour plus d'informations sur les chaînes de format personnalisées, consultez la rubrique [Mise en forme des types](/previous-versions/) sur MSDN.  
  
-   Si une chaîne de format personnalisée a été spécifiée, sa priorité est plus élevée que les paramètres par défaut qui sont spécifiques à une culture. Par exemple, supposez que vous définissez une chaîne de format personnalisée de "#,###" pour afficher le nombre 1234 sous la forme 1,234. Cela peut avoir une signification différente selon que l'utilisateur se trouve aux États-Unis ou en Europe. Avant de spécifier un format personnalisé, tenez compte de l'effet du format que vous choisissez sur des utilisateurs de cultures différentes qui peuvent être amenés à consulter le rapport.  
  
-   Si vous spécifiez une chaîne de format non valide, le texte mis en forme est interprété en tant que chaîne littérale qui remplace la mise en forme.  
  
-   Si vous mettez en forme une combinaison de nombres et de caractères dans une même zone de texte, envisagez d'utiliser un espace réservé pour mettre en forme les nombres en les séparant du reste du texte. Pour plus d’informations, consultez [Mise en forme du texte et des espaces réservés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md). Si une chaîne de format non valide est spécifiée pour la propriété Format de la zone de texte, la chaîne de format est ignorée. Si une chaîne de format non valide est spécifiée pour la propriété Format du graphique ou de la jauge, la chaîne de format que vous avez spécifiée est interprétée en tant que chaîne et la mise en forme n’est pas appliquée.  
  
-   Si vous sélectionnez **Devise** sous **Catégorie** et si vous activez **Afficher les valeurs en**, vous pouvez sélectionner **Milliers**, **Millions** ou **Milliards** pour afficher les nombres au format financier. Par exemple, si la valeur du champ est 1 789 905 394, que vous sélectionnez **Milliards** et spécifiez 2 comme nombre de décimales, la valeur affichée dans le rapport est 1,78.  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en forme du texte et des espaces réservés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [Mise en forme des lignes, des couleurs et des images &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-lines-colors-and-images-report-builder-and-ssrs.md)   
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Mettre en forme les étiquettes des axes en tant que dates ou devises &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Mise en forme des échelles sur une jauge &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-scales-on-a-gauge-report-builder-and-ssrs.md)  
  

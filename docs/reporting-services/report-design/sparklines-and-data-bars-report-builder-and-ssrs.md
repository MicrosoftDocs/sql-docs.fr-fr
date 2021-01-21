---
title: Graphiques sparkline et barres de données (Générateur de rapports) | Microsoft Docs
description: Découvrez les avantages de l’utilisation des graphiques sparkline et des barres de données dans le Générateur de rapports. Ces graphiques compacts transmettent beaucoup d’informations dans un petit espace.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.sparklines.f1
- "10544"
ms.assetid: b287436b-fa48-4970-a1a7-1dbcb86e7411
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8eb3019b347427b2b24a8fb48083a77935f75f8
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595150"
---
# <a name="sparklines-and-data-bars-report-builder-and-ssrs"></a>Graphiques sparkline et barres de données (Générateur de rapports et SSRS)
  Les graphiques sparkline et les barres de données sont des graphiques simples de petite taille qui communiquent beaucoup d'informations dans un petit espace, souvent intégré au texte.   
    
  Dans les rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , ils sont souvent utilisés dans les tables et les matrices. Ils tirent leur impact de l'affichage d'un grand nombre d'entre eux en même temps et de la possibilité de les comparer rapidement par superposition, au lieu de les examiner séparément. Les valeurs hors norme, les lignes qui ne sont pas exécutées comme les autres, sont ainsi plus visibles. Malgré sa petite taille, chaque graphique sparkline représente fréquemment plusieurs points de données, souvent dans le temps. Les barres de données peuvent représenter plusieurs points de données, mais en général n'en illustrent qu'un seul. Chaque graphique sparkline présente en général une série unique. Vous ne pouvez pas ajouter de graphique sparkline à un groupe de détails dans une table. Étant donné que les graphiques sparkline affichent des données agrégées, ils doivent entrer dans une cellule associée à un groupe. Les graphiques sparkline et les barres de données ont les mêmes éléments de graphique de base de catégories, séries et valeurs, mais ils n'ont aucune légende, ligne d'axe, étiquette ni graduation.  
  
 ![rs_SparklineExample](../../reporting-services/report-design/media/rs-sparklineexample.gif "rs_SparklineExample")  
  
 Pour une prise en main rapide des graphiques sparkline, consultez [Didacticiel : ajouter un graphique sparkline à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/tutorial-add-a-sparkline-to-your-report-report-builder.md) , ainsi que les vidéos [Procédure : créer un graphique sparkline dans une table](/SharePoint/sharepoint-server) et [Graphiques sparkline, graphiques à barres et indicateurs dans le Générateur de rapports](/previous-versions/dn912438(v=msdn.10)) .  
  
> [!NOTE]  
>  Vous pouvez publier des graphiques sparkline et des barres de données avec leur table, matrice ou liste parente indépendamment d'un rapport en tant que partie de rapport. En savoir plus sur les [Parties de rapports](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
##  <a name="types-of-sparklines"></a><a name="KindsofSparklines"></a> Types de graphiques sparkline  
 Vous pouvez créer presque autant de types de graphiques sparkline qu'il n'existe de graphiques standard. En général, vous ne pouvez pas créer de graphiques sparkline 3D. Vous pouvez dériver des versions de graphique sparkline de ces graphiques intégraux :  
  
-   [Histogrammes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/column-charts-report-builder-and-ssrs.md) : Histogrammes de base, empilés et empilés à 100 %.  
  
-   [Graphiques en courbes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/line-charts-report-builder-and-ssrs.md) : Tous à l'exception du graphique en courbes 3D.  
  
-   [Graphiques en aires &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/area-charts-report-builder-and-ssrs.md) : Tous à l'exception des graphiques en aires 3D.  
  
-   [Graphiques en secteurs &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md) : et graphiques en anneau plats et en 3D, mais pas les autres formes, comme les graphiques en entonnoir et en pyramide.  
  
-   [Graphiques d’étendue &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/range-charts-report-builder-and-ssrs.md) : Graphiques boursiers, en bougies, avec des barres d'erreur et à surfaces.  
  
##  <a name="data-bars"></a><a name="DataBars"></a> Barres de données  
 Les barres de données représentent en général un point de données unique, même si elles peuvent représenter plusieurs points de données, tout comme des graphiques à barres standard. Elles contiennent souvent plusieurs séries sans catégorie ou ont un regroupement de séries.  
  
 ![rs_DataBars](../../reporting-services/report-design/media/rs-databars.gif "rs_DataBars")  
  
 Dans cet exemple utilisant des barres de données empilées, chaque barre de données, même unique, illustre plusieurs points de données. Par exemple, les trois couleurs différentes de la barre pourraient représenter des tâches de trois niveaux de priorité avec la longueur de la barre représentant le nombre total de tâches affectées à chaque personne. Si vous avez créé ces barres de données empilées100 %à la place, chaque barre remplirait la cellule et les différentes couleurs représenteraient le pourcentage de la totalité pour chaque niveau de priorité.  
  
 Vous pouvez dériver des versions de barre de données de ces graphiques intégraux :  
  
-   [Graphiques à barres &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/bar-charts-report-builder-and-ssrs.md) : Graphiques à barres de base, empilées et empilées à 100 %.  
  
-   [Histogrammes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/column-charts-report-builder-and-ssrs.md) : Histogrammes de base, empilés et empilés à 100 %. Les histogrammes peuvent être des graphiques sparkline ou des barres de données.  
  
##  <a name="aligning-sparkline-data-in-a-table-or-matrix"></a><a name="AlignDatainTableMatrix"></a> Alignement des données du graphique sparkline dans une table ou matrice  
 Lorsque vous insérez un graphique sparkline dans une table ou matrice, il est habituellement important que les points de données dans chaque graphique sparkline soient alignés avec les points de données des autres graphiques sparkline dans cette colonne. Sinon, il est difficile de comparer les données dans des lignes différentes. Par exemple, lorsque vous comparez les chiffres des ventes par mois pour différents vendeurs dans votre société, vous souhaitez que les mois soient alignés. Si un employé était absent pendant le mois d'avril, il n'existe aucune donnée pour cet employé pour ce mois. Vous souhaitez voir un vide pour ce mois et les données pour les mois suivants alignées avec celles pour les autres employés. Pour cela, vous pouvez aligner l'axe horizontal. Pour plus d’informations, consultez la section relative aux graphiques sparkline dans [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md), ainsi que la rubrique [Aligner les données d’un graphique dans une table ou une matrice &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/align-the-data-in-a-chart-in-a-table-or-matrix-report-builder-and-ssrs.md).  
  
 De la même façon, les données doivent également être alignées verticalement pour pouvoir être comparées entre les lignes, ce qui signifie que la hauteur des barres ou lignes dans un graphique sparkline ou une barre de données doit être relative à la hauteur des barres et lignes dans tous les autres graphiques sparkline ou barres de données. Sinon, vous ne pouvez pas comparer les lignes entre elles.  
  
 ![rs_SparklineAlignData](../../reporting-services/report-design/media/rs-sparklinealigndata.gif "rs_SparklineAlignData")  
  
 Dans cette image, l'histogramme affiche les ventes quotidiennes pour chaque employé. Notez que les jours pendant lesquels un employé ne génère aucun chiffre d'affaires, le graphique laisse un espace et aligne les jours suivants. Voici un exemple d'alignement horizontal. Notez également que, pour certains employés, chaque barre est courte et aucune barre n'atteint le haut de la cellule. Voici un exemple d'alignement vertical ; sans cet alignement, dans les lignes sans grandes barres, les barres courtes se développeraient pour remplir la hauteur de la cellule.  
  
##  <a name="understanding-the-data-supplied-to-a-sparkline-or-data-bar"></a><a name="UnderstandScope"></a> Présentation des données fournies à un graphique sparkline ou une barre de données  
 Quand vous ajoutez un graphique sparkline ou une barre de données à une table ou une matrice, cette opération est appelée *imbrication* d’une région de données dans une autre. L'imbrication signifie que les données fournies au graphique sparkline ou à la barre de données sont contrôlées par le dataset sur lequel la table ou la matrice est basée, et selon sa position dans la table ou matrice. Pour plus d’informations, consultez [Régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md).  
  
##  <a name="converting-a-sparkline-or-data-bar-to-a-full-chart"></a><a name="ConvertSparklinetoChart"></a> Conversion d'un graphique sparkline ou d'une barre de données en un graphique intégral  
 Les graphiques sparkline et les barres de données sont un simple type de graphique. Si vous préférez bénéficier des fonctionnalités des graphiques intégraux, vous pouvez convertir un graphique de ce type en graphique intégral en cliquant avec le bouton droit sur le graphique, puis en cliquant sur **Convertir en graphique intégral**. Lorsque vous procédez ainsi, les lignes de l'axe, les étiquettes, les graduations et la légende sont ajoutées automatiquement.  
  
> [!NOTE]  
>  Vous ne pouvez pas convertir un graphique intégral en un graphique sparkline ou une barre de données en un seul clic. Toutefois, vous pouvez créer un graphique sparkline ou une barre de données à partir d'un graphique intégral uniquement en supprimant tous les éléments de graphique qui ne figurent pas dans les graphiques sparkline et les barres de données.  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Rubriques de procédures  
 [Ajouter des graphiques sparkline et des barres de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-sparklines-and-data-bars-report-builder-and-ssrs.md)  
  
 [Aligner les données d’un graphique dans une table ou une matrice &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/align-the-data-in-a-chart-in-a-table-or-matrix-report-builder-and-ssrs.md)  
  
### <a name="other-how-to-topics-for-charts"></a>Autres rubriques de procédures pour les graphiques  
 Les graphiques sparkline et les barres de données étant un type de graphique, vous pouvez également consulter les rubriques de procédures suivantes pour obtenir des informations utiles et pertinentes sur les graphiques :  
  
 [Ajouter un graphique à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)  
  
 [Ajouter des points vides à un graphique (Générateur de rapports et SSRS)](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)  
  
 [Ajouter ou supprimer des marges dans un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-remove-margins-from-a-chart-report-builder-and-ssrs.md)  
  
 [Modifier un type de graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-a-chart-type-report-builder-and-ssrs.md)  
  
 [Définir les couleurs d’un graphique à l’aide d’une palette &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/define-colors-on-a-chart-using-a-palette-report-builder-and-ssrs.md)  
  
 [Afficher des info-bulles dans une série &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [Spécifier une échelle logarithmique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)  
  
 [Spécifier un intervalle d’axe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)  
  
 [Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Tutoriel : Ajouter un graphique sparkline à un rapport &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-add-a-sparkline-to-your-report-report-builder.md)   
  

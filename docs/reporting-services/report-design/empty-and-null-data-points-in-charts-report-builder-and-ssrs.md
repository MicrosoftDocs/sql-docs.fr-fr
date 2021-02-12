---
title: Points de données vides et Null dans les graphiques (Générateur de rapports) | Microsoft Docs
description: Découvrez le traitement des graphiques pour les valeurs vides, l’utilisation d’espaces réservés et l’affichage des points de données vides et Null dans le Générateur de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: faddd29d-4cc1-4c2c-8e29-d3d9918fe22a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4ed8a71adea4b57d96137cc94168ca1810956bdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073783"
---
# <a name="empty-and-null-data-points-in-charts-report-builder-and-ssrs"></a>Points de données vides et Null dans les graphiques (Générateur de rapports et SSRS)

  Si vous affichez des champs avec des valeurs vides ou null dans un graphique de votre rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , l’aspect du graphique peut ne pas correspondre à vos attentes. Les graphiques traitent les valeurs vides différemment en fonction du type de graphique spécifié :  
  
-   Si le type de graphique est un type de graphique linéaire (graphique à barres, histogramme, graphique en nuage de points, graphique en courbes, graphique en aires, graphique d'étendue), les valeurs vides sont affichées sous forme d'espaces vides, ou « vides », dans le graphique. Si vous souhaitez spécifier des points vides, vous devez ajouter des espaces réservés à ces points. Pour plus d’informations, consultez [Ajouter des points vides à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md).  
  
-   Si le graphique utilisé est un graphique linéaire contigu (graphique à aires, graphique à barres, histogramme, graphique en courbes, graphique à nuages de points), des points de données vides y sont ajoutés pour maintenir la continuité dans les séries.  
  
-   Si le graphique utilisé est un graphique non linéaire (polaire, à secteurs, en anneau, en entonnoir ou en pyramide), les valeurs vides n'y sont pas affichées.  
  
-   Dans les types de graphiques à base de formes, les valeurs Null sont omises.  
  
 Un exemple de graphique avec des points de données vides est disponible sous forme d'exemple de rapport. Pour plus d'informations sur le téléchargement de cet exemple de rapport et d'autres rapports, consultez [Exemples de rapports du Générateur de rapports et du Concepteur de rapports](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="removing-empty-or-null-values"></a>Suppression de valeurs vides ou Null  
 Pour éviter de masquer des données importantes, envisagez la suppression des valeurs vides de votre dataset. Pour filtrer les valeurs Null, vous pouvez utiliser la clause NOT IS NULL dans votre requête. Vous pouvez également ajouter une expression de filtrage qui spécifie que vous ne voulez afficher que les valeurs différentes de zéro. Pour plus d’informations, consultez [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
## <a name="fields-with-no-values-in-a-chart"></a>Champs sans valeurs dans un graphique  
 Si un champ ne contient aucune valeur dans le dataset retourné, le graphique affiche un graphique vide sans point de données, mais le nom de la série (en général, le nom du champ) est ajouté en tant qu'élément de légende.  
  
 Ce comportement est différent du cas où il n'y a aucune ligne de données dans le dataset retourné, ce qui peut se produire lorsque le rapport est paramétré et que la valeur sélectionnée retourne un jeu de résultats vide. Si votre requête de dataset retourne zéro ligne de données, un message s'affiche au moment de l'exécution pour indiquer qu'aucune donnée ne peut être présentée. Vous pouvez personnaliser ce message en modifiant la légende NoDataMessage pour le rapport dans le volet **Propriétés** . Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  

## <a name="next-steps"></a>Étapes suivantes

[Graphiques](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Mise en forme d’un graphique](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Ajouter un graphique à un rapport](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)   
[Dépanner les graphiques](../../reporting-services/report-design/troubleshoot-charts-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

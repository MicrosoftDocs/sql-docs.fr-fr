---
title: Ajouter un graphique à un rapport (Générateur de rapports) | Microsoft Docs
description: Découvrez comment ajouter un graphique à un rapport paginé quand vous souhaitez synthétiser des données dans un format visuel dans le Générateur de rapports.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a6b595dc-f775-4a53-8554-74a0bf9335ec
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 075dac12d1a4b5e393ead71da7bbf297fe40ae69
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596774"
---
# <a name="add-a-chart-to-a-report-report-builder-and-ssrs"></a>Ajouter un graphique à un rapport (Générateur de rapports et SSRS)
  Quand vous souhaitez résumer des données sous un format visuel dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , utilisez une région de données de graphique. Il est important de choisir un type de graphique approprié au type des données que vous présentez. Cela affecte l'interprétation des données lorsqu'elles sont transformées en graphique. Pour plus d’informations, consultez [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md).  
  
 Le moyen le plus simple d'ajouter une région de données graphique à votre rapport consiste à exécuter l'Assistant Nouveau graphique. Cet Assistant propose des histogrammes, des graphiques à barres, des graphiques en courbes, des graphiques à secteurs et des graphiques en aires. Pour ces types de graphiques et d'autres, vous pouvez également ajouter un graphique manuellement.  
  
 Après avoir ajouté une région de données graphique à l’aire de conception, vous pouvez faire glisser des champs de dataset du rapport pour les données numériques et non numériques vers le volet Données du graphique du graphique. Cliquez sur le graphique pour afficher le volet Données du graphique avec ses trois zones : Groupes de séries, Groupes de catégories et Valeurs.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-chart-to-a-report-by-using-the-chart-wizard"></a>Pour ajouter un graphique à un rapport à l'aide de l'Assistant Graphique  
  
1.  > [!NOTE]  
    >  L'Assistant Graphique est disponible uniquement dans le Générateur de rapports.  
  
     Sous l’onglet **Insérer** , cliquez sur **Graphique**, puis sur **Assistant Graphique**.  
  
2.  Suivez les étapes de l’Assistant **Nouveau graphique** .  
  
3.  Sous l’onglet **Accueil** , cliquez sur **Exécuter** pour visualiser le rapport rendu.  
  
4.  Sous l’onglet **Exécuter** , cliquez sur **Conception** pour continuer à utiliser le rapport.  
  
## <a name="to-add-a-chart-to-a-report"></a>Pour ajouter un graphique à un rapport  
  
1.  Créez un rapport et définissez un dataset. Pour plus d’informations, consultez [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md).  
  
2.  Sous l’onglet **Insérer** , cliquez sur **Graphique**, puis sur **Insérer un graphique**.  
  
3.  Cliquez sur l'aire de conception à l'endroit où doit venir se positionner le coin supérieur gauche du graphique, puis faites glisser la souris jusqu'à l'endroit où doit venir se positionner son coin inférieur droit.  
  
     La boîte de dialogue **Sélectionner un type de graphique** s’affiche.  
  
4.  Sélectionnez le type de graphique que vous souhaitez ajouter. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
5.  Cliquez sur le graphique pour afficher le volet **Données du graphique** .  
  
6.  Ajoutez un ou plusieurs champs à la zone **Valeurs** . Ces informations seront tracées sur l'axe des ordonnées.  
  
7.  Ajoutez un champ de regroupement à la zone **Groupes d’abscisses** . Quand vous ajoutez ce champ à la zone **Groupes d’abscisses** , un champ de regroupement est automatiquement créé. Chaque groupe représente un point de données dans votre série.  
  
8.  Pour résumer les données par catégorie, cliquez avec le bouton droit sur le champ de données et cliquez sur **Propriétés de la série**. Dans la zone **Catégorie** , sélectionnez le champ de catégorie dans la liste déroulante. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
9. Sous l’onglet **Accueil** , cliquez sur **Exécuter** pour visualiser le rapport rendu.  
  
10. Sous l’onglet **Exécuter** , cliquez sur **Conception** pour continuer à utiliser le rapport.  
  
 Sur les graphiques avec des axes, tels que les graphiques à barres et histogrammes, il est possible que l'axe des abscisses n'affiche pas toutes les étiquettes de catégorie. Pour plus d’informations sur la façon de modifier les étiquettes d’axe, consultez [Spécifier un intervalle d’axe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Types de graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md)   
 [Points de données vides et Null dans les graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/empty-and-null-data-points-in-charts-report-builder-and-ssrs.md)   
 [Tutoriel : Ajouter un graphique à barres à un rapport (Générateur de rapports)](../tutorial-add-a-bar-chart-to-your-report-report-builder.md)   
 [Tutoriel : Ajouter un graphique à barres à un rapport (Concepteur de rapports)](/previous-versions/sql/sql-server-2008-r2/cc281302(v=sql.105))   
 [Tutoriel : Ajouter un graphique à secteurs à un rapport (Générateur de rapports)](../tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [Tutoriel : Ajouter un graphique à secteurs à un rapport (Concepteur de rapports)](/previous-versions/sql/sql-server-2008-r2/cc281304(v=sql.105))  
  

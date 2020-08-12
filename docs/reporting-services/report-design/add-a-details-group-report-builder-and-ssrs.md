---
title: Ajouter un groupe de détails (Générateur de rapports) | Microsoft Docs
description: Découvrez comment ajouter un groupe de détails à une région de données de tableau matriciel existante pour afficher les données détaillées d’une matrice dans le Générateur de rapports.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 5ef8efba-6d48-4aeb-a3b9-a02ba5a44614
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 040f7017734a31b77792c3cb2b3d714742b5044a
ms.sourcegitcommit: e572f1642f588b8c4c75bc9ea6adf4ccd48a353b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84779131"
---
# <a name="add-a-details-group-report-builder-and-ssrs"></a>Ajouter un groupe de détails (Générateur de rapports et SSRS)
Dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , les données de détail d’un dataset de rapport sont spécifiées en tant que groupe sans expression de groupe. Ajoutez un groupe de détails à une région de données de tableau matriciel existante lorsque vous souhaitez afficher les données de détail d'une matrice, rajouter des données de détail que vous avez supprimées d'une table ou d'une liste ou ajouter d'autres groupes de détails. Pour plus d’informations sur les groupes, consultez [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-details-group-to-a-tablix-data-region"></a>Pour ajouter un groupe de détails à une région de données de tableau matriciel  
  
1.  Sur l'aire de conception, cliquez n'importe où dans une région de données de tableau matriciel pour la sélectionner. Le volet Regroupement affiche les groupes de lignes et de colonnes de la région de données sélectionnée.  
  
2.  Dans le volet Regroupement, cliquez avec le bouton droit sur un groupe correspondant à un groupe enfant des plus profonds. Cliquez sur **Ajouter un groupe**, puis sur **Groupe enfant**. La boîte de dialogue **Groupe de tableaux matriciels** s'affiche.  
  
3.  Dans **Expression Groupe**, laissez l'expression vide. Un groupe de détails ne possède aucune expression.  
  
4.  Sélectionnez **Afficher les données de détail**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Un nouveau groupe de détails est ajouté en tant que groupe enfant dans le volet Regroupement et le descripteur de ligne du groupe que vous avez sélectionné au cours de l'étape 1 affiche l'icône du groupe de détails. Pour plus d’informations sur les descripteurs, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md)   
 [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md) [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)      
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  

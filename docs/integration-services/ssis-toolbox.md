---
description: Boîte à outils SSIS
title: Boîte à outils SSIS | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.toolboxfavorites.F1
- sql13.dts.designer.toolbox.F1
- sql13.dts.designer.toolboxcommon.F1
ms.assetid: 552ff592-eeef-46e8-b4a2-9b2384c869aa
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c67835df192436ce3b8264849589a1db3f66fee
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "92192373"
---
# <a name="ssis-toolbox"></a>Boîte à outils SSIS

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Tous les composants installés sur l’ordinateur local apparaissent automatiquement dans la **Boîte à outils SSIS**. Lorsque vous installez des composants additionnels, cliquez avec le bouton droit dans la boîte à outils, puis cliquez sur **Boîte à outils Actualiser** pour ajouter les composants.  
 
 Quand vous créez un projet SSIS ou ouvrez un projet existant, la **Boîte à outils SSIS** est automatiquement affichée. Vous pouvez également ouvrir la boîte à outils en cliquant sur le bouton de la boîte à outils en haut à droite de l’aire de conception du package, ou en cliquant sur Affichage -> Autres fenêtres -> Boîte à outils SSIS.
 
 > [!NOTE]
> Si vous ne voyez pas la boîte à outils, cliquez sur Affichage -> Autres fenêtres -> Boîte à outils SSIS.
 
Vous pouvez obtenir des informations supplémentaires sur un composant de la boîte à outils en cliquant sur le composant pour afficher sa description en bas de la boîte à outils. Pour certains composants, vous pouvez également accéder à des exemples qui montrent comment configurer et utiliser les composants. Ces exemples sont disponibles sur [MSDN](/samples/browse/). Pour accéder aux exemples depuis la **Boîte à outils SSIS**, cliquez sur le lien **Rechercher des exemples** qui apparaît sous la description.  
  
> [!NOTE]
> Vous ne pouvez pas *supprimer* les composants installés à partir de la boîte à outils.  

## <a name="toolbox-categories"></a>Catégories de la boîte à outils
 Dans la **boîte à outils SSIS**, le flux de contrôle et les composants de flux de données sont organisés en catégories.  Vous pouvez développer et réduire des catégories, et réorganiser des composants.  Restaurez l’organisation par défaut en cliquant avec le bouton droit dans la boîte à outils, puis en cliquant sur **Restaurer les valeurs par défaut de la boîte à outils**.  
  
 Les catégories **Favoris** et **Commun** apparaissent dans la boîte à outils lorsque vous sélectionnez les onglets **Flux de contrôle**, **Flux de données** et **Gestionnaires d'événements** . La catégorie **Autres tâches** apparaît dans la boîte à outils lorsque vous sélectionnez l'onglet **Flux de contrôle** ou l'onglet **Gestionnaires d'événements** . Les catégories **Autres transformations**, **Autres sources** et **Autres destinations** apparaissent dans la boîte à outils lorsque vous sélectionnez l'onglet **Flux de données** .  

 ## <a name="add-azure-components-to-the-toolbox"></a>Ajouter des composants Azure à la boîte à outils  
 Le Feature Pack Azure pour Integration Services contient les gestionnaires de connexions permettant de se connecter aux sources de données Azure, ainsi que les tâches permettant d’effectuer les opérations Azure courantes. Installez le Feature Pack pour ajouter ces éléments à la boîte à outils. Pour plus d’informations, consultez [Feature Pack Azure pour Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

## <a name="move-a-toolbox-item-to-another-category"></a>Déplacer un élément de boîte à outils vers une autre catégorie  
  
1.  Cliquez avec le bouton droit sur un élément de la boîte à outils SSIS, puis cliquez sur l'un des éléments suivants :  
  
    -   **Déplacer vers Favoris**  
  
    -   **Déplacer vers Courant**  
  
    -   **Déplacer vers Autres sources**  
  
    -   **Déplacer vers Autres destinations**  
  
    -   **Déplacer vers Autres transformations**  
  
    -   **Déplacer vers Autres tâches**  
  
## <a name="refresh-the-ssis-toolbox"></a>Actualiser la boîte à outils SSIS  
  
1.  Cliquez avec le bouton droit dans la boîte à outils SSIS, puis cliquez sur **Actualiser boîte à outils**.
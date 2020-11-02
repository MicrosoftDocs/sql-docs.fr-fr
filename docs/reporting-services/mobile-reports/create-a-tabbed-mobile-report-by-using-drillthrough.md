---
title: Créer un rapport mobile tabulé à l’aide de l’extraction | Rapports mobiles Reporting Services | Microsoft Docs
description: Découvrez comment créer un rapport mobile Reporting Services qui s’affiche et fonctionne comme un rapport à onglets à l’aide de l’extraction et des paramètres.
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: c4d5d80d-370a-4a6d-8b76-698bd5ba5ba6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 95b153be1b4dc5a45effeb678ca0ccef83f06e6e
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907337"
---
# <a name="create-a-tabbed-mobile-report-by-using-drillthrough"></a>Créer un rapport mobile tabulé à l’aide de l’extraction
Découvrez comment créer un rapport mobile [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] qui apparaît et fonctionne comme un rapport tabulé à l’aide de l’extraction et des paramètres.

Par exemple, dans ce rapport, les jauges de la partie supérieure fonctionnent comme des onglets. Quand vous cliquez sur la jauge des transports, les données qui se trouvent dans le reste du graphique sont filtrées sur les données relatives aux transports.

![Capture d’écran montrant le rapport Finances - Transport, avec la jauge des transports sélectionnée.](../../reporting-services/mobile-reports/media/tabbed-mobile-report-web-viewer-transportation-complete.png)

En réalité, il s’agit d’un ensemble de cinq rapports, ayant chacun un paramètre différent qui permet de filtrer les données en fonction de la jauge sélectionnée en haut du rapport. Vous commencez par créer cinq rapports. Ensuite, pour chacun des cinq rapports, vous faites des quatre autres jauges des extractions pour les quatre autres rapports.

Voici les étapes de cet exemple :

## <a name="create-the-basic-report"></a>Créer le rapport de base

1. Créez un rapport appelé « Ventes » comportant cinq jauges :

    * Ventes
    * Transport
    * Carburant
    * Stockage
    * Dépenses diverses

   ![Capture d’écran d’un rapport nommé « Ventes », comportant cinq jauges :](../../reporting-services/mobile-reports/media/01-sales-mobile-report-publisher.png)
    
2. Définissez **Accentuation** sur **On** pour la jauge Ventes afin qu’elle contraste avec le reste du rapport ; dans le cas présent, blanc sur noir.

    ![Capture d’écran de la jauge Ventes avec une flèche rouge pointant vers le curseur Accentuation sur la position Activé.](../../reporting-services/mobile-reports/media/01a-sales-accent-mobile-report-publisher.png)
    
3. Enregistrez le rapport sur un serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="make-copies-of-the-report"></a>Effectuer des copies du rapport

1. Faites quatre copies du rapport Ventes et nommez-les : 

    * Transport
    * Carburant
    * Stockage
    * Dépenses diverses

3. Enregistrez-les sur le serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="set-the-gauge-as-a-drillthrough"></a>Définir la jauge comme extraction

Dans cette section, vous définissez chaque jauge (à part la jauge Ventes) comme extraction pour son propre rapport.

1. Dans le rapport Ventes, sélectionnez la jauge Transport.

    ![Capture d’écran du rapport Ventes avec une flèche rouge reliant la jauge Transport à l’option Cible d’extraction.](../../reporting-services/mobile-reports/media/02-sales-create-drillthrough-mobile-report-publisher.png)

2. Sélectionnez l’onglet **Disposition** , puis dans le volet **Propriétés visuelles** , sélectionnez **Cible d’extraction** .

3. Sélectionnez **Rapport mobile** .

4. Accédez au rapport qui sera la destination de l’extraction et sélectionnez-le ; dans le cas présent « Finances - Transport ».

    ![Capture d’écran de la boîte de dialogue Ouvrir à partir du serveur, avec l’option Finances - Transport mise en évidence.](../../reporting-services/mobile-reports/media/03-sales-select-dashboard-mobile-report-publisher.png)

5. Dans **Configurer le rapport cible** , sélectionnez le paramètre de filtrer du rapport, puis sélectionnez **Appliquer** .

   ![Capture d’écran de la section Configurer le rapport cible montrant les paramètres du rapport Finances - Transport.](../../reporting-services/mobile-reports/media/04-sales-apply-parameters-mobile-report-publisher.png)
   
6. Répétez ces étapes pour chaque autre jauge du rapport Ventes. 

## <a name="set-the-gauges-for-the-other-reports"></a>Définir les jauges pour les autres rapports

1.  Ouvrez le rapport Transport, définissez la jauge Ventes comme extraction pour le rapport Ventes, et les trois autres jauges comme des extractions pour leurs propres rapports.

2. Dans le rapport Transport, définissez le paramètre **Accentuation** de la jauge Transport sur **On** de sorte qu’elle contraste avec le reste du rapport.

3. Répétez ces étapes pour les rapports Carburant, Stockage et Dépenses diverses. 

## <a name="view-the-report-in-the-web-portal"></a>Afficher le rapport dans le portail web

1. Accédez au serveur de rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et ouvrez l’un des rapports. 

2. Notez que chaque jauge présente une icône d’extraction en haut à droite.

    ![Capture d’écran de la jauge Carburant.](../../reporting-services/mobile-reports/media/web-viewer-drillthrough-icon-mobile-report-builder.png)

3. Sélectionnez l’une des jauges pour accéder au rapport filtré des données de cette jauge.

   ![Capture d’écran montrant le rapport Finances - Transport avec une flèche rouge pointant vers la jauge Transport, qui est encadrée en rouge.](../../reporting-services/mobile-reports/media/06-mobile-report-web-viewer-transportation.png)

### <a name="see-also"></a>Voir aussi
    
* [Ajouter des paramètres à un rapport mobile](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [Ajouter l’extraction à partir d’un rapport mobile à d’autres rapports mobiles ou des URL](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)




  


---
title: Rechercher des parties de rapports et définir un dossier par défaut (Générateur de rapports) | Microsoft Docs
description: Découvrez comment ajouter des parties de rapports existantes, telles que des tables et des graphiques, à votre rapport à partir de la bibliothèque de parties de rapports dans le Générateur de rapports.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 5cf38068-65d1-4fe8-81f3-a404d8fbc663
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7db8a7ec6a9f4340dcbdf03badb0a77cdbc42591
ms.sourcegitcommit: e572f1642f588b8c4c75bc9ea6adf4ccd48a353b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84778911"
---
# <a name="browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs"></a>Rechercher des parties de rapports et définir un dossier par défaut (Générateur de rapports et SSRS)
Pour créer un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , le plus simple est d’ajouter à votre rapport des parties de rapports existantes, telles que des tables et des graphiques, à partir de la bibliothèque de parties de rapports. Lorsque vous ajoutez une partie de rapport à votre rapport, vous ajoutez également tout ce qu'elle doit comporter pour fonctionner. Par exemple, toute partie de rapport qui affiche des données dépend d'un dataset, autrement dit, une requête et une connexion à une source de données. Après avoir ajouté la partie de rapport à votre rapport, vous pouvez la modifier comme vous le souhaitez.  
  
 Vous pouvez définir un dossier par défaut pour publier des parties de rapports sur le serveur de rapports ou le site SharePoint intégré avec un serveur de rapports.  
  
 Pour plus d’informations, consultez [Publication de parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
## <a name="to-browse-for-report-parts"></a>Pour rechercher des parties de rapports  
  
1.  Dans le menu **Insérer** , cliquez sur **Parties de rapports**.  
  
     Si vous n’êtes pas déjà connecté, cliquez sur **Connectez-vous à un serveur de rapports**, puis entrez le nom.  
  
    > [!NOTE]  
    >  Vous devez être connecté à un serveur de rapports pour rechercher des parties de rapports.  
  
2.  Vous pouvez limiter votre recherche en spécifiant des détails sur la partie de rapport. Tapez tout ou partie du nom et de la description dans la zone de **recherche** ou cliquez sur **Ajouter des critères** et ajoutez des valeurs pour un ou plusieurs de ces champs :  
  
    -   Créé par  
  
    -   Date de création  
  
    -   Date de dernière modification  
  
    -   Auteur de la dernière modification  
  
    -   Dossier du serveur  
  
    -   Type  
  
     Par exemple, pour rechercher une image, cliquez sur **Ajouter des critères**, puis sur **Type**. Dans la zone de liste déroulante, cochez la case **Image** , appuyez sur Entrée, puis cliquez sur la loupe Rechercher.  
  
    > [!NOTE]  
    >  Pour les valeurs **Créé par** et **Auteur de la dernière modification** , recherchez le nom d’utilisateur de la personne tel qu’il est représenté sur le serveur de rapports.  
  
## <a name="to-set-a-default-folder-for-report-parts"></a>Pour définir un dossier par défaut pour les parties de rapport  
  
1.  Cliquez sur **Générateur de rapports**, puis sur **Options**.  
  
2.  Dans la boîte de dialogue **Options** , sous l’onglet **Paramètres** , tapez un nom de dossier dans la zone de texte **Publier les parties de rapports dans ce dossier par défaut** .  
  
 Le Générateur de rapports créera ce dossier si vous êtes autorisé à créer des dossiers sur le serveur de rapports et que le dossier n'existe pas encore.  
  
 Vous n'avez pas à redémarrer le Générateur de rapports pour que ce paramètre entre en vigueur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vérifier la présence de mises à jour ou désactiver les mises à jour (Générateur de rapports et SSRS)](https://msdn.microsoft.com/9c69792d-d7c4-453b-ae2f-6d2d071d8606)   
 [Parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)   
 [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Résoudre les problèmes liés aux parties de rapports (Générateur de rapports et SSRS)](https://msdn.microsoft.com/d9fe1932-46e7-421b-a8a9-4c54d9576e94)   
 [Publier et republier des parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/publish-and-republish-report-parts-report-builder-and-ssrs.md)  
  
  

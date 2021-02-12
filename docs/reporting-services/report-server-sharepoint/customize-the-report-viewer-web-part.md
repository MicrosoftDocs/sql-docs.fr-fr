---
title: Personnaliser le composant WebPart Visionneuse de rapports | Microsoft Docs
description: Vous pouvez utiliser le composant WebPart de la visionneuse de rapports pour afficher les rapports qui s’exécutent sur un serveur SQL Server Reporting Services configuré pour l’intégration SharePoint.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ff03267639a2ea62000c077330b2db547128be7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074780"
---
# <a name="customize-the-report-viewer-web-part"></a>Personnaliser le composant WebPart Visionneuse de rapports

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Vous pouvez utiliser le composant WebPart Visionneuse de rapports pour afficher les rapports qui s’exécutent sur un serveur de rapports configuré pour l’intégration SharePoint. Les rapports consultables comprennent des fichiers de définition de rapport (.rdl) et des rapports Générateur de rapports. Les rapports s’ouvrent automatiquement dans le composant WebPart Visionneuse de rapports dans une nouvelle page. Vous pouvez aussi ajouter un composant WebPart Visionneuse de rapports à une page ou un site web existant si vous voulez qu’un rapport déterminé soit toujours visible dans cette page.

> [!NOTE]
> Même si leurs noms sont identiques, le composant WebPart Visionneuse de rapports installé via le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est différent du composant WebPart Visionneuse de rapports inclus dans le fichier RSWebParts.cab. Les instructions de cette rubrique sont spécifiques au composant WebPart Visionneuse de rapports installé à l’aide du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

 Vous pouvez personnaliser le composant WebPart Visionneuse de rapports comme suit :  
  
-   Modifiez l’aspect du composant WebPart en définissant des propriétés.  
  
-   choisissez quelles fonctionnalités de création de rapport interactives sont disponibles sur la barre d'outils Rapports ;  
  
-   spécifiez quelles zones d'affichage sont disponibles ; Le composant WebPart Visionneuse de rapports compte une zone d’affichage de rapport, une zone de paramètres et une zone d’informations d’identification.  
  
 Vous ne pouvez pas étendre le composant WebPart Visionneuse de rapports pour prendre en charge d’autres types de fichiers et vous ne pouvez pas remplacer la barre d’outils Rapports par une barre d’outils personnalisée ou ajouter de nouvelles fonctionnalités à la barre d’outils existante. Si vous devez personnaliser les fonctionnalités standard, créez un composant WebPart personnalisé.  

## <a name="setting-web-part-properties"></a>Définition des propriétés d’un composant WebPart

 Un composant WebPart a des propriétés personnalisées qui servent à configurer des fonctionnalités spécifiques. Un composant WebPart a aussi des propriétés communes qui se retrouvent dans chaque composant.  
  
### <a name="change-default-properties"></a>Modifier les propriétés par défaut

 Le composant WebPart Visionneuse de rapports a des propriétés par défaut qui conviennent parfaitement à l’ouverture de rapports à la demande à partir d’un dossier ou d’une bibliothèque. Par défaut, tous les contrôles disponibles sont affichés dans la barre d’outils. La hauteur et la largeur sont définies de façon à utiliser tout l’espace disponible dans la page web. Si vous voulez modifier les propriétés par défaut, vous pouvez personnaliser le composant WebPart via **Paramètres du site**.  
  
1.  Dans le menu **Actions de site** , cliquez sur **Paramètres du site**.  
  
2.  Sous Galeries, cliquez sur **Composants WebParts**.  
  
3.  Cliquez sur **ReportViewer.dwp**.  
  
4.  Ouvrez le volet Outils et définissez les propriétés de votre choix.  
  
### <a name="customize-an-embedded-report-viewer-in-a-web-page"></a>Personnaliser une Visionneuse de rapports incorporée dans une page web

 Vous pouvez définir des propriétés pour intégrer la Visionneuse de rapports dans une page web. La Visionneuse de rapports peut utiliser le même style et les couleurs que la page qui la contient. Vous pouvez masquer tout ou une partie de la barre d'outils, de l'explorateur de document et de la zone des paramètres pour maximiser la zone d'affichage des rapports dans l'espace alloué. Le rapport utilise toujours les styles que vous avez définis pour celui-ci au moment de le créer. Vous ne pouvez pas personnaliser l’apparence du rapport après l’avoir publié dans une bibliothèque SharePoint.  
  
 Si vous incorporez le composant WebPart Visionneuse de rapports dans une page web, vous devez attribuer à la propriété **URL de rapport** un rapport spécifique. Dans le cas contraire, la Visionneuse de rapports affiche des instructions de liaison à un rapport. Vous ne pouvez pas personnaliser ou supprimer les instructions.  
  
### <a name="custom-properties-of-the-report-viewer-web-part"></a>Propriétés personnalisées du composant WebPart Visionneuse de rapports

 Lorsque vous définissez des propriétés personnalisées, sachez que certaines propriétés ne sont utilisées que si le composant WebPart Visionneuse de rapports est incorporé dans une page. Les exemples comprennent le titre, la hauteur, la largeur, le style de cadre et la zone. D'autres propriétés, de barre d'outils et de paramètres par exemple, sont utilisées indépendamment de l'affichage de la Visionneuse de rapports dans une page ou de l'ouverture d'un rapport en mode page entière.  
  
 Les propriétés personnalisées du composant WebPart Visionneuse de rapports sont listées ci-dessous.  
  
|Propriété|Description|  
|--------------|-----------------|  
|Rapport|Chemin d'accès complet d'un rapport situé sur le site SharePoint actuel ou sur un site inclus dans la même batterie de serveurs ou application Web. Pour obtenir les résultats optimaux lors de la définition de propriétés supplémentaires, cliquez sur Appliquer après avoir spécifié l'URL du rapport.|  
|Cible du lien hypertexte|Code HTML standard qui spécifie le cadre cible permettant d'afficher le contenu lié au sein du document actuel. Pour les rapports qui comportent des liens hypertexte vers des sites Web externes, vous pouvez spécifier si un document cible doit s'ouvrir à la place du rapport existant dans la fenêtre active, ou s'il doit s'ouvrir dans une nouvelle fenêtre de navigateur. Les valeurs valides sont **_Top**, **_Blank** et **_Self**. **_Top** utilise la fenêtre active, **_Blank** charge le document dans une nouvelle fenêtre de navigateur et **_Self** ouvre le document dans le cadre actif. Bien que **_Parent** soit une valeur valide pour l’attribut Target en langage HTML, ne l’utilisez pas pour un composant WebPart Visionneuse de rapports incorporé dans une page.|  
|Générer automatiquement le titre du composant WebPart|Titre généré qui inclut le nom du composant WebPart Visionneuse de rapports et le nom du rapport, séparés par un tiret. Si le rapport n'a pas de titre, le nom du fichier de rapport est utilisé. Le titre est visible lorsque vous ajoutez un composant WebPart à une page. Si cette case à cocher est activée, le titre est généré chaque fois que la page est actualisée.|  
|Générer automatiquement le lien des détails du titre du composant WebPart|Lien hypertexte généré qui s’affiche au-dessus du composant WebPart. Vous pouvez cliquer sur le lien afin d'ouvrir le rapport dans une nouvelle page, en mode page entière.|  
|Afficher l'élément de menu Générateur de rapports|Affiche ou masque l’option de menu **Actions** pour ouvrir le Générateur de rapports.|  
|Afficher l'élément de menu Abonnement|Affiche ou masque l’option de menu **Actions** pour créer un abonnement pour le rapport.|  
|Afficher l'élément de menu Imprimer|Affiche ou masque l’option de menu **Actions** pour imprimer le rapport.|  
|Afficher l'élément de menu Exporter|Affiche ou masque l’option de menu **Actions** pour exporter le rapport.|  
|Afficher le bouton Actualiser|Affiche ou masque le bouton Actualiser sur la barre d'outils.|  
|Afficher les commandes de navigation entre les pages|Affiche ou masque les boutons de navigation dans les rapports sur la barre d'outils. Cette option modifie la visibilité de tous les contrôles de navigation.|  
|Afficher le bouton Précédente|Affiche ou masque le bouton Précédente sur la barre d'outils.|  
|Afficher les commandes de recherche|Affiche ou masque les commandes de recherche sur la barre d'outils. Les commandes de recherche autorisent un utilisateur à rechercher le texte dans le rapport rendu. Cette option modifie la visibilité de toutes les commandes de recherche.|  
|Afficher le contrôle de zoom|Affiche ou masque le contrôle de zoom sur la barre d'outils.|  
|Afficher le bouton Flux Atom|Affiche ou masque le bouton Flux Atom sur la barre d'outils.<br /><br /> ![htmlviewer_datafeed](../../reporting-services/media/htmlviewer-datafeed.gif "htmlviewer_datafeed")|  
|Emplacement de la barre d'outils|Détermine l'emplacement de la barre d'outils dans la visionneuse de rapports. Les valeurs possibles sont **Top** et **Bottom**.|  
|Zone de message|Les valeurs valides sont **Affiché**, **Réduit** et **Caché**. **Affiché** affiche de la zone Paramètres pour les rapports qui comportent des valeurs paramétrables et qui nécessitent une entrée utilisateur comme condition d’exécution du rapport. Utilisez **Caché** si tous les paramètres du rapport sont spécifiés et si vous ne voulez pas que la zone des paramètres soit visible par les utilisateurs.|  
|Largeur de la zone des paramètres|Vous pouvez choisir l'unité de mesure et la valeur. La valeur par défaut est 200 pixels. Cette propriété doit respecter une seule exigence, elle doit être supérieure à zéro.|  
|Explorateur de documents|Contrôle de navigation entre les rapports qui est défini dans le rapport et qui permet d'accéder en un seul clic à des sections spécifiques d'un rapport. Il est disponible dans les rapports HTML. L'Explorateur de documents s'affiche dans une zone réductible en regard de la zone d'affichage du rapport. Les valeurs valides sont **Affiché**, **Réduit** et **Caché**. Si un Explorateur de documents est défini pour un rapport, la zone est développée par défaut, à moins qu’elle ne soit cachée ou réduite via les propriétés du composant WebPart. Si l'Explorateur de documents est réduit, vous pouvez cliquer sur la flèche pour le développer.|  
|Largeur de la zone de l'explorateur de documents|Vous pouvez choisir l'unité de mesure et la valeur. La valeur par défaut est 200 pixels. Cette propriété doit respecter une seule exigence, elle doit être supérieure à zéro.|  
|Charger les paramètres|Récupérez les propriétés de paramètre pour le rapport. Tous les rapports n'ont pas des paramètres. Si le rapport n'a pas de paramètres, aucune valeur n'est retournée. Si vous définissez les propriétés d'un rapport que vous venez de télécharger, vous risquez de recevoir une erreur indiquant que la connexion à la source de données a été supprimée. Si cela se produit, réinitialisez la connexion, puis terminez la définition des propriétés des paramètres une fois la connexion spécifiée. Pour plus d’informations sur la définition de la connexion, consultez [Créer et gérer des sources de données partagées &#40;Reporting Services en mode intégré SharePoint&#41;](/previous-versions/sql/).<br /><br /> Pour obtenir des résultats optimaux, cliquez sur **Appliquer** avant de cliquer sur Charger les paramètres.<br /><br /> Une fois que vous avez chargé les propriétés des paramètres, vous pouvez les définir de la même façon que dans les pages de propriétés des paramètres du rapport. Pour plus d’informations sur la définition des paramètres, consultez [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).|  

## <a name="customizing-the-toolbar"></a>Personnalisation de la barre d’outils

 La barre d'outils s'affiche sous le titre et s'étend sur toute la partie supérieure du rapport. La barre d’outils fournit un menu **Actions** , la navigation entre les pages pour les rapports paginés, l’actualisation et le zoom. Elle inclut un contrôle d'Explorateur de documents pour les rapports qui disposent d'un Explorateur de documents. Le menu **Actions** comprend des commandes permettant d’exporter un rapport, de rechercher du texte ou des nombres dans un rapport, d’imprimer un rapport et d’ouvrir le rapport dans le Générateur de rapports.  
  
 Vous ne pouvez pas ajouter de nouvelles commandes au menu  **Actions** mais vous pouvez le personnaliser en modifiant les options visibles aux utilisateurs. Pour modifier la visibilité des boutons et des contrôles de la barre d’outils, modifiez les options dans la section **Visibilité des éléments de barre d’outils** du composant WebPart. Vous pouvez également supprimer la commande **Imprimer** ou des formats d’exportation spécifiques en rendant ces fonctionnalités inaccessibles sur le serveur de rapports. Les contrôles de navigation entre les pages sont disponibles pour les rapports qui comportent des sauts de page ; à défaut, le rapport est constitué d'une seule page de longueur variable. La commande **Actualiser** permet de traiter à nouveau le rapport à l’aide des paramètres actuels du rapport. Pour afficher tous les contrôles sur une ligne, attribuez au moins une valeur de 400 pixels à la largeur globale du composant WebPart.  

## <a name="customizing-the-viewing-area"></a>Personnalisation de la zone d’affichage

 Une zone d'affichage sert à afficher les rapports. La zone d'affichage du rapport est partagée avec les zones Paramètres et Informations d'identification, si celles-ci sont utilisées. Si des informations d'identification sont requises, la zone Informations d'identification s'affiche en regard d'une zone d'affichage du rapport vide. La zone Informations d'identification disparaît une fois que l'utilisateur fournit les informations d'identification et qu'il exécute le rapport. Pour personnaliser le texte invitant les utilisateurs à fournir des informations d’identification, modifiez les propriétés de connexion de la source de données. Pour plus d’informations, consultez [Créer et gérer des sources de données partagées &#40;Reporting Services en mode intégré SharePoint&#41;](/previous-versions/sql/).  
  
 La zone Paramètres fournit des champs permettant d'entrer des valeurs avant l'exécution du rapport. Elle n'est utilisée que lorsqu'une définition de rapport comprend des paramètres. Lorsque la zone Paramètres ou Informations d’identification est visible, l’affichage du rapport est ajusté en fonction de la largeur restante du composant WebPart. Vous pouvez définir les propriétés du composant WebPart afin de personnaliser la largeur de la zone Paramètres. Vous pouvez également définir les étiquettes qui s'affichent en regard des paramètres individuels sur la page. Pour plus d’informations sur la modification des étiquettes de paramètres, consultez [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
## <a name="see-also"></a>Voir aussi

 [Composant WebPart Visionneuse de rapports sur un site SharePoint](./report-viewer-web-part-sharepoint-site.md)   
 [Ajouter le composant Web Visionneuse de rapports à une page web](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
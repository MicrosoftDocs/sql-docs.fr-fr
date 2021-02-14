---
title: Exporter des rapports (Générateur de rapports) | Microsoft Docs
description: Vous pouvez exporter un rapport Reporting Services dans un autre format de fichier, tel que PowerPoint ou PDF, à l’aide du Générateur de rapports, du Concepteur de rapports ou du serveur de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d82c6f45d6cd8e4e35735291433d3e054afe5481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024105"
---
# <a name="export-reports-report-builder-and-ssrs"></a>Exporter des rapports (Générateur de rapports et SSRS)

  Vous pouvez exporter un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans un autre format (PowerPoint, Image, PDF, [!INCLUDE[ofprword](../../includes/ofprword-md.md)]ou [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] ) ou en générant un document de service Atom qui répertorie les flux de données compatibles Atom disponibles dans le rapport. Vous pouvez exporter votre rapport à partir du Générateur de rapports, du Concepteur de rapports ([!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]) ou du serveur de rapports.  
  
 L'exportation d'un rapport permet de réaliser les opérations suivantes :  
  
-   **Travailler sur les données du rapport dans une autre application.** Par exemple, vous pouvez exporter votre rapport vers Excel et continuer à utiliser les données dans Excel.  
  
-   **Imprimer le rapport dans un autre format.** Par exemple, vous pouvez exporter le rapport au format de fichier PDF, puis l'imprimer.  
  
-   **Enregistrer une copie du rapport sous un autre type de fichier.** Par exemple, vous pouvez exporter un rapport vers Word et l'enregistrer, en créant une copie du rapport.  
  
-   **Utiliser les données de rapport comme flux dans les applications.** Par exemple, vous pouvez générer des flux de données conformes à Atom et exploitables par Power Pivot ou Power BI, puis utiliser les données dans Power Pivot ou Power BI. Pour plus d’informations, consultez [Générer des flux de données à partir d’un rapport](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
-   Le rendu du rapport sur le serveur de rapports est utile lorsque vous définissez des abonnements ou remettez vos rapports via messagerie électronique, ou si vous souhaitez enregistrer un rapport qui est disponible sur le serveur de rapports. Pour plus d’informations, consultez [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit de nombreuses extensions de rendu et prend en charge les exportations de rapports dans les formats de fichiers usuels. Les extensions de rendu prennent en charge les formats de fichiers comportant des sauts de page conditionnels (Word ou Excel, par exemple), des sauts de page manuels (formats PDF ou TIFF, par exemple) ou des données uniquement (formats CSV ou XML conforme à Atom, par exemple).  
  
 La pagination du rapport peut être affectée lorsque vous exportez un rapport sous un format différent. Lorsque vous affichez un aperçu du rapport, vous visualisez le rapport tel qu'il est rendu par l'extension de rendu HTML, qui respecte les règles de saut de page conditionnelle. Lorsque vous exportez un rapport vers un format de fichier différent, tel qu'Adobe Acrobat (PDF), la pagination est basée sur la taille de page physique, qui respecte les règles de saut de page manuel. Les pages peuvent également être séparées par des sauts de page logiques que vous ajoutez à un rapport, mais la longueur réelle d'une page varie selon le type de convertisseur que vous utilisez. Pour modifier la pagination de votre rapport, vous devez comprendre le comportement de pagination de l'extension de rendu que vous choisissez. Vous devrez peut-être ajuster la conception de votre disposition de rapport pour cette extension de rendu. Pour plus d’informations, consultez [Mise en page et rendu](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-export-a-report-from-report-builder"></a><a name="bkmk_export_from_rb"></a> Pour exporter un rapport du Générateur de rapports

1.  Créez le rapport ou affichez son aperçu.  
  
2.  Cliquez sur **Exporter** dans le ruban.  
  
     ![Exporter - Générateur de rapports](../../reporting-services/report-builder/media/ssrb-export.png "Exporter - Générateur de rapports")  
  
3.  Sélectionnez le format que vous souhaitez utiliser.  
  
     La boîte de dialogue **Enregistrer sous** s’affiche. Par défaut, le nom du fichier correspond à celui du rapport que vous avez exporté. Vous pouvez éventuellement modifier le nom du fichier.  
  
##  <a name="to-export-a-report-from-the-ssrsnoversion-web-portal"></a><a name="bkmk_export_from_rm"></a> Pour exporter un rapport à partir du portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
1.  À partir de la page [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Accueil **du portail web** , naviguez jusqu’au rapport que vous souhaitez exporter.  
  
2.  Cliquez sur le rapport à afficher et affichez son aperçu.  
  
3.  Dans la barre d’outils Visionneuse de rapports, cliquez sur la flèche déroulante **Exporter** .  
  
     ![Exporter - Portail web Reporting Services](../../reporting-services/report-builder/media/ssrsportal-export.png "Exporter - Portail web Reporting Services")  
  
4.  Sélectionnez le format que vous souhaitez utiliser.  
  
5.  Cliquez sur **Exporter**. Un message vous demande si vous souhaitez ouvrir ou enregistrer le fichier.  
  
6.  Pour afficher le rapport dans le format d'exportation sélectionné, cliquez sur **Ouvrir**.  
  
     \- ou -  
  
     Pour enregistrer immédiatement le rapport dans le format d'exportation sélectionné, cliquez sur **Enregistrer**.  
  
     À l'aide de l'application associée au format que vous avez choisi, le rapport est affiché ou enregistré. Si vous cliquez sur **Enregistrer**, vous devrez indiquer un emplacement pour enregistrer votre rapport.  
  
##  <a name="to-export-a-report-from-a-sharepoint-library"></a><a name="bkmk_export_from_sharepoint"></a> Pour exporter un rapport à partir d’une bibliothèque SharePoint  
  
1.  Affichez l'aperçu du rapport.  
  
2.  Dans la barre d'outils, cliquez sur **Actions**, pointez sur **Exporter**, puis sélectionnez le format à utiliser.  
  
     La boîte de dialogue **Téléchargement de fichiers** s'affiche.  
  
3.  Pour afficher le rapport dans le format d'exportation sélectionné, cliquez sur **Ouvrir**.  
  
     \- ou -  
  
     Pour enregistrer immédiatement le rapport dans le format d'exportation sélectionné, cliquez sur **Enregistrer**.  
  
     À l'aide de l'application associée au format que vous avez choisi, le rapport est affiché ou enregistré. Si vous cliquez sur **Enregistrer**, vous devrez indiquer un emplacement pour enregistrer votre rapport.  
  
     Modifiez éventuellement le nom de fichier du rapport exporté.  
  
     **Remarque** Si le programme ne peut pas ouvrir le rapport dans le format que vous avez choisi car vous ne disposez pas d'un programme associé à ce type de fichier, vous serez invité à enregistrer le rapport exporté ou à rechercher un programme en ligne pour ouvrir le rapport.  
  
##  <a name="rendering-extension-types"></a><a name="RendererTypes"></a> Types d'extensions de rendu  
 Il existe trois types d'extensions de rendu [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   **Extensions de convertisseurs de données** Les extensions de rendu de données suppriment du rapport toute la mise en forme et les informations relatives à la disposition et affichent uniquement les données. Le fichier résultant peut être utilisé pour importer les données de rapport brutes dans un autre type de fichier, tel qu'Excel, une autre base de données, un message de données XML ou une application personnalisée. Les convertisseurs de données ne prennent pas en charge les sauts de page.  
  
     Les extensions de rendu de données suivantes sont prises en charge : CSV, XML et Atom.  
  
-   **Extensions de convertisseurs de saut de page conditionnelle** Les extensions de rendu de saut de page conditionnelle conservent la disposition et la mise en forme du rapport. Le fichier résultant est optimisé pour l’affichage à l’écran et la remise, par exemple sur une page web ou dans les contrôles **ReportViewer** .  
  
     Les extensions de rendu de saut de page conditionnel suivantes sont prises en charge : [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word et archive web (MHTML).  
  
-   **Extensions de rendu de saut de page manuel** Les extensions de convertisseurs de saut de page manuel conservent la disposition et la mise en forme du rapport. Le fichier résultant est optimisé pour une impression cohérente ou pour l'affichage en ligne du rapport dans un format de livre.  
  
     Les extensions de rendu de saut de page manuel suivantes sont prises en charge : TIFF et PDF.  
  
##  <a name="formats-you-can-export-while-viewing-reports"></a><a name="ExportFormats"></a> Formats d’exportation disponibles pendant l’affichage de rapports  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit des extensions de rendu qui affichent les rapports dans divers formats. Il est conseillé d’optimiser la conception des rapports en fonction du format de fichier choisi.  Le tableau suivant liste les formats d’exportation disponibles dans l’interface utilisateur.  D’autres formats vous sont proposés avec des abonnements à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou si vous effectuez l’exportation à partir de l’URL d’accès.  Consultez la section [Autres modes d'exportation des rapports](#OtherWaysExportingReports)dans cette rubrique.  
  
|Format|Type d'extension de rendu|Description|  
|------------|------------------------------|-----------------|  
|Fichier Acrobat (PDF)|Saut de page manuel|L'extension de rendu PDF présente les rapports sous forme de fichiers s'affichant dans des visionneuses comme Adobe Acrobat si elles prennent en charge le format PDF 1.3. Bien que PDF 1.3 soit compatible avec Adobe Acrobat 4.0 et version ultérieure, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge Adobe Acrobat 6 ou version ultérieure. Cette extension de rendu ne nécessite pas les logiciels Adobe pour effectuer le rendu du rapport. Toutefois, les visionneuses PDF comme Adobe Acrobat sont indispensables pour afficher ou imprimer un rapport au format PDF.<br /><br /> Pour plus d’informations, consultez [Exportation vers un fichier PDF](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md).|  
|Atom|Données|L'extension de rendu Atom génère des flux conformes à Atom à partir des rapports. Les flux de données peuvent être lus et échangés avec des applications, comme Power Pivot ou Power BI, qui utilisent des flux de données conformes à Atom.<br /><br /> La sortie est un document de service Atom qui répertorie les flux disponibles à partir d'un rapport. Au moins un flux est créé pour chaque région de données dans un rapport. Selon le type de région de données et les données affichées par cette région, plusieurs flux peuvent être générés.<br /><br /> Pour plus d’informations, consultez [Génération de flux de données à partir de rapports](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).|  
|CSV|Données|L'extension de rendu CSV (valeurs séparées par des virgules) permet de rendre les rapports sous la forme d'une représentation aplatie des données d'un rapport dans un format standardisé, texte brut qui peut être facilement lu et échangé avec de nombreuses applications.<br /><br /> Pour plus d’informations, consultez [Exportation vers un fichier PDF](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md).|  
|EXCELOPENXML|Saut de page conditionnelle|Affiché avec la mention « Excel » dans les menus d’exportation lors de la consultation de rapports. L’extension de rendu Excel affiche un rapport au format Excel (.xslx) compatible avec [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2013.  Pour plus d’informations, consultez [Exportation vers Microsoft Excel](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).|  
|PowerPoint|Saut de page manuel|l’extension de rendu PowerPoint affiche un rapport au format PowerPoint (.pptx) compatible avec PowerPoint 2013.|  
|Fichier TIFF|Saut de page manuel|L'extension de rendu de type image effectue le rendu d'un rapport dans un fichier bitmap ou un métafichier. Par défaut, l'extension de rendu de type image génère un fichier TIFF du rapport, qui peut être présenté dans plusieurs pages. Lorsque le client reçoit l'image, il peut l'afficher dans une visionneuse d'images et l'imprimer.<br /><br /> L'extension de rendu de type image peut générer des fichiers dans l'un des formats pris en charge par [!INCLUDE[ndptecgdiplus](../../includes/ndptecgdiplus-md.md)] : BMP, EMF, EMFPlus, GIF, JPEG, PNG et TIFF.<br /><br /> Pour plus d’informations, consultez [Exportation vers un fichier image](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md).|  
|Archive Web|Saut de page conditionnelle|L'extension de rendu HTML effectue le rendu d'un rapport au format HTML. Elle peut également produire des pages HTML entièrement formées ou des fragment HTML à incorporer dans d'autres pages HTML. La sortie HTML est générée avec l'encodage UTF-8.<br /><br /> L’extension de rendu HTML représente l’extension de rendu par défaut pour les rapports qui sont prévisualisés dans le Générateur de rapports et qui s’affichent dans un navigateur, y compris pendant une exécution dans portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Pour plus d’informations, consultez [Rendu au format HTML](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md).|  
|WORDOPENXML|Saut de page conditionnelle|Affiché avec la mention « Word » dans le menu d’exportation lors de l’affichage des rapports. L’extension de rendu Word génère un rapport au format Word (.docx) compatible avec [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2013.  Pour plus d’informations, consultez [Exportation vers Microsoft Word](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md).|  
|XML|Données|L'extension de rendu XML rend un rapport au format XML. Le schéma du rapport XML est spécifique du rapport et contient uniquement des données. Les informations de mise en page ne sont pas rendues et la pagination n'est pas conservée par l'extension de rendu XML. La sortie XML générée par cette extension peut être importée dans une base de données, utilisée en tant que message de données XML ou envoyée à une application personnalisée.<br/><br/> Pour plus d’informations, consultez [Exportation vers XML](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md).|  
  
##  <a name="generating-data-feeds-from-a-report"></a><a name="GeneratingDataFeedsFromReport"></a> Génération de flux à partir d'un rapport  
 Pour générer des flux à partir d’un rapport, exécutez ce dernier dans le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis cliquez sur l’icône **Générer un flux** dans la barre d’outils du portail web. Vous êtes invité à enregistrer ou à ouvrir le fichier. Si vous avez choisi **Ouvrir**, le document de service Atom s'ouvre dans l'application associée à l'extension de fichier .atomsvc. Si vous avez choisi **Enregistrer**, le document est enregistré en tant que fichier .atomsvc. Par défaut, le nom du fichier correspond au nom du rapport. Vous pouvez remplacer ce nom par un autre plus explicite.  
  
 Vous enregistrez le document de service Atom sur votre ordinateur. Ultérieurement, vous pouvez le télécharger vers un serveur de rapports ou tout autre serveur afin de le rendre accessible aux autres utilisateurs. Pour plus d’informations, consultez [Génération de flux de données à partir de rapports](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md) et [Générer des flux de données à partir d’un rapport](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
##  <a name="troubleshooting-exported-reports"></a><a name="Troubleshooting"></a> Dépannage des rapports exportés  
 Vos rapports peuvent parfois avoir une apparence différente ou ne pas fonctionner comme vous le voulez après les avoir exportés dans un format différent. Cela se produit parce que certaines règles et limitations peuvent s'appliquer au convertisseur. Vous pouvez prendre en compte de nombreuses limitations lors de la création du rapport. Vous devrez peut-être utiliser une mise en page légèrement différente dans votre rapport, aligner soigneusement les éléments du rapport, restreindre les pieds de page du rapport à une seule ligne de texte, etc.  
  
 Si votre rapport contient du texte Unicode avec des chiffres arabes, ou s'il contient des dates en chiffres arabes, les dates et les nombres ne sont pas affichés correctement lorsque vous imprimez le rapport ou que vous l'exportez dans l'un des formats suivants.  
  
-   PDF  
  
-   Word  
  
-   Excel  
  
-   Image/TIFF  
  
 Si vous exportez le rapport au format HTML, les dates et les nombres sont affichés correctement.  
  
 Les rubriques relatives à des convertisseurs spécifiques décrivent comment les éléments de rapport et les régions de données sont restitués, ainsi que les limitations et les solutions pour chaque convertisseur.  
  
-   [Exportation vers un fichier CSV &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)  
  
-   [Exportation vers Microsoft Excel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)  
  
-   [Exportation vers Microsoft Word &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)  
  
-   [Rendu au format HTML &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)  
  
-   [Exportation vers un fichier PDF &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)  
  
-   [Exportation vers un fichier image &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)  
  
-   [Exportation vers XML &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)  
  
-   [Génération de flux de données à partir de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] propose des fonctionnalités supplémentaires qui vous permettent de créer des rapports qui fonctionnent correctement dans d'autres formats. Les sauts de page sur les régions de données de tableau matriciel (table, matrice et liste), les groupes et les rectangles vous permettent de mieux contrôler la pagination du rapport. Les pages de rapport, délimitées par les sauts de page, peuvent avoir des noms différents et une numérotation redéfinie. À l'aide des expressions, les noms et numéros des pages peuvent être mis à jour dynamiquement lorsque le rapport est exécuté. Pour plus d’informations, voir [Pagination dans Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
 Par ailleurs, vous pouvez utiliser la fonction globale intégrée RenderFormat pour appliquer de manière conditionnelle des mises en page de rapport différentes selon le type de convertisseur. Pour plus d’informations, consultez [Références à des champs Globals et Users prédéfinis](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).

##  <a name="other-ways-of-exporting-reports"></a><a name="OtherWaysExportingReports"></a> Autres modes d'exportation des rapports  
 L’exportation d’un rapport est une tâche à la demande que vous effectuez quand le rapport est ouvert dans le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou le Générateur de rapports. Pour automatiser une opération d'exportation (par exemple l'exportation d'un rapport vers un dossier partagé en tant que type de fichier spécifique, selon une planification récurrente), créez un abonnement chargé de remettre le rapport dans un dossier partagé. Pour plus d'informations, consultez [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
 Les rapports prévisualisés dans les outils de création de rapports ou ouverts dans une application de navigation telle que le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont toujours rendus en premier au format HTML. Vous ne pouvez pas spécifier une autre extension de rendu par défaut pour l'affichage. Toutefois, vous pouvez créer un abonnement qui produit un rapport dans le format de rendu de votre choix afin de le remettre ultérieurement dans une boîte de réception de courrier électronique ou un dossier partagé. Pour plus d’informations, consultez [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) et [Créer, modifier ou supprimer des abonnements pilotés par les données](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md).  
  
 Vous pouvez également accéder à un rapport via une URL qui spécifie une extension de rendu en tant que paramètre d'URL et qui effectue le rendu du rapport directement au format spécifié sans l'afficher en premier au format HTML. L'exemple suivant affiche un rapport au format Excel :  
  
```  
https://<Report Server Name>/reportserver?/Sales/YearlySalesSummary&rs:Format=Excel&rs:Command=Render  
```  
  
 et ce qui suit affiche un rapport PowerPoint à partir d’une instance nommée :  
  
```  
https://<Report Server Name/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Pour plus d’informations, consultez [Export a Report Using URL Access](../../reporting-services/export-a-report-using-url-access.md).  

## <a name="next-steps"></a>Étapes suivantes

[Contrôle des sauts de page, des en-têtes, des colonnes et des lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
[Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
[Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
[Enregistrement des rapports &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

---
title: Exportation vers Microsoft Excel (Générateur de rapports) | Microsoft Docs
description: Dans le Générateur de rapports, l’extension de rendu Excel affiche un rapport paginé au format Office Open XML à ouvrir dans Microsoft Excel.
ms.date: 12/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 74f726fc-2167-47af-9093-1644e03ef01f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4e8b43d3e9e772b06961602bf2d8bd69aa51216e
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878852"
---
# <a name="exporting-to-microsoft-excel-report-builder-and-ssrs"></a>Exportation vers Microsoft Excel (Générateur de rapports et SSRS)
  L’extension de rendu Excel de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] restitue un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] au format [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (.xlsx). Avec l'extension de rendu Excel, la largeur des colonnes dans Excel correspond plus précisément à la largeur des colonnes dans les rapports.  
  
 Il s'agit du format Office Open XML. Le type de contenu des fichiers générés par ce convertisseur est **application/vnd.openxmlformats-officedocument.spreadsheetml.sheet** et l’extension des fichiers est .xlsx.  
  
 Vous pouvez modifier certains paramètres par défaut pour ce convertisseur en modifiant les paramètres d'informations de périphérique. Pour plus d’informations, voir [Excel Device Information Settings](../../reporting-services/excel-device-information-settings.md).  
  
 Pour plus d’informations sur l’exportation vers Excel, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
> [!IMPORTANT]  
>  Quand vous définissez un paramètre de type **Chaîne**, la zone de texte qui apparaît vous permet d’entrer n’importe quelle valeur. Si un paramètre de rapport n'est pas directement lié à un paramètre de requête et les valeurs de paramètre sont incluses dans le rapport, un utilisateur de rapport peut taper une syntaxe d'expression, un script ou une URL dans la valeur de paramètre et effectuer le rendu du rapport dans Excel. Si un autre utilisateur affiche ensuite le rapport et clique sur le contenu du paramètre de rendu, celui-ci peut exécuter accidentellement le lien ou le script malveillant.  
>   
>  Pour réduire le risque d'exécution accidentelle de scripts malveillants, ouvrez les rapports rendus uniquement à partir de sources approuvées. Pour plus d’informations sur la sécurisation des rapports, consultez [Sécurisation des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).  
  
##  <a name="excel-limitations"></a><a name="ExcelLimitations"></a> Limitations d'Excel  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] impose des limitations aux rapports exportés en raison des fonctionnalités d'Excel et de ses formats de fichiers. Les plus significatives sont les suivantes :  
  
-   La largeur de colonne maximum est limitée à 255 caractères ou de 1 726,5 points. Le convertisseur ne vérifie pas que la largeur de colonne est inférieure à cette limite.  
  
-   Le nombre de caractères maximum dans une cellule est limité à 32 767. Si cette valeur est dépassée, le convertisseur affiche un message d'erreur.  
  
-   La hauteur de ligne maximum est de 409 points. Si la hauteur de ligne dépasse 409 points afin de s’ajuster au contenu de la ligne, la cellule Excel affiche une partie du texte jusqu’à 409 points. Le reste du contenu de la cellule se trouve toujours dans la cellule (jusqu’au nombre maximal de caractères dans Excel qui est de 32 767).

-  Étant donné que la hauteur de ligne maximale est de 409 points, si la hauteur de cellule dans le rapport est supérieure à 409 points, Excel fractionne le contenu de la cellule en plusieurs lignes.
  
-   Le nombre maximal de feuilles de calcul n'est pas défini dans Excel, mais des facteurs externes, tels que la mémoire et l'espace disque, peuvent entraîner des limitations.  
  
-   Dans les plans, Excel autorise jusqu'à sept niveaux imbriqués.  
  
-   Si l'élément de rapport qui contrôle si un autre élément est affiché/masqué ne se trouve pas dans la ligne ou la colonne précédente ou suivante par rapport à l'élément qui est affiché/masqué, le plan est également désactivé.  
  
 Pour plus d’informations sur les limitations d’Excel, consultez [Spécifications et limites relatives à Excel](https://support.microsoft.com/en-us/office/excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3).  
  
### <a name="sizes-of-excel-2003-xls-files"></a>Taille des fichiers Excel 2003 (.xls)  
  
> [!IMPORTANT]  
>  L'extension de rendu [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 est déconseillée. Pour plus d’informations, consultez [Fonctions déconseillées de SQL Server Reporting Services dans SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 Lorsque les rapports sont exportés et enregistrés pour la première fois dans Excel 2003, ils ne bénéficient pas de l'optimisation de fichier qu'Excel applique automatiquement aux fichiers de classeur .xls. Cette taille de fichier plus élevée peut être à l'origine de problèmes dans les abonnements par messagerie électronique et les pièces jointes aux messages. Pour réduire la taille des fichiers \*.xls pour les rapports exportés, ouvrez les fichiers \*.xls, puis enregistrez de nouveau les classeurs. La taille des fichiers est alors généralement réduite de 40 à 50 pour cent.  
  
> [!NOTE]  
>  Dans Excel 2003, environ 1 000 caractères sont affichés dans une cellule Excel sur la feuille de calcul, mais le nombre maximal de caractères peut être modifié dans la barre de formule. Cette limitation ne s’applique pas aux fichiers Excel actuels (.xlsx).  
  
### <a name="text-boxes-and-text"></a>Zones de texte et texte  
 Les limitations suivantes s'appliquent aux zones de texte et au texte :  
  
-   Les valeurs des zones de texte qui sont des expressions ne sont pas converties en formules Excel. La valeur de chaque zone de texte est évaluée durant le traitement du rapport. L'expression évaluée est exportée en tant que contenu de chaque cellule Excel.  
  
-   Les zones de texte sont rendues dans une cellule Excel. La taille, le type, les ornements et le style de police sont les seuls éléments de mise en forme pris en charge sur du texte dans une cellule Excel.  
  
-   L'effet de texte « Surligné » n'est pas pris en charge dans Excel.  
  
-   Excel ajoute une marge intérieure par défaut d'environ 3,75 points à gauche et à droite des cellules. Si les paramètres de marge intérieure d’une zone de texte sont inférieurs à 3,75 points et ne sont assez larges pour accueillir le texte, le texte est renvoyé à la ligne dans Excel.  
  
    > [!NOTE]  
    >  Pour corriger ce problème, augmentez la largeur de la zone de texte dans le rapport.  
  
### <a name="images"></a>Images  
 Les limitations suivantes s'appliquent aux images :  
  
-   Les images d'arrière-plan pour les éléments de rapport sont ignorées, car Excel ne prend pas en charge les images d'arrière-plan pour les cellules individuelles.  
  
-   L'extension de rendu Excel prend en charge uniquement l'image d'arrière-plan du corps de rapport. Si une image d'arrière-plan de corps du rapport est affichée dans le rapport, l'image est rendue sous la forme d'une image d'arrière-plan de feuille de calcul.  
  
### <a name="rectangles"></a>Rectangles  
 La limitation suivante s'applique aux rectangles :  
  
-   Les rectangles dans les pieds de page de rapport ne sont pas exportés vers Excel. Toutefois, les rectangles dans le corps du rapport, les cellules de tableau matriciel, etc. sont restitués sous forme de plage de cellules Excel.  
  
### <a name="report-headers-and-footers"></a>En-têtes et pieds de page de rapport  
 Les limitations suivantes s'appliquent aux en-têtes et pieds de page de rapport :  
  
-   Les en-têtes et les pieds de page Excel prennent en charge un maximum de 256 caractères, y compris la balise. L'extension de rendu tronque la chaîne à 256 caractères.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne prend pas en charge les marges dans les en-têtes et pieds de page de rapport. En cas d'exportation vers Excel, ces valeurs de marge sont définies sur zéro et tout en-tête ou pied de page contenant plusieurs lignes de données peut ne pas imprimer plusieurs lignes, en fonction des paramètres de l'imprimante.  
  
-   Les zones de texte dans un en-tête ou un pied de page conservent leur mise en forme mais pas leur alignement en cas d'exportation vers Excel. En effet, les espaces de début et de fin sont supprimés lorsque le rapport est restitué dans Excel.  
  
### <a name="merging-cells"></a>Fusion de cellules  
 La limitation suivante s'applique à la fusion de cellules :  
  
-   Si des cellules sont fusionnées, le retour automatique à la ligne ne fonctionne pas correctement. S’il existe des cellules fusionnées sur une ligne sur laquelle une zone de texte est restituée avec la propriété AutoSize, le redimensionnement automatique ne fonctionne pas.  
  
 Le convertisseur Excel est essentiellement un convertisseur de mise en page. Sa mission étant de répliquer aussi fidèlement que possible la mise en page du rapport rendu dans une feuille de calcul Excel, certaines cellules peuvent être fusionnées dans la feuille de calcul afin de conserver la mise en page du rapport. Les cellules fusionnées peuvent provoquer des problèmes car les fonctionnalités de tri d'Excel requièrent que les cellules soient fusionnées d'une façon très spécifique pour que le tri fonctionne correctement. Par exemple, Excel requiert que les plages de cellules fusionnées aient la même taille pour pouvoir être triées.  
  
 S'il est important que les rapports exportés vers des feuilles de calcul Excel puissent être triés, les remarques suivantes peuvent vous aider à réduire le nombre de cellules fusionnées dans vos feuilles de calcul Excel, qui est la principale cause des difficultés rencontrées avec la fonctionnalité de tri d'Excel.  
  
-   Le non-alignement à droite et à gauche des éléments est la cause la plus commune de cellules fusionnées. Assurez-vous que les bords droit et gauche de tous les éléments de rapport sont alignés les uns avec les autres. Le fait d'aligner les éléments et de leur donner la même largeur résoudra le problème dans la majorité des cas.  
  
-   Même après alignement précis de tous les éléments, il peut arriver dans de rares cas que certaines colonnes continuent d'être fusionnées. Cela peut être dû à la conversion d'unité interne et à l'arrondi lorsque la feuille de calcul Excel est restituée. Dans le langage RDL (Report Definition Language), vous pouvez spécifier la position et la taille dans des unités de mesure différentes comme les pouces, les pixels, les centimètres et les points. En interne, Excel utilise les points. Pour réduire la conversion et l'imprécision potentielle de l'arrondi lors de la conversion des pouces et des centimètres en points, envisagez de spécifier toutes les mesures en points entiers pour éviter toute complication. Un centimètre est égal à 28,35 points.  
  
### <a name="report-row-groups-and-column-groups"></a>Groupes de lignes et groupes de colonnes de rapport  
 Les rapports qui incluent des groupes de lignes ou des groupes de colonnes contiennent des cellules vides en cas d'exportation vers Excel. Imaginez un rapport qui regroupe des lignes relatives à des distances de trajet domicile-travail. Chaque distance de trajet domicile-travail peut contenir plusieurs clients. L'illustration suivante montre ce rapport.  
  
 ![Rapport dans le portail web Reporting Services](../../reporting-services/report-builder/media/ssrb-excelexportssrs.png "Rapport dans le portail web Reporting Services")  
  
 Quand le rapport est exporté vers Excel, la distance de trajet domicile-travail s’affiche dans une seule cellule de la colonne Commute Distance (distance de trajet domicile-travail). Suivant l'alignement du texte dans le rapport (en haut, au milieu ou en bas), la valeur est dans la première cellule, dans la cellule du milieu ou dans la dernière cellule. Les autres cellules sont vides. La colonne Name qui contient le nom des clients ne compte aucune cellule vide. L'image suivante présente le rapport après son exportation vers Excel. Les bordures de cellules rouges ont été ajoutées pour les mettre en évidence. Les zones grises sont les cellules vides. (Ni les lignes rouges ni les zones grises ne figurent dans le rapport exporté.)  
  
 ![Rapport exporté vers Excel, avec des lignes](../../reporting-services/report-builder/media/ssrb-exportedexcellines.png "Rapport exporté vers Excel, avec des lignes")  
  
 Cela signifie que les rapports avec des groupes de lignes ou des groupes de colonnes nécessitent d'être modifiés après leur exportation vers Excel et avant que vous puissiez afficher les données exportées dans le tableau croisé dynamique. Vous devez ajouter la valeur de groupe aux cellules dans lesquelles elle est manquante pour transformer la feuille de calcul en une table plate avec des valeurs dans toutes les cellules. L’image suivante correspond à la feuille de calcul mise à jour.  
  
 ![Rapport exporté vers Excel, aplati](../../reporting-services/report-builder/media/ssrb-excelexportnomatrix.png "Rapport exporté vers Excel, aplati")  
  
 Donc si vous créez un rapport à la seule fin de l’exporter vers Excel en vue d’une analyse plus approfondie des données du rapport, évitez d’effectuer un regroupement sur des lignes ou des colonnes de votre rapport.  
  
## <a name="excel-renderer"></a>Convertisseur Excel  
  
### <a name="current-xlsx-excel-file-renderer"></a>Convertisseur des fichiers Excel actuels (.xlsx)  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], le convertisseur Excel par défaut est la version compatible avec les fichiers [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] actuels (.xlsx). Il s’agit de l’option **Excel** dans les menus **Exportation** du portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et de la liste SharePoint.  
  
 Quand vous utilisez le convertisseur Excel par défaut, et non le convertisseur Excel 2003 (.xls), vous pouvez installer le module de compatibilité Microsoft Office pour Word, Excel et PowerPoint pour permettre aux versions antérieures d’Excel d’ouvrir les fichiers exportés.  
  
### <a name="excel-2003-xls-renderer"></a>Convertisseur Excel 2003 (.xls)  
  
> [!IMPORTANT]  
>  L'extension de rendu [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 est déconseillée. Pour plus d’informations, consultez [Fonctions déconseillées de SQL Server Reporting Services dans SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 La version antérieure du convertisseur Excel, compatible avec Excel 2003, s'appelle désormais Excel 2003 et est répertoriée dans les menus sous ce nom. Le type de contenu des fichiers générés par ce convertisseur est **application/vnd.ms-excel** et l’extension des noms de fichiers est .xls.  
  
 Par défaut, l'option de menu **Excel 2003** n'est pas visible. Un administrateur peut la rendre visible dans certains cas en mettant à jour le fichier de configuration RSReportServer. Pour exporter des rapports à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] à l'aide du convertisseur Excel 2003, vous devez mettre à jour le fichier de configuration RSReportDesigner.  
  
 L'extension relative à l'option de menu **Excel 2003** n'est jamais visible dans les scénarios suivants :  
  
-   Utilisation du Générateur de rapports en mode déconnecté et affichage de l'aperçu d'un rapport dans le Générateur de rapports. Dans la mesure où le fichier de configuration RSReportServer réside sur le serveur de rapports, les outils ou produits à partir desquels vous exportez les rapports doivent être connectés à un serveur de rapports pour permettre la lecture du fichier de configuration.  
  
-   Utilisation du composant WebPart Visionneuse de rapports en mode local alors que la batterie de serveurs SharePoint n'est pas intégrée à un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Rapports en mode local et rapports en mode connecté dans la Visionneuse de rapports &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 Si le convertisseur relatif à l'option de menu **Excel 2003** est configuré pour être visible, les options Excel et Excel 2003 sont disponibles dans les scénarios suivants :  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode natif du portail web.  
  
-   Utilisation du site SharePoint lorsque Reporting Services est installé en mode intégré SharePoint.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et affichage de l'aperçu des rapports.  
  
-   Connexion du Générateur de rapports à un serveur de rapports.  
  
-   Utilisation du composant WebPart Visionneuse de rapports en mode distant.  
  
 Le code XML suivant affiche les éléments des deux extensions de rendu Excel dans les fichiers de configuration RSReportServer et RSReportDesigner :  
  
 `<Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>`  
  
 `<Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>`  
  
 L’extension EXCELOPENXML définit le convertisseur Excel pour les fichiers Excel actuels (.xlsx). L'extension EXCEL définit la version d'Excel 2003. `Visible = "false"` indique que le convertisseur Excel 2003 est masqué. Pour plus d’informations, consultez [Fichier de configuration RsReportserver.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) et [Fichier de configuration RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).  
  
### <a name="differences-between-the-current-xlsx-excel-and-excel-2003-renderers"></a>Différences entre les convertisseurs de fichiers Excel actuels (.xlsx) et Excel 2003  
 Les rapports restitués avec les convertisseurs de fichiers Excel actuels (.xlsx) ou Excel 2003 sont en général identiques, et rares sont les cas où vous remarquerez des différences entre les deux formats. Le tableau suivant compare les convertisseurs Excel et Excel 2003.  
  
|Propriété|Excel 2003|Version actuelle d’Excel|  
|--------------|----------------|-------------------|  
|Nombre maximal de colonnes par feuille de travail|256|16 384|  
|Nombre maximal de lignes par feuille de travail|65 536|1,048,576|  
|Nombre de couleurs autorisé dans une feuille de travail|56 (palette)<br /><br /> Si plus de 56 couleurs sont utilisées dans le rapport, l'extension de rendu fait correspondre la couleur requise à l'une des 56 couleurs déjà disponibles dans la palette personnalisée.|Approximativement 16 millions (couleurs 24 bits)|  
|Fichiers compressés ZIP|None|Compression ZIP|  
|Famille de polices par défaut|Arial|Calibri|  
|Taille de police par défaut|10pt|11pt|  
|Hauteur de ligne par défaut|12,75 pt|15 pt|  
  
 Dans la mesure où le rapport définit la hauteur de ligne de manière explicite, la hauteur de ligne par défaut affecte uniquement les lignes dimensionnées automatiquement lors de l'exportation vers Excel.  
  
##  <a name="report-items-in-excel"></a><a name="ReportItemsExcel"></a> Éléments de rapport dans Excel  
 Les rectangles, les sous-rapports, le corps du rapport et les régions de données sont rendus sous la forme d'une plage de cellules Excel. Les zones de texte, les images, ainsi que les graphiques, les barres de données, les graphiques sparkline, les cartes, les jauges et les indicateurs doivent être rendus dans une cellule Excel, qui peut être fusionnée en fonction de la mise en page du reste du rapport.  
  
 Les images, les graphiques, les graphiques sparkline, les barres de données, les cartes, les jauges, les indicateurs et les lignes sont positionnés dans une cellule Excel, mais se trouvent au-dessus du quadrillage de la cellule. Les lignes sont rendues sous forme de bordures de cellules.  
  
 Les graphiques, les graphiques sparkline, les barres de données, les cartes, les jauges et les indicateurs sont exportés sous forme d'images. Les données qu'ils représentent, par exemple les étiquettes de membres et de valeurs d'un graphique, ne sont pas exportées et ne sont pas disponibles dans le classeur Excel, à moins qu'elles ne soient incluses dans une colonne ou une ligne d'une région de données au sein d'un rapport.  
  
 Si vous souhaitez utiliser des données de graphiques, de graphiques sparkline, de barres de données, de cartes, de jauges et d'indicateurs, exportez le rapport vers un fichier .csv ou générez des flux de données conformes à Atom à partir du rapport. Pour plus d’informations, consultez [Exportation vers un fichier CSV &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md) et [Génération de flux de données à partir de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
## <a name="page-sizing"></a>Dimensionnement de page  
 L'extension de rendu Excel utilise les paramètres de hauteur et de largeur de page pour déterminer le paramètre du papier à définir dans la feuille de calcul Excel. Excel essaie de faire correspondre le paramétrage des propriétés PageHeight et PageWidth à l’un des formats de papier les plus courants.  
  
 Si aucune correspondance n'est trouvée, Excel utilise la taille de la page par défaut pour l'imprimante. Si la largeur de la page est inférieure à la hauteur du papier, l'orientation est définie sur Portrait, dans le cas contraire, l'orientation Paysage est utilisée.  
  
##  <a name="worksheet-tab-names"></a><a name="WorksheetTabNames"></a> Noms des onglets de feuille de calcul  
 Lorsque vous exportez un rapport vers Excel, les pages de rapport créées par les sauts de page sont exportées dans différentes feuilles de calcul. Si vous avez fourni un nom de page initial pour le rapport, chaque feuille de calcul du classeur Excel porte ce nom par défaut. Le nom s'affiche sous l'onglet de feuille de calcul. Toutefois, puisque chaque feuille de calcul d'un classeur doit porter un nom unique, un nombre entier démarrant à 1 et incrémenté de 1 est ajouté au nom de page initial pour chaque feuille de calcul supplémentaire. Par exemple, si le nom de page initial est **État des ventes par exercice**, la deuxième feuille de calcul sera nommée **État des ventes par exercice1**, la troisième **État des ventes par exercice2**, etc.  
  
 Si toutes les pages de rapport créées par les sauts de page fournissent de nouveaux noms de page, chaque feuille de calcul portera le nom de la page associée. Toutefois, ces noms de page peuvent ne pas être uniques. Si les noms de page ne sont pas uniques, les feuilles de calcul sont nommées de la même manière que les pages initiales. Par exemple, si le nom de page de deux groupes est **Ventes NO**, un onglet de feuille de calcul aura le nom **Ventes NO**, et l'autre **Ventes NO1**.  
  
 Si le rapport ne fournit ni un nom de page initial, ni un nom de page associé aux sauts de page, les onglets de feuille de calcul porteront les noms par défaut, à savoir **Feuil1**, **Feuil2**, etc.  
  
 Reporting Services fournit des propriétés à définir dans les rapports, les régions de données, les groupes et les rectangles afin de vous aider à créer des rapports qui peuvent être exportés vers Excel comme vous le souhaitez. Pour plus d’informations, voir [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> Propriétés du document  
 Le convertisseur Excel écrit les métadonnées suivantes dans le fichier Excel.  
  
|Propriétés des éléments de rapport|Description|  
|-------------------------------|-----------------|  
|Date de création|Date et heure d'exécution du rapport sous la forme d'une valeur de date/d'heure ISO.|  
|Auteur|Report.Author|  
|Description|Report.Description|  
|LastSaved|Date et heure d'exécution du rapport sous la forme d'une valeur de date/d'heure ISO.|  
  
##  <a name="page-headers-and-footers"></a><a name="PageHeadersFooters"></a> En-têtes et pieds de page  
 En fonction du paramètre des informations relatives au périphérique SimplePageHeaders, l'en-tête de page peut être rendu de deux façons : l'en-tête de page peut être rendu au haut du quadrillage de chaque feuille de calcul ou dans la section d'en-tête de la feuille de calcul Excel. Par défaut, l'en-tête est rendu sur le quadrillage de la feuille de calcul Excel.  
  
 Le pied de page est toujours rendu dans la section du pied de page de la feuille de calcul d'Excel, indépendamment de la valeur du paramètre SimplePageHeaders.  
  
 Les sections d'en-tête et de pied de page Excel prennent en charge un maximum de 256 caractères, y compris la balise. Si cette limite est dépassée, le convertisseur Excel supprime les caractères de balisage en commençant à la fin de la chaîne d'en-tête et/ou de pied de page pour réduire le nombre total de caractères. Si tous les caractères de balisage sont supprimés et que la longueur dépasse encore le maximum, la chaîne est tronquée à partir de la droite.  
  
### <a name="simplepageheader-settings"></a>Paramètres SimplePageHeader  
 Par défaut, le paramètre SimplePageHeaders d'informations relatives au paramètre est défini à **False**; par conséquent, les en-têtes de page sont rendus sous forme de lignes dans le rapport sur la surface de la feuille de calcul Excel. Les lignes de la feuille de calcul qui contiennent les en-têtes sont verrouillées. Vous pouvez figer ou libérer le volet dans Excel. Si l'option **Titres à imprimer** est sélectionnée, ces en-têtes sont automatiquement définis pour être imprimés sur chaque page de la feuille de calcul.  
  
 L'en-tête de page est répété au haut de chaque feuille de calcul du classeur sauf sur la feuille de couverture de l'explorateur de documents si l'option **Titres à imprimer** est sélectionnée sous l'onglet Mise en page dans Excel. Si l'option **Imprimer sur la première page** ou **Imprimer sur la dernière page** n'est pas sélectionnée dans les boîtes de dialogue Propriétés d'en-tête du rapport ou Propriétés de pied de page du rapport, l'en-tête ne sera pas ajouté respectivement à la première ou à la dernière page.  
  
 Les pieds de page sont rendus dans la section de pied de page Excel.  
  
 En raison des limitations Excel, les zones de texte sont le seul type d'élément de rapport qui peut être rendu dans la section en-tête/pied de page Excel.  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> Interactivité  
 Certains éléments interactifs sont pris en charge dans Excel. Vous trouverez ci-dessous une description de comportements spécifiques.  
  
### <a name="show-and-hide"></a>Afficher et masquer  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] . Les groupes, lignes et colonnes qui contiennent des éléments de rapport qui peuvent être affichés/masqués sont rendus sous forme de plans Excel. Excel crée des plans qui affichent ou masquent des lignes et des colonnes entières, ce qui peut provoquer le masquage d'éléments de rapport qui ne doivent pas l'être. De plus, les symboles de plan Excel peuvent encombrer des plans qui se chevauchent. Pour résoudre ces problèmes, les règles suivantes relatives au plan sont appliquées lors de l'utilisation de l'extension de rendu Excel :  
  
-   L'élément de rapport dans l'angle supérieur gauche qui peut être affiché/masqué peut continuer à être affiché/masqué dans Excel. Les éléments de rapport qui peuvent être affichés/masqués et qui partagent un espace vertical ou horizontal avec l'élément de rapport qui peut être affiché/masqué dans l'angle supérieur gauche ne peuvent pas être affichés/masqués dans Excel.  
  
-   Pour déterminer si une région de données sera réductible par lignes ou colonnes, la position de l'élément de rapport qui contrôle le basculement et la position de l'élément de rapport basculé sont déterminés. Si l'élément qui contrôle le basculement s'affiche avant l'élément à afficher/masquer, l'élément est réductible par lignes. Dans le cas contraire, l'élément est réductible par colonnes. Si l'élément qui contrôle le basculement s'affiche à côté et au-dessus de la zone à afficher/masquer, l'élément est rendu avec une ligne réductible par lignes.  
  
-   Pour déterminer où les sous-totaux sont placés dans le rapport rendu, l'extension de rendu examine la première instance d'un membre dynamique. Si un membre statique homologue s'affiche immédiatement au-dessus, le membre dynamique est considéré comme le sous-total. Les plans sont configurés pour indiquer qu'il s'agit de données de synthèse. Si un membre dynamique n'a pas de frère statique, la première instance de l'instance est le sous-total.  
  
-   En raison d'une limitation Excel, les plans ne peuvent être imbriqués que de 7 niveaux.  
  
### <a name="document-map"></a>Explorateur de documents  
 Si des étiquettes d'explorateur de documents existent dans le rapport, un explorateur de documents est rendu. L'explorateur de documents est rendu sous la forme d'une feuille de calcul de couverture Excel insérée à la place du premier onglet du classeur. La feuille de calcul s'appelle **Explorateur de documents**.  
  
 Le texte affiché dans l’explorateur de documents est déterminé par la propriété DocumentMapLabel de l’élément de rapport ou du groupe. Les étiquettes Explorateur de documents sont répertoriées dans l'ordre dans lequel elles apparaissent dans le rapport, en commençant à la première ligne, de la première colonne. Chaque cellule de l'étiquette de l'explorateur de documents est mise en retrait du nombre de niveaux du rapport. Chaque niveau de mise en retrait est représenté en plaçant l'étiquette dans une colonne suivante. Excel prend en charge jusqu'à 256 niveaux d'imbrication de plan.  
  
 Le plan de l'explorateur de documents est rendu sous la forme d'un plan réductible Excel. La structure du plan correspond à la structure imbriquée de l'explorateur de documents. L'état développé ou réduit du plan commence au deuxième niveau.  
  
 Le nœud racine de la carte est le nom de rapport, \<*reportname*>.rdl, qui n’est pas interactif. La police des liens de l'explorateur de documents est Arial, 10pt.  
  
### <a name="drillthrough-links"></a>Liens d'extraction  
 Les liens d'extraction qui s'affichent dans les zones de texte sont rendus sous forme de liens hypertexte Excel dans la cellule dans laquelle le texte est rendu. Les liens d'extraction des images et des graphiques sont rendus sous forme de liens hypertexte Excel dans l'image lors du rendu. Quand vous cliquez dessus, le lien d’extraction ouvre le navigateur par défaut du client et accède à la vue HTML de la cible.  
  
### <a name="hyperlinks"></a>Liens hypertexte  
 Les liens hypertexte qui s'affichent dans les zones de texte sont rendus sous forme de liens hypertexte Excel dans la cellule dans laquelle le texte est rendu. Les liens hypertexte des images et des graphiques sont rendus sous forme de liens hypertexte Excel dans l'image lors du rendu. Quand vous cliquez dessus, le lien hypertexte ouvre le navigateur par défaut du client et accède à l’URL cible.  
  
### <a name="interactive-sorting"></a>Tri interactif  
 Excel ne prend pas en charge le tri interactif.  
  
### <a name="bookmarks"></a>Signets  
 Les liens de signet qui s'affichent dans les zones de texte sont rendus sous forme de liens hypertexte Excel dans la cellule dans laquelle le texte est rendu. Les liens de signet pour les images et les graphiques sont rendus sous forme de liens hypertexte Excel dans l'image en cas de rendu. Lors d'un clic, le signet va à la cellule Excel dans laquelle l'élément de rapport contenant un signet est rendu.  
  
##  <a name="changing-reports-at-run-time"></a><a name="ConditionalFormat"></a> Modification des rapports au moment de l'exécution  
 Si un rapport doit être restitué dans plusieurs formats et qu'il n'est pas possible de créer une mise en page de rapport qui restitue le rapport comme vous le souhaitez dans tous les formats requis, envisagez peut-être d'utiliser la valeur de la fonction globale intégrée RenderFormat pour modifier l'apparence du rapport de manière conditionnelle au moment de l'exécution. De cette façon, vous pouvez masquer ou afficher des éléments de rapport en fonction du convertisseur utilisé afin d'obtenir un résultat optimal dans chaque format. Pour plus d’informations, consultez [Références à des champs Globals et Users prédéfinis &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Fonctionnalités interactives des différentes extensions de rendu de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Rendu des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  

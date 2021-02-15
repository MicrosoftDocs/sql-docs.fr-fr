---
title: Utilisation du contrôle RSClientPrint dans des applications personnalisées | Microsoft Docs
description: RSPrintClient permet une impression côté client des rapports affichés dans la Visionneuse HTML. Le serveur de rapports affiche le rapport dans l’extension de rendu d’image (EMF).
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- RSPrintClient control
- print controls [Reporting Services]
- custom printing [Reporting Services]
- client-side printing
ms.assetid: 8c0bdd18-8905-4e22-9774-a240fc81a8a7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c977799cb20d3735b4cc77d2608c0412ce51769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022272"
---
# <a name="using-the-rsclientprint-control-in-custom-applications"></a>Utilisation du contrôle RSClientPrint dans les applications personnalisées
  Le contrôle [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ActiveX **RSPrintClient** permet une impression côté client des rapports affichés dans la Visionneuse HTML. À partir de la boîte de dialogue **Imprimer** de ce contrôle, un utilisateur peut démarrer un travail d’impression, afficher l’aperçu d’un rapport, spécifier les pages à imprimer et modifier les marges. Lors d'une impression côté client, le serveur de rapports génère le rendu du rapport dans l'extension de rendu (EMF) d'image, puis utilise les fonctionnalités d'impression du système d'exploitation afin de créer le travail d'impression et de l'envoyer à une imprimante.  
  
 L'impression côté client permet de contrôler et d'améliorer la qualité d'impression d'un rapport HTML en laissant de côté les paramètres d'impression du navigateur sur l'ordinateur de l'utilisateur et en utilisant à la place le format de page, les marges, l'en-tête et le pied de page du rapport afin de créer la sortie imprimée. Le contrôle d'impression lit les valeurs de propriétés du rapport afin de définir la taille et les marges des pages.  
  
 Les développeurs qui souhaitent activer la fonctionnalité d’impression côté client dans des barres d’outils ou Visionneuses tierces peuvent accéder au contrôle ActiveX via l’objet COM **RSClientPrint**. Le contrôle peut être distribué librement. La liste suivante fournit des recommandations relatives à l'utilisation du contrôle :  
  
-   Utilisez le contrôle pour améliorer l'impression des rapports basés sur le Web. Vous pouvez spécifier l’objet dans n’importe quel langage de programmation compatible avec [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] ou dans un script. Le contrôle n'est pas destiné aux applications [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Forms.  
  
-   Copiez le fichier .cab des fichiers programmes [!INCLUDE[ssRSNoVersion](../../../includes/ssrsnoversion-md.md)] et ajoutez-le au code de base de votre application personnalisée.  
 
-   Utilisez une balise \<OBJECT> pour spécifier le contrôle.  
  
-   Spécifiez une URL relative ou complète pointant vers le fichier .cab dans l'attribut OBJECT CODEBASE.  
  
-   Spécifiez vos propres informations de version de l'application pour permettre au fichier .cab d'assurer le suivi de la version utilisée dans votre application.  
    
  
## <a name="rsprintclient-overview"></a>Présentation de RSPrintClient  
 Le contrôle affiche une boîte de dialogue d'impression personnalisée qui prend en charge les fonctionnalités communes aux autres boîtes de dialogue d'impression, notamment l'aperçu avant impression, les options de pages pour les choix spécifiques liés aux pages, aux plages, aux marges des pages et à l'orientation. Le contrôle est créé sous forme de package désigné en tant que fichier CAB. Le texte de la boîte de dialogue **Imprimer** est localisé dans toutes les langues prises en charge dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le contrôle ActiveX **RSPrintClient** utilise l’extension de rendu d’image (EMF) pour imprimer le rapport. Les informations de périphérique EMF suivantes sont utilisées : StartPage, EndPage, MarginBottom, MarginLeft, MarginTop, MarginRight, PageHeight et PageWidth. Les autres paramètres d'informations de périphérique pour le rendu d'image ne sont pas pris en charge.  
  
### <a name="language-support"></a>Prise en charge de la langue  
 Le contrôle d'impression fournit une interface utilisateur en plusieurs langues ; par ailleurs, il accepte les valeurs d'entrée calibrées selon les différents systèmes de mesure. La langue et le système de mesure utilisés sont déterminés par les propriétés **Culture** et **UICulture**. Les deux propriétés acceptent les valeurs LCID. Si vous spécifiez une valeur LCID pour une langue qui représente une variante d'une autre langue prise en charge, vous obtiendrez la langue la plus proche. Si vous spécifiez une valeur LCID qui n'est pas prise en charge et pour laquelle il n'existe aucune valeur LCID approchante, vous obtiendrez l'anglais (États-Unis).  
  
## <a name="using-rsclientprint-in-code"></a>Utilisation de RSClientPrint dans le code  
 L’objet **RSClientPrint** sert à accéder par programmation au contrôle ActiveX, ainsi qu’à ses méthodes et propriétés. Le contrôle fournit une boîte de dialogue modale pour l'aperçu avant impression.  
  
### <a name="specifying-default-values"></a>Spécification des valeurs par défaut  
 Vous pouvez initialiser la boîte de dialogue **Imprimer** avec les valeurs relatives aux marges et aux pages du rapport. Par défaut, la boîte de dialogue **Imprimer** est initialisée à l’aide des valeurs provenant de la définition de rapport. Vous pouvez utiliser les valeurs par défaut ou en spécifier d'autres en définissant les propriétés de l'objet.  
  
 Toutes les dimensions sont définies en millimètres. La conversion des mesures se produit au moment de l’exécution si **Culture** et **UICulture** ont pour valeurs des paramètres régionaux non basés sur des mesures métriques.  
  
 Pour comprendre quelles sont les valeurs utilisées pour les dimensions et les marges des pages, vous pouvez utiliser la méthode **GetProperties** afin de récupérer les valeurs par défaut :  
  
-   **PageHeight** et **PageWidth** spécifient la hauteur et la largeur de page par défaut. Lorsque le contrôle d'impression est lancé, ces valeurs de propriétés servent à sélectionner le format de papier le plus approprié à l'imprimante actuellement sélectionnée. Si **PageWidth** est supérieur à **PageHeight**, l’orientation est définie sur Paysage. Dans le cas contraire, elle est définie au format Portrait.  
  
-   **LeftMargin**, **RightMargin**, **TopMargin** et **BottomMargin** ont tous par défaut la valeur 12,2 millimètres.  
  
 Ces propriétés sont stockées dans la collection de propriétés **Item** sur le serveur de rapports. Les valeurs sont remplacées chaque fois qu'une définition de rapport est mise à jour.  
  
### <a name="rsclientprint-properties"></a>Propriétés de RSClientPrint  
  
|Propriété|Type|L/E|Default|Description|  
|--------------|----------|--------|-------------|-----------------|  
|MarginLeft|Double|L/E|paramètre du rapport|Obtient ou définit la marge de gauche. La valeur par défaut, si elle n'est pas définie par le développeur ou spécifiée dans le rapport, est de 12,2 millimètres.|  
|MarginRight|Double|L/E|paramètre du rapport|Obtient ou définit la marge de droite. La valeur par défaut, si elle n'est pas définie par le développeur ou spécifiée dans le rapport, est de 12,2 millimètres.|  
|MarginTop|Double|L/E|paramètre du rapport|Obtient ou définit la marge supérieure. La valeur par défaut, si elle n'est pas définie par le développeur ou spécifiée dans le rapport, est de 12,2 millimètres.|  
|MarginBottom|Double|L/E|paramètre du rapport|Obtient ou définit la marge inférieure. La valeur par défaut, si elle n'est pas définie par le développeur ou spécifiée dans le rapport, est de 12,2 millimètres.|  
|PageWidth|Double|L/E|paramètre du rapport|Obtient ou définit la largeur de page. La valeur par défaut, si elle n’est pas définie par le développeur ou spécifiée dans la définition de rapport, est de 215,9 millimètres.|  
|PageHeight|Double|L/E|paramètre du rapport|Obtient ou définit la hauteur de page. La valeur par défaut, si elle n'est pas définie par le développeur ou spécifiée dans la définition de rapport, est de 279,4 millimètres.|  
|Culture|Int32|L/E|Paramètres régionaux du navigateur|Spécifie l'identificateur de paramètres régionaux (LCID). Cette valeur détermine l'unité de mesure de l'entrée d'utilisateur. Par exemple, si un utilisateur tape **3**, la valeur est mesurée en millimètres quand la langue est le français, ou en pouces quand la langue est l’anglais des États-Unis. Les valeurs suivantes sont valides : 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|UICulture|String|L/E|Culture du client|Spécifie la localisation de la chaîne de la boîte de dialogue. Le texte de la boîte de dialogue d'impression est disponible dans les langues suivantes : chinois simplifié, chinois traditionnel, anglais, français, allemand, italien, japonais, coréen, et espagnol. Les valeurs suivantes sont valides : 1028, 1031, 1033, 1036, 1040, 1041, 1042, 2052, 3082.|  
|Authenticate|Boolean|L/E|False|Spécifie si le contrôle émet une commande GET destinée au serveur de rapports pour établir une connexion pour l'impression hors session.|  
  
### <a name="when-to-set-the-authenticate-property"></a>Quand définir la propriété Authenticate  
 Quand vous effectuez une impression dans une session de navigateur, vous n’avez pas besoin de définir la propriété **Authenticate**. Dans le contexte d'une session active, toutes les demandes adressées au serveur de rapports par le contrôle d'impression sont gérées via le navigateur. Le navigateur définit les variables de session nécessaires pour communiquer avec le serveur de rapports.  
  
 Si vous effectuez une impression hors session (par exemple, si vous envoyez un rapport directement à l’imprimante sans l’ouvrir préalablement), le contrôle d’impression doit émettre une requête HTTP **GET** pour configurer la session avec le serveur de rapports. Pour émettre la requête **GET**, vous définissez **Authenticate** sur **True**.  
  
 Vous ne devez émettre la requête **GET** que si vous utilisez la sécurité intégrée Windows ou l’authentification de base. Si vous utilisez l’authentification par formulaire, la propriété **Authenticate** est ignorée. Le code de votre application doit définir la session et authentifier l'utilisateur avec l'extension de sécurité personnalisée que vous fournissez. Si vous utilisez l'authentification par formulaires, veillez à définir la valeur d'expiration sur les cookie d'authentification de manière à protéger les sessions pendant une durée suffisante. En effet, si la valeur est trop faible, il est demandé aux utilisateurs de fournir des informations d'identification de connexion à chaque expiration du cookie.  
  
### <a name="clsids"></a>CLSID  
 Lorsque vous exécutez le rapport sur site, utilisez les valeurs CLSID suivantes.  
  
-   41861299-EAB2-4DCC-986C-802AE12AC499  
  
-   5554DCB0-700B-498D-9B58-4E40E5814405  
  
-   60677965-AB8B-464f-9B04-4BA871A2F17F  
  
 Lorsque vous exécutez le rapport dans Azure SQL Reporting, vous utilisez les valeurs CLSID suivantes.  
  
-   3DD32426-554D-48C0-A200-65D3BF880E38  
  
-   05662494-ACF9-446A-BE4C-7D3F7EA7F62F  
  
### <a name="rsprintclient-support-for-the-print-method"></a>Prise en charge de RSPrintClient pour la méthode d'impression  
 L’objet **RSClientPrint** prend en charge la méthode **Print** qui est utilisée pour lancer la boîte de dialogue Imprimer. La méthode **Print** dispose des arguments suivants.  
  
|Argument|E/S|Type|Description|  
|--------------|----------|----------|-----------------|  
|ServerPath|Dans|String|Spécifie le répertoire virtuel du serveur de rapports (par exemple, `https://adventure-works/reportserver`).|  
|ReportPathParameters|Dans|String|Spécifie le nom complet du rapport, ainsi que les paramètres, dans l'espace de noms de dossier du serveur de rapports. Les rapports sont extraits via une URL. Par exemple : "/AdventureWorks Sample Reports/Employee Sales Summary&EmpID=1234"|  
|ReportName|Dans|String|Nom court du rapport (dans l'exemple ci-dessus, le nom court est Employee Sales Summary). Il s'affiche dans la boîte de dialogue Imprimer ainsi que dans la file d'attente à l'impression.|  
  
### <a name="example"></a>Exemple  
 L’exemple HTML suivant montre comment spécifier le fichier .cab, la méthode **Print** et les propriétés dans JavaScript :  
  
 `<BODY onload="Print()">`  
  
 `<OBJECT ID="RSClientPrint" CLASSID="CLSID: 5554DCB0-700B-498D-9B58-4E40E5814405D3" CODEBASE="<URL to the .CAB file>#Version=<your application version information>" VIEWASTEXT></OBJECT>`  
  
 `<script language="javascript">`  
  
 `function Print()`  
  
 `{`  
  
 `RSClientPrint.MarginLeft = 12.7;`  
  
 `RSClientPrint.MarginTop = 12.7;`  
  
 `RSClientPrint.MarginRight = 12.7;`  
  
 `RSClientPrint.MarginBottom = 12.7;`  
  
 `RSClientPrint.Culture = 1033;`  
  
 `RSClientPrint.UICulture = 9;`  
  
 `RSClientPrint.Print('https://localhost/rtm', '%2fEmployee_Sales_Summary&ReportMonth=6&ReportYear=2004&EmpID=20', 'Employee_Sales_Summary')`  
  
 `}`  
  
 `</script>`  
  
 `</BODY>`  
  
## <a name="see-also"></a>Voir aussi  
 [Imprimer des rapports à partir d’un navigateur à l’aide du contrôle d’impression &#40;Générateur de rapports et SSRS&#41;](../../../reporting-services/report-builder/print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Paramètres d’informations de périphérique pour l’image](../../../reporting-services/image-device-information-settings.md)  
  
  

---
title: Utiliser l’accès par URL dans des applications web
description: L’accès par URL dans Reporting Services permet d’accéder aux rapports par le biais d’un réseau, ce qui permet d’intégrer l’affichage et la navigation des rapports dans une application web personnalisée.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- links [Reporting Services], URL access
- URL access [Reporting Services], Web applications
- POST requests
- direct addressing [Reporting Services]
- Web applications [Reporting Services]
- hyperlinks [Reporting Services]
ms.assetid: 39e7918c-ad2d-4ca6-b099-2dd4dbdb83dc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f2df0e4bd2a78b52fcf08587ffed2eff219282b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074880"
---
# <a name="integrating-reporting-services-using-url-access---web-application"></a>Intégration de Reporting Services à l’aide de l’accès URL - Application web
  L'accès URL dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est conçu spécifiquement pour permettre l'accès à des rapports individuels sur un réseau. Ce type d'accès convient pour intégrer l'affichage des rapports et la navigation au sein de ces derniers dans une application Web personnalisée. Pour utiliser l'accès URL dans des applications Web, vous pouvez :  
  
-   définir une URL vers un serveur de rapports spécifique à partir d'un site ou d'un portail Web ;  
  
-   utiliser une méthode POST de formulaire et passer des paramètres de chaîne de requête à une URL du serveur de rapports à l'aide de champs de formulaire.  
  
## <a name="url-access-through-direct-addressing"></a>Accès URL via l'adressage direct  
 Pour accéder à un serveur de rapports ou à un élément de la base de données du serveur de rapports à l’aide d’une URL, entrez simplement l’adresse URL dans un navigateur ou une application web. Vous pouvez également fournir des paramètres à l'URL qui peuvent affecter l'apparence du rapport ou de la ressource en cours d'accès. Une URL peut cibler un serveur de rapports par le biais de la barre d’adresses d’un navigateur web, ou elle peut être la source d’un **IFrame** qui fait partie d’une application ou d’un portail web plus important. Vous pouvez ajouter des liens hypertexte à des rapports sur différentes pages Web de votre portail, et cibler un frame spécifique pour le rapport ou ouvrir une nouvelle fenêtre de navigateur dans le processus.  
  
 Dans l'exemple suivant, le lien hypertexte cible un frame nommé « main » qui peut être différent de celui qui inclut le lien hypertexte. Le lien hypertexte peut faire partie d'un portail Web.  
  
```  
<a href="https://server/reportserver?/SampleReports/Territory Sales   
Drilldown&rs:Command=Render&rc:LinkTarget=main" target="main" >  
   Click here for the Territory Sales Drilldown sample report  
</a>  
```  
  
 Dans l’exemple précédent, le paramètre d’informations de périphérique **LinkTarget** est passé avec une valeur « main » dans la chaîne de requête de l’URL. De cette manière, tous les liens hypertexte d'extraction dans le rapport ciblent également le frame nommé « main ».  
  
 Pour plus d’informations sur les paramètres d’informations de périphérique, consultez [Transmission de paramètres d’informations de périphérique aux extensions de rendu](../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md).  
  
 Notez que de nombreux serveurs et navigateurs limitent le nombre de caractères autorisés dans une URL. Dans certains cas, une limite de 256 caractères est imposée. Pour contourner cette limitation, vous pouvez utiliser des requêtes POST à l'aide de l'envoi de formulaire.  
  
> [!NOTE]  
>  Internet Explorer impose une longueur maximale de 2 083 caractères dans les URL. Cette limite s'applique aux URL de requêtes POST et GET. Toutefois, POST n'est pas limité par la taille de l'URL pour l'envoi de paires nom/valeur dans le cadre d'un formulaire, celles-ci étant transférées dans l'en-tête et non dans l'URL.  
  
## <a name="url-access-through-a-form-post-method"></a>Accès URL via une méthode POST de formulaire  
 Lorsqu'un utilisateur demande des données à partir d'un serveur de rapports à l'aide de l'accès URL, la requête HTTP utilise la méthode GET. Ceci équivaut à un envoi de formulaire où METHOD="GET". Les demandes d'URL ou les envois de formulaire qui utilisent METHOD="GET" sont limités par le nombre maximal de caractères qu'un serveur ou navigateur Web peut traiter.  
  
 Avec les requêtes POST (METHOD="POST" et champs d'entrée), les paires nom/valeur sont transférées dans l'en-tête et non dans l'URL. Par conséquent, les paires nom/valeur de la chaîne de requête ne font pas partie de l'URL, ce qui vous permet de fournir des listes de paramètres plus longues et plus complexes.  
  
 À l'aide de l'accès direct, un utilisateur peut afficher l'URL pour le serveur de rapports et éventuellement modifier la chaîne de requête ou noter les paramètres particuliers de la demande d'URL et du serveur de rapports pour une utilisation ultérieure.  
  
 L'exemple de code HTML suivant montre l'utilisation d'un formulaire que vous pouvez utiliser pour cibler un serveur de rapports avec une URL spécifique et passer des paramètres de chaîne de requête dans le cadre des champs d'entrée du formulaire.  
  
```  
<FORM id="frmRender" action="https://server/reportserver?/SampleReports/  
   Territory Sales Drilldown" method="post" target="_self">  
   <INPUT type="hidden" name="rs:Command" value="Render">   
   <INPUT type="hidden" name="rc:LinkTarget" value="main">  
   <INPUT type="hidden" name="rs:Format" value="HTML4.0">  
   <INPUT type="submit" value="Button">  
</FORM>  
```  
  
 Dans l'exemple précédent, si un utilisateur clique sur le bouton du formulaire, le serveur de rapports retourne un rapport rendu en HTML ciblant le frame actuel. Une chaîne d'accès URL comparable peut se présenter comme suit :  
  
```  
https://server/reportserver?/SampleReports/Territory Sales   
Drilldown&rs:Command=Render&rc:LinkTarget=main&rs:Format=HTML4.0  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Intégration de Reporting Services dans des applications](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Intégration de Reporting Services à l’aide de l’accès URL](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)   
 [Utilisation de l’accès URL dans une application Windows](../../reporting-services/application-integration/integrating-reporting-services-using-url-access-windows-application.md)   
 [Accès URL &#40;SSRS&#41;](../../reporting-services/url-access-ssrs.md)  
  
  

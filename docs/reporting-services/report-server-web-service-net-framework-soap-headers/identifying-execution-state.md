---
title: Identification de l’état d’exécution | Microsoft Docs
description: Découvrez comment utiliser Reporting Services pour identifier l’état d’exécution dans le but d’interagir avec le rapport de plusieurs façons.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- session states [Reporting Services]
- lifetimes [Reporting Services]
- sessions [Reporting Services]
- SessionHeader SOAP header
ms.assetid: d8143a4b-08a1-4c38-9d00-8e50818ee380
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fbd189a85121d2ef18c69a1d7886f6b3c3294978
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045058"
---
# <a name="identifying-execution-state"></a>Identification de l'état d'exécution
  Le protocole HTTP (Hypertext Transfer Protocol) est un protocole sans connexion et sans état, ce qui signifie qu'il n'indique pas automatiquement si des requêtes différentes proviennent du même client ou si une seule instance de navigateur continue d'afficher activement une page ou un site. Les sessions créent une connexion logique pour maintenir l'état entre le serveur et le client sur HTTP. Les informations spécifiques à l'utilisateur relatives à une session particulière sont appelées l'état de session.  
  
 La gestion des sessions implique de mettre en corrélation une requête HTTP avec d'autres requêtes précédentes générées à partir de la même session. Sans gestion des sessions, ces requêtes apparaissent non liées pour le service Web Report Server en raison de la nature sans connexion et sans état du protocole HTTP.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'expose pas un concept holistique de l'état de session tel que celui qui est exposé par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Toutefois, lors de l'exécution de rapports, le serveur de rapports maintient l'état entre les appels de méthode sous la forme d'une **exécution**. Une exécution permet à l'utilisateur d'interagir de plusieurs façons avec le rapport, notamment en chargeant le rapport à partir du serveur de rapports, en définissant des informations d'identification et des paramètres pour le rapport et en effectuant son rendu.  
  
 Pendant qu'ils communiquent avec un serveur de rapports, les clients utilisent l'exécution pour gérer l'affichage des rapports et la navigation des utilisateurs vers d'autres pages d'un rapport, ainsi que pour afficher ou masquer certaines sections d'un rapport. Une exécution unique existe pour chaque rapport que l'application cliente exécute.  
  
 En général, la durée de vie d'une exécution commence lorsqu'un utilisateur accède à un navigateur ou à une application cliente et qu'il sélectionne un rapport à afficher. L'exécution est ignorée à l'issue d'un court délai d'attente une fois que la dernière requête a été reçue (le délai d'attente par défaut s'élève à 20 minutes).  
  
 Du point de vue d'un service Web, la durée de vie commence lorsque les méthodes <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A>, <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> ou <xref:ReportExecution2005.ReportExecutionService.Render%2A> du service Web Report Server sont appelées. L'application peut utiliser d'autres méthodes pour manipuler l'exécution active (par exemple, définir des paramètres et des sources de données). L'exécution est ignorée à l'issue d'un court délai d'attente une fois que la dernière requête a été reçue (le délai d'attente par défaut s'élève à 20 minutes).  
  
 Une application conserve une trace de plusieurs exécutions actives entre des appels des méthodes <xref:ReportExecution2005.ReportExecutionService.Render%2A> et <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A> du service Web en enregistrant la propriété <xref:ReportExecution2005.ExecutionHeader.ExecutionID%2A>, laquelle est retournée dans l'en-tête SOAP des méthodes <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> et <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>.  
  
 Le diagramme suivant illustre le chemin d'accès de traitement et de rendu des rapports.  
  
 ![Chemin d'accès de traitement/de rendu de rapport](../../reporting-services/report-server-web-service-net-framework-soap-headers/media/rs-render-process-diagram.gif "Chemin d'accès de traitement/de rendu de rapport")  
  
 Pour prendre en charge les fonctions décrites ci-dessus, la méthode de rendu SOAP a été divisée en plusieurs méthodes qui comprennent des phases d'initialisation de l'exécution, de traitement et de rendu.  
  
 Pour effectuer le rendu d'un rapport par programme, vous devez :  
  
-   charger le rapport ou la définition de rapport à l'aide des méthodes <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> ou <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> ;  
  
-   vérifier si le rapport a besoin d'informations d'identification ou de paramètres en vérifiant les valeurs des propriétés <xref:ReportExecution2005.ExecutionInfo.CredentialsRequired%2A> et <xref:ReportExecution2005.ExecutionInfo.ParametersRequired%2A> de l'objet <xref:ReportExecution2005.ExecutionInfo> retourné par les méthodes <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> ou <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> ;  
  
-   définir, si nécessaire, les informations d'identification et/ou les paramètres à l'aide des méthodes <xref:ReportExecution2005.ReportExecutionService.SetExecutionCredentials%2A> et <xref:ReportExecution2005.ReportExecutionService.SetExecutionParameters%2A> ;  
  
-   appeler la méthode <xref:ReportExecution2005.ReportExecutionService.Render%2A> pour effectuer le rendu du rapport.  
  
 Lorsqu'un rapport est en session, le rapport sous-jacent stocké dans la base de données du serveur de rapports peut changer. Par exemple, la définition de rapport peut changer, le rapport peut être supprimé ou déplacé et les autorisations des utilisateurs peuvent changer. Si le rapport est dans une session active, il n'est pas affecté par les modifications apportées au rapport sous-jacent (autrement dit, le rapport stocké dans la base de données du serveur de rapports).  
  
 Vous pouvez également gérer une session de rapport à l'aide de commandes d'accès URL.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [Informations techniques de référence &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Utilisation d’en-têtes SOAP Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  

---
title: Points de terminaison du service web Report Server | Microsoft Docs
description: Le service Web Report Server fournit trois points de terminaison pour la gestion d’un serveur de rapports, ainsi que pour l’exécution de rapports et la navigation dans ces derniers.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- management endpoints [Reporting Services]
- Web service [Reporting Services], endpoints
- endpoints [Reporting Services]
- execution endpoints [Reporting Services]
- Report Server Web service, endpoints
ms.assetid: f3f5d85f-9359-4508-bc5a-7f78a3cf7421
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1d9a0bfe8aa4a469899a558c1cbe1d3b38b337b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063744"
---
# <a name="report-server-web-service-endpoints"></a>Points de terminaison du service Web Report Server
  Le service Web Report Server fournit plusieurs points de terminaison pour la gestion d’un serveur de rapports aussi bien que l'exécution de rapports et la navigation dans ces derniers.  
  
## <a name="the-management-endpoints"></a>Points de terminaison de gestion  
 Trois points de terminaison sont disponibles pour la gestion des objets sur un serveur de rapports, <xref:ReportService2005>, <xref:ReportService2006> et <xref:ReportService2010>. Le point de terminaison <xref:ReportService2005> permet de gérer des objets sur un serveur de rapports qui est configuré pour le mode natif. Le point de terminaison <xref:ReportService2006> permet de gérer des objets sur un serveur de rapports qui est configuré pour le mode intégré SharePoint. Le point de terminaison <xref:ReportService2010> fusionne les fonctionnalités de <xref:ReportService2005> et <xref:ReportService2006> et peut gérer des objets sur un serveur de rapports configuré pour le mode natif ou intégré SharePoint.  
  
> [!IMPORTANT]  
>  Quand un serveur de rapports est configuré pour le mode intégré SharePoint, les API <xref:ReportService2005> retournent une erreur **rsOperationNotSupportedSharePointMode**. Si le serveur de rapports est configuré pour le mode natif, les API <xref:ReportService2006> retournent une erreur **rsOperationNotSupportedNativeMode**. De la même façon, lorsque les API spécifiques au mode dans <xref:ReportService2010> sont utilisées sur des modes involontaires, les API retourneront les erreurs correspondantes.  
  
> [!NOTE]  
>  Les points de terminaison <xref:ReportService2005> et <xref:ReportService2006> sont déconseillés dans [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. Le point de terminaison <xref:ReportService2010> inclut les fonctionnalités des deux points de terminaison, ainsi que des fonctionnalités de gestion supplémentaires.  
  
 Si le serveur de rapports est configuré pour le mode natif ou intégré SharePoint, le langage WSDL pour le point de terminaison de gestion est accessible à l'aide de l'une des adresses URL suivantes :  
  
```  
https://<Server Name>/ReportServer/ReportService2010.asmx?wsdl  
```  
  
 Pour plus d’informations, consultez [Accès à l’API SOAP](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="the-execution-endpoint"></a>Point de terminaison d'exécution  
 Le point de terminaison <xref:ReportExecution2005> facilite aux développeurs la personnalisation du traitement des rapports et le rendu à partir d'un serveur de rapports dans les modes intégrés natif et SharePoint. Le point de terminaison inclut des classes et des méthodes qui existaient dans des versions antérieures du service Web Report Server. En outre, nombre de nouvelles classes et méthodes, exposées par le biais du point de terminaison d'exécution, ont été ajoutées au service Web Report Server.  
  
 Le langage WSDL pour le point de terminaison de gestion est accessible à l'aide de l'adresse URL suivante :  
  
```  
https://<Server Name>/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Si le serveur de rapports est configuré pour le mode intégré SharePoint, le langage WSDL est accessible à l'aide de l'URL suivante :  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx?wsdl  
```  
  
 Pour plus d’informations, consultez [Accès à l’API SOAP](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md).  
  
## <a name="sharepoint-proxy-endpoints"></a>Points de terminaison de proxy SharePoint  
 Lorsqu'un serveur de rapports est configuré pour le mode intégré SharePoint et que le complément [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] est installé, un jeu de points de terminaison de proxy est installé sur le serveur SharePoint. Les points de terminaison de proxy constituent l'API primaire pour le développement de solutions de rapport lorsqu'un serveur de rapports est configuré pour le mode intégré SharePoint. Lors du développement avec les points de terminaison de proxy, le complément [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] gère l'échange des informations d'identification entre le serveur SharePoint et le serveur de rapports en mode d'authentification Compte approuvé. Lors du développement par rapport aux points de terminaison du serveur de rapports, l'application appelante doit gérer l'échange des informations d'identification en mode d'authentification Compte approuvé. Le tableau suivant répertorie les points de terminaison installés avec le complément [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Point de terminaison proxy|Description|  
|--------------------|-----------------|  
|<xref:ReportService2006>|Fournit les API permettant de gérer un serveur de rapports configuré pour le mode intégré SharePoint.<br /><br /> Remarque : Ce point de terminaison est déprécié dans [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)].|  
|<xref:ReportService2010>|Fournit les API permettant de gérer un serveur de rapports configuré pour le mode natif ou intégré SharePoint.|  
|<xref:ReportExecution2005>|Fournit les API pour l'exécution de rapports et la navigation dans ces derniers.|  
|<xref:ReportServiceAuthentication>|Fournit les API pour l'authentification des utilisateurs avec un serveur de rapports lorsque l'application Web SharePoint est configurée pour l'authentification par formulaires.|  
  
 Voici des exemples d'adresses URL pour le référencement des points de terminaison de proxy sur un site SharePoint.  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportService2010.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportExecution2005.asmx  
```  
  
```  
https://<Server Name>/<Site Name>/_vti_bin/ReportServer/ReportServiceAuthentication.asmx  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Génération d’applications à l’aide du service web et de .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  

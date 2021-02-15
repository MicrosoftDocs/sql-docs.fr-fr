---
title: Accès à l’API SOAP | Microsoft Docs
description: Le service Web Report Server utilise SOAP sur HTTP et agit comme une interface de communication entre les clients et le serveur de rapports. Utilisez WSDL pour appeler le service.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- XML Web service [Reporting Services], WSDL
- Web service [Reporting Services], SOAP
- calling Web service
- SOAP [Reporting Services], accessing
- Report Server Web service, SOAP
- Web service [Reporting Services], WSDL
- WSDL [Reporting Services]
- XML Web service [Reporting Services], SOAP
- Report Server Web service, WSDL
- referencing WSDL
ms.assetid: 63bb870a-4dbf-46bd-8921-78f8ebe5fd75
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5fcb5dd895f6997ed72c91e9c66d9bd7be1c5aa4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018652"
---
# <a name="accessing-the-soap-api"></a>Accès à l'API  SOAP
  Le service Web Report Server utilise le protocole SOAP (Simple Object Access Protocol) sur HTTP et joue le rôle d'interface de communication entre les programmes clients et le serveur de rapports. Le service Web fournit deux points de terminaison ; un pour l'exécution des rapports et un autre pour la gestion des rapports. Par ailleurs, il se compose de méthodes et d'un jeu d'objets de type complexe que vous pouvez utiliser pour accéder aux fonctionnalités complètes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour appeler le service, vous devez référencer WSDL (Web Services Description Language) Reporting Services.  
  
## <a name="referencing-the-reporting-services-wsdl"></a>Référencement deWSDL Reporting Services  
 Pour parvenir à appeler un service Web, vous devez savoir comment accéder à ce service, quelles opérations sont prises en charge, quels paramètres sont attendus et ce que ce service retourne. WSDL fournit ces informations dans un document XML qui peut être lu ou traité par un ordinateur.  
  
 Les services Web Report Server sont exposés dans trois points de terminaison différents. Le nom du fichier WSDL est différent pour chaque point de terminaison. Le point de terminaison <xref:ReportService2010> contient des méthodes pour gérer des objets dans un serveur de rapports en mode natif ou intégré SharePoint. L'accès au fichier WSDL de ce point de terminaison est réalisé à travers `ReportService2010.asmx?wsdl.`.  
  
> [!NOTE]  
>  Les points de terminaison <xref:ReportService2005> et <xref:ReportService2006> sont déconseillés dans [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Le point de terminaison <xref:ReportService2010> inclut les fonctionnalités des deux points de terminaison, ainsi que des fonctionnalités de gestion supplémentaires.  
  
-   Le point de terminaison <xref:ReportExecution2005> permet aux développeurs de traiter les rapports par programme et d'en effectuer le rendu sur un serveur de rapports. L’accès au WSDL de ce point de terminaison s’effectue à l’aide de `ReportExecution2005.asmx?wsdl`.  
  
 WSDL peut être utilisé par les kits de développement qui prennent en charge SOAP et les services web, comme le SDK [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 L'exemple suivant montre le format de l'URL au fichier WSDL de gestion [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
```  
https://server/reportserver/ReportService2010.asmx?wsdl  
```  
  
 Le tableau suivant décrit chaque élément de l'URL.  
  
|Élément de l'URL|Description|  
|-----------------|-----------------|  
|*server*|Nom du serveur sur lequel le serveur de rapports est déployé.|  
|*reportserver*|Nom du dossier qui contient le service Web XML. Cet élément est configuré au moment de l'installation.|  
|*\<endpoint name>.asmx*|Nom du point de terminaison du service Web.|  
  
 Pour plus d'informations sur le format WSDL, consultez la spécification WSDL du W3C (World Wide Consortium) à l'adresse http://www.w3.org/TR/wsdl.  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service Web des serveurs de rapports](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  

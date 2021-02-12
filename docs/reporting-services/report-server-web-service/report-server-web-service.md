---
title: Services web Report Server | Microsoft Docs
description: Reporting Services fournit les fonctionnalités du serveur de rapports avec le service Web Report Server, un point de terminaison de service SOAP pour l’exécution et la gestion des rapports.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- SSIS, Web service
- Web service [Reporting Services]
- Reporting Services, extending
- SQL Server Reporting Services, Web service
- Reporting Services, Web service
- XML Web service [Reporting Services]
- Report Server Web service
ms.assetid: 16c21dec-6b46-4497-9a0c-1b0f2b6ab8fc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 55160717791f4d14a7f008725710f725069cd6d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022078"
---
# <a name="report-server-web-service"></a>service Web Report Server
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] permet d’accéder à toutes les fonctionnalités du serveur de rapports par le biais du service web Report Server. Le service Web Report Server est un service Web XML avec une API SOAP. Il utilise SOAP sur HTTP et agit comme une interface de communication entre les programmes clients et le serveur de rapports. Le service Web fournit deux points de terminaison (un pour l'exécution des rapports et l'autre pour la gestion des rapports) avec des méthodes qui exposent les fonctionnalités du serveur de rapports et qui vous permettent de créer des outils personnalisés pour n'importe quelle partie du cycle de vie du rapport.  
  
 Trois méthodes principales s’offrent à vous pour développer des applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] basées sur le service web. Vous pouvez :  
  
-   Développer des applications à l’aide du SDK de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] et du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Pour plus d’informations sur l’utilisation du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour générer des applications de service web, consultez [Génération d’applications à l’aide du service web et du .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md).  
  
-   Développer des applications à l’aide de l’utilitaire **rs** (RS.exe), l’environnement de script [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Avec les scripts [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], vous pouvez exécuter n’importe laquelle des opérations de service web Report Server. Pour plus d’informations sur les scripts dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Écrire des scripts avec l’utilitaire rs.exe et le service web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
-   Développer des applications à l'aide de n'importe quel jeu SOAP d'outils de développement. Pour plus d’informations, consultez [Rôle de SOAP dans Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md).  
  
## <a name="programming-diagram"></a>Diagramme de programmation  
 ![Options de développement du service web Report Server](../../reporting-services/report-server-web-service/media/reportserviceswebserviceprog-01.gif "Options de développement du service web Report Server")  
Options de développement de service Web disponibles dans Reporting Services  
  
## <a name="in-this-section"></a>Dans cette section  
 [Méthodes du service web du serveur de rapports](../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)  
 Décrit les fonctionnalités et les méthodes de chaque service Web Report Server.  
  
 [Le rôle de SOAP dans Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md)  
 Fournit une vue d'ensemble du protocole SOAP et de la manière dont il est utilisé dans les services Web Report Server.  
  
 [Accès à l’API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)  
 Décrit le langage WSDL (Web Service Description Language) et fournit des URL pour accéder à un fichier WSDL Reporting Services.  
  
 [Génération d’applications à l’aide du service web et de .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
 Contient des informations sur le développement d'applications et de services Web qui appellent l'API SOAP Reporting Services.  
  
 [Écrire des scripts avec l'utilitaire rs.exe et le service Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)  
 Fournit une vue d'ensemble de l'environnement de script [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 [Informations techniques de référence &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)  
 Contient les documents de référence spécifiques aux méthodes des services Web Report Server et aux types complexes correspondants.  
  
## <a name="user-requirements-for-web-service-development"></a>Conditions requises liées à l'utilisateur pour le développement du service Web  
 Pour développer des applications à l'aide du service Web Report Server, vous devez posséder :  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 5.5 ou version ultérieure, installé sur un ordinateur doté d’une connexion Internet et ayant accès au serveur de rapports ;  
  
-   le SDK de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] installé sur un ordinateur si vous souhaitez développer et déployer des applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] ;  
  
-   des connaissances approfondies des fonctionnalités et des capacités de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ;  
  
-   de solides acquis concernant SOAP et les [!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)];  
  
-   de l’expérience en développement dans un langage compatible avec le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], tel que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], si vous envisagez d’utiliser le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] comme plateforme de développement.  
  
## <a name="see-also"></a>Voir aussi  
 [Service Web des serveurs de rapports](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  

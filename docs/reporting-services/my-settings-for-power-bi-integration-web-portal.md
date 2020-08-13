---
title: Mes paramètres pour l’intégration de Power BI (portail web) | Microsoft Docs
description: Découvrez la page Mes paramètres du portail web Reporting Services, et notamment comment celle-ci est utilisée par les utilisateurs pour gérer leur connexion à Power BI.
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
f1_keywords:
- pbi
- power bi
- power bi integration
ms.assetid: 85c2fac7-80bf-45b7-8654-764b5f5231f5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f25ab8f848c642de95f1ba62eaca15bbb8b7e7d9
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248575"
---
# <a name="my-settings-for-power-bi-integration-web-portal"></a>Mes paramètres pour l’intégration de Power BI (portail web)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Grâce à la page **Mes paramètres** du [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], les utilisateurs individuels peuvent gérer leur connexion à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]. Lorsque vous parcourez les étapes permettant d’épingler un élément de rapport, vous serez automatiquement invité à vous connecter.  Toutefois, vous pouvez utiliser la page **Mes paramètres** pour vous connecter manuellement ou vous déconnecter.  Si l’option de menu **Mes paramètres** n’est pas visible, cela signifie que le serveur de rapports n’est pas intégré à [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)].  Pour plus d’informations, consultez [Intégration du serveur de rapports Power BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md).  
  
![ssRS_WebPortal_MySettings](../reporting-services/media/ssrs-webportal-mysettings.png)  
  
## <a name="why-sign-in"></a>Pourquoi se connecter ?

 Lorsque vous vous connectez, vous établissez une relation entre votre compte d’utilisateur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et votre compte [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .  La connexion crée un jeton de sécurité, qui est valable pendant 90 jours. Si le jeton expire et que des éléments sont épinglés à Power BI, une notification apparaît.  
   
 ![ssRS_WebPortal_PowerBI_Notification](../reporting-services/media/ssrs-webportal-powerbi-notification.png)    
   
Pour que les vignettes des tableaux de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] soient actualisées, vous devez vous reconnecter via **Mes paramètres**.  
  
![ssRS_WebPortal_PowerBI_SignIn_Again](../reporting-services/media/ssrs-webportal-powerbi-signin-again.png)  
  
Une fois que vous êtes connecté, un jeton de sécurité est créé.  La mise à jour des vignettes de votre tableau de bord se lance sur leurs calendriers configurés précédemment.  

## <a name="next-steps"></a>Étapes suivantes

[Intégration de Power BI Report Server](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md)   
[Épingler des éléments Reporting Services aux tableaux de bord Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)   
[Tableaux de bord dans Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)  
[Portail Web](../reporting-services/web-portal-ssrs-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

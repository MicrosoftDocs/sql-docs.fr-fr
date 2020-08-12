---
title: Activer un serveur de rapports pour un accès Power BI Mobile | Microsoft Docs
description: Découvrez comment connecter l’application mobile Power BI à Reporting Services pour utiliser des rapports mobiles. Les rapports mobiles sont une fonctionnalité du mode natif.
ms.date: 12/17/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: c1a71522-394b-46a7-b9ec-f964bdd81d82
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 69e6f1b03b7ef6b1fb68d6c08cd1f526a6b481fa
ms.sourcegitcommit: f0772f614482e0b3cde3609e178689ce62ca3a19
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84545553"
---
# <a name="enable-a-report-server-for-power-bi-mobile-access"></a>Activer un serveur de rapports pour un accès Power BI Mobile
Vous pouvez utiliser l’application Power BI Mobile pour exploiter des rapports mobiles. Vous devez configurer quelques éléments pour permettre à l’application Power BI Mobile de se connecter à Reporting Services.  
  
-   [Rapports mobiles nécessitant Reporting Services en mode natif](#nativemode)  
-   [Activer l’authentification de base pour Reporting Services](#basicauth) (pour CTP 3.2)  
-   [activation recommandée du protocole HTTPS avec un certificat valide pour l’appareil client](#https)  
-   [Examiner les paramètres du pare-feu](#firewall)  
  
<a name="nativemode"/> 

## <a name="reporting-services-native-mode-required"></a>Reporting Services en mode natif requis  
Les rapports mobiles sont une fonctionnalité du mode natif. Ils ne sont pas disponibles en mode intégré SharePoint. l’application Power BI Mobile ne fonctionne qu’avec une instance en mode natif.  
  
<a name="basicauth"/>  

## <a name="enable-basic-authentication"></a>Activer l’authentification de base  
l’application Power BI Mobile iOS requiert l’authentification de base pour se connecter et exploiter des rapports mobiles. Reporting Services n’est pas configuré avec l’authentification de base activée par défaut. Pour plus d’informations sur la configuration de l’authentification de base, consultez [Configurer l’authentification de base sur le serveur de rapports](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md).  
  
Vous devez également activer l’authentification Windows pour que l’application d’édition publie le rapport Mobile.  
  
<a name="https"/>  

## <a name="enable-https"></a>Activer HTTPS  
Il est recommandé d’activer HTTPS dans Reporting Services si vous activez l’authentification de base. Si vous activez HTTPS, vérifiez que les appareils clients exécutant l’application Power BI Mobile iOS peuvent approuver les certificats utilisés. Cela signifie que la chaîne de certification doit être valide et accessible à l’appareil client.  
  
Si vous devez utiliser un certificat auto-signé à des fins d’évaluation ou de développement, vous pouvez l’exporter du serveur de rapports et l’installer sur l’appareil mobile. Pour plus d’informations sur l’installation, consultez la documentation de votre appareil.  
  
Pour plus d’informations sur l’activation de TLS, consultez [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
<a name="firewall"/>
  
## <a name="review-firewall-settings"></a>Examiner les paramètres du pare-feu  
Il est recommandé d’examiner vos paramètres de pare-feu pour vous assurer que tous les appareils peuvent se connecter à Reporting Services.   
  
Pour plus d’informations sur la configuration du pare-feu Windows, consultez [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
## <a name="see-also"></a>Voir aussi  
  
[Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
[Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)  
[Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
  
  
  
  
  


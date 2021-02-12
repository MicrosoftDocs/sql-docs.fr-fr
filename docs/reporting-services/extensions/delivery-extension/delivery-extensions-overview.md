---
title: Vue d’ensemble des extensions de remise | Microsoft Docs
description: Lisez une vue d’ensemble des extensions de remise, que vous pouvez utiliser pour fournir des rapports Reporting Services de différentes façons, par exemple par courrier électronique ou par partage de fichiers.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- subscriptions [Reporting Services], delivery extensions
- delivery extensions [Reporting Services], about extensions
ms.assetid: a30600a9-bbed-4519-9426-3470ff2982e7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8eecb8ce351fd067d944daf4fdd7fc9ee811341f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076650"
---
# <a name="delivery-extensions-overview"></a>Vue d'ensemble des extensions de remise
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permet aux utilisateurs de créer et de publier des rapports qui, une fois créés et publiés, peuvent être remis à différents emplacements. De plus, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] inclut plusieurs extensions de remise et une API de remise qui permettent aux développeurs de créer des extensions de remise supplémentaires pour étendre les fonctionnalités de remise proposées dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 Le tableau suivant répertorie les extensions de remise fournies avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Extension de remise|Description|  
|------------------------|-----------------|  
|Messagerie du serveur de rapports|Utilise un serveur SMTP pour envoyer électroniquement des rapports aux utilisateurs ou groupes individuels.|  
|Partage de fichiers du serveur de rapports|Utilisé pour distribuer des rapports dans votre organisation aux partages de fichier réseau. Permet de copier automatiquement un rapport dans un partage de fichiers dans le cadre d'une planification désignée.|  
  
 ![Architecture d'extension de remise de Reporting Services](../../../reporting-services/extensions/delivery-extension/media/bk-reportservicedelivery.gif "Architecture d'extension de remise de Reporting Services")  
Architecture d'extension de remise de Reporting Services  
  
 Les extensions de remise sont associées avec des abonnements. Lors de la création d'un abonnement, un utilisateur peut choisir l'une des extensions de remise disponibles pour déterminer le mode de remise du rapport. Dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], les abonnements se trouvent dans la base de données du serveur de rapports. Lorsqu'un événement se produit, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] associe l'événement aux abonnements contenus dans la base de données du serveur de rapports. Pour chaque abonnement attaché à l'événement, le serveur de rapports crée une notification. Pour les abonnements pilotés par les données, une notification est créée pour chaque destinataire. Une fois qu'une notification est créée, le serveur de rapports appelle une extension de remise particulière et passe dans les valeurs les paramètres d'extensions spécifiés dans la notification. L'extension de remise envoie la notification à l'utilisateur spécifiée par l'extension de remise sélectionnée.  
  
 Les extensions de remise implémentent l'API de l'extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. En prenant en charge l'API de l'extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], les extensions de remise peuvent recevoir des notifications du serveur de rapports et fournir l'état de la notification.  
  
 Le serveur de rapports ne gère pas de destinations de remise pour les notifications et les rapports. La collecte des informations de destination est prise en charge par le code que vous écrivez dans votre extension de remise.  
  
## <a name="subscriptions-and-delivery-extensions"></a>Extensions d'abonnements et de remise  
 Les applications clientes créent des abonnements qui utilisent des extensions de remise à l'aide de deux méthodes du service Web Report Server : <xref:ReportService2010.ReportingService2010.CreateSubscription%2A> et <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>. Pour modifier des abonnements déjà existants, les méthodes <xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> et <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A> sont utilisées. Lors de la création d'un abonnement, l'utilisateur sélectionne également une extension de remise pour l'abonnement et entre des valeurs pour les paramètres d'extension requis. Lorsqu'un utilisateur enregistre un abonnement, celui-ci est stocké dans la base de données du serveur de rapports. Les abonnements créent des notifications selon une planification ou un événement. Lorsqu'une remise commence, l'extension de remise sélectionnée charge en premier les données de configuration à partir du fichier de configuration. Ensuite, les paramètres d'extension pour l'abonnement sont extraits, et les valeurs sont définies. Enfin, la méthode <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.Deliver%2A> est appelée, et la notification est envoyée.  
  
## <a name="developer-requirements"></a>Spécifications pour le développeur  
 Pour développer une extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vous devez posséder :  
  
-   un ordinateur de déploiement sur lequel est installé un serveur de rapports ;  
  
-   un ordinateur de développement sur lequel est installé [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] ou le SDK [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] ;  
  
-   une connaissance détaillée des fonctions et fonctionnalités [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], notamment l'abonnement et la remise ;  
  
-   une compréhension approfondie de [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] et des contrôles Web si vous envisagez d'implémenter votre propre interface utilisateur d'abonnement pour le Gestionnaire de rapports ;  
  
-   de l’expérience en développement dans un langage [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] tel que [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  

---
title: Paramètres de site SharePoint pour le composant WebPart de la visionneuse de rapports - SSRS | Microsoft Docs
description: Découvrez comment configurer les paramètres de site SharePoint dans le composant WebPart de la visionneuse de rapports sur un serveur de rapports SQL Server.
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: jt000
ms.author: jasontre
ms.openlocfilehash: 8673f824762a9c7f6a28cd1232e742aac4428b23
ms.sourcegitcommit: 66a0672e47415dbd5cfd8d19075102c8c3973e70
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83764907"
---
# <a name="sharepoint-site-settings-for-the-report-viewer-web-part---reporting-services"></a>Paramètres de site SharePoint pour le composant WebPart de la visionneuse de rapports - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

Le composant WebPart de la visionneuse de rapports comporte deux paramètres qui peuvent être configurés. Ces paramètres peuvent être activés et désactivés dans la page Paramètres du site SharePoint, par un administrateur de site. Chaque site a ses propres paramètres. Par ailleurs, ces paramètres ne sont pas réinitialisés après la réinstallation du composant WebPart de la visionneuse de rapports.

## <a name="accessing-the-site-settings-page"></a>Accès à la page Paramètres du site

Pour accéder aux paramètres du site :

1. Dans votre site SharePoint, cliquez sur l’icône **engrenage** en haut à gauche et sélectionnez **Paramètres du site**.

    ![Paramètres du site à partir de l’icône d’engrenage.](media/sharepoint-site-settings.png)

2. En cliquant sur **Paramètres du composant WebPart de la visionneuse de rapports** dans le groupe de paramètres de site **Reporting Services**.

    > [!NOTE]
    > L’accès aux paramètres de site est également possible par un accès direct à `<site>/_layouts/15/ReportViewerWebPart/ReportViewerWebPartSettings.aspx`.

## <a name="report-viewer-web-part-settings"></a>Paramètres du composant WebPart de la visionneuse de rapports

|Paramètre|Commentaires|  
|-------------|--------------|  
|Collecter les données d’utilisation|Permet d’envoyer à Microsoft des informations relatives aux erreurs et à l’utilisation en vue d’améliorer les produits. Pour plus d’informations sur la stratégie de collecte de données pour le signalement d’erreurs Microsoft, consultez [Microsoft SQL Server Privacy Statement](https://go.microsoft.com/fwlink/?LinkID=868444).|  
|Activer les métadonnées d’accessibilité pour les rapports|Définit les [informations de périphérique `AccessibleTablix`](../html-device-information-settings.md) pour les rapports rendus.| 

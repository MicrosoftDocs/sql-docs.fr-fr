---
title: Prise en charge des versions de contrôle de la visionneuse de rapports
description: Le contrôle Microsoft Report Viewer est compatible avec SQL Server Reporting Services et Power BI Report Server, qui respectent la politique moderne de durée de vie du support.
author: maggiesMSFT
ms.custom: seo-lt-2019
ms.author: maggies
ms.reviewer: jonhp
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
ms.date: 12/01/2020
ms.openlocfilehash: fac7ebfc88432ef3bd8abde6c0e4e62115311520
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015639"
---
# <a name="support-for-report-viewer-current-branch-versions"></a>Prise en charge des versions Current Branch de Report Viewer

**_S’applique à: Microsoft Report Viewer versions 150.900.148 et ultérieures_**

Le **contrôle Microsoft Report Viewer** est compatible avec SQL Server Reporting Services et Power BI Report Server qui respectent la [politique moderne de durée de vie du support](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) de Microsoft. Ces informations s’appliquent aux versions **ASP.NET** et **WinForms** distribuées par le biais de [NuGet](https://www.nuget.org/). Toutes les versions publiées sont disponibles sur [NuGet](https://www.nuget.org/). Les correctifs, fonctionnalités ou autres mises à jour sont restaurés par progression vers la dernière version. Vous devez appliquer les versions les plus récentes pour recevoir des changements. Report Viewer continue de recevoir les **mises à jour critiques et de sécurité**, avec un préavis d’au moins un an concernant tout changement lié à la politique de support.

Pour un historique des versions du contrôle Report Viewer, consultez les liens suivants :

- [Windows Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/)
- [Formulaires web ASP.NET](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)

## <a name="application-server-and-report-server-combinations"></a>Combinaisons de serveurs d’applications et de serveurs de rapports

Certaines fonctionnalités du contrôle Visionneuse de rapports se calquent sur les comportements par défaut du système d'exploitation. Par conséquent, ils peuvent nécessiter l’exécution de la même version pour le client (le serveur d’applications exécutant le contrôle Visionneuse de rapports) et le serveur (exécutant Reporting Services). Les combinaisons de serveurs d’applications et de serveurs de rapports suivantes sont prises en charge :

| Serveur d’applications | Serveur de rapports |
| :----------------- | :------ |
| Windows Server 2012 | Windows Server 2012 |
| Windows Server 2012 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 |
| Windows Server 2016 et versions ultérieures | Windows Server 2016 et versions ultérieures |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le contrôle Visionneuse de rapports, consultez [Démarrage de l’intégration de Reporting Services à l’aide des contrôles de la visionneuse de rapports](integrating-reporting-services-using-reportviewer-controls-get-started.md).

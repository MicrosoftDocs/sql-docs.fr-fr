---
title: Effectuer le rendu d’un instantané d’historique de rapports à l’aide de l’accès URL | Microsoft Docs
description: Découvrez comment afficher un rapport reposant sur une capture instantanée d’historique de rapport en indiquant le paramètre rs:Snapshot et en définissant sa valeur sur un ID de capture instantanée valide.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services], report history
- history snapshots [Reporting Services]
- historical data [Reporting Services]
- snapshots [Reporting Services], URL access
- snapshots [Reporting Services], rendering report history
ms.assetid: 3f87f82d-0e61-4492-9c4b-f5238c39e8cd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 01a173da6f22b2f2ed1cc5c273b4e1f0f1c0ec1a
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87247476"
---
# <a name="render-a-report-history-snapshot-using-url-access"></a>Rendre un instantané d'historique de rapport à l'aide de l'accès URL
  Vous pouvez effectuer le rendu d’un rapport reposant sur une capture instantanée d’historique de rapport en indiquant le paramètre *rs:Snapshot* et en définissant sa valeur sur un ID de capture instantanée valide. La valeur de ce paramètre doit être spécifiée au format AAAA-MM-JJTHH:MM:SS, conformément à la norme ISO 8601.  
  
 Si vous omettez ce paramètre, le rapport est rendu selon le paramétrage des options d'exécution de rapports et de gestion du cache du serveur de rapports. Pour plus d’informations sur l’exécution des rapports, consultez [Définir les propriétés de traitement d’un rapport](../reporting-services/report-server/set-report-processing-properties.md).  
  
## <a name="example"></a>Exemple  
 L'exemple suivant montre une URL qui extrait un instantané d'historique de rapport :  
  
```  
https://myrshost/reportserver?/SampleReports/Company Sales&rs:Snapshot=2003-04-07T13:40:02  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Accès URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Référence de paramètres d'accès URL](../reporting-services/url-access-parameter-reference.md)  
  
  

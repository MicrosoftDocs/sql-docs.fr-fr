---
title: Utilisation de la classe RenderedOutputFile pour une extension de remise | Microsoft Docs
description: Découvrez comment les extensions de remise peuvent utiliser la classe RenderedOutputFile, qui stocke un rapport rendu ou des ressources de rapport.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- RenderedOutputFile class
- data streams [Reporting Services]
- delivery extensions [Reporting Services], data streams
ms.assetid: 8b591801-42d5-49fa-b710-bf7e6917accf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f743227a2927ad97c5a0bcc76c3634726969caef
ms.sourcegitcommit: 2f166e139f637d6edfb5731510d632a13205eb25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84529105"
---
# <a name="using-the-renderedoutputfile-class-for-a-delivery-extension"></a>Utilisation de la classe RenderedOutputFile pour une extension de remise
  La classe <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> représente un flux de données et des informations relatives aux propriétés associées du flux de données. La propriété **Data** de la classe <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> est utilisée pour représenter un rapport rendu ou signaler une ressource en tant qu’objet **Stream**.  
  
 La méthode <xref:Microsoft.ReportingServices.Interfaces.Report.Render%2A> de l’objet **Report** retourne un tableau d’un ou plusieurs objets <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> qui constituent un rapport rendu unique. Le premier objet <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> est le rapport rendu. Les autres objets <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> sont des ressources qui doivent être remises avec les données du rapport (par exemple, un fichier HTML et les images associées). Les extensions de rendu qui sont des extensions de rendu de flux unique (IMAGE, PDF, MHTML et EXCEL) renvoient un seul objet <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> dans le tableau.  
  
 Pour un exemple d’utilisation de la classe <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile>, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="see-also"></a>Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  

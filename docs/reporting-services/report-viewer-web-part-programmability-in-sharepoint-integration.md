---
title: Programmabilité du composant WebPart Visionneuse de rapports dans l’intégration SharePoint | Microsoft Docs
description: Découvrez comment créer des composants WebPart personnalisés qui fournissent les paramètres et le chemin du rapport au composant WebPart Visionneuse de rapports à l’aide de connexions de composants WebPart.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 714017b7-1bd6-4950-a3c6-d0df8450a877
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d7054e2af081bd76505f3adebbae4cc7541f8a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023273"
---
# <a name="report-viewer-web-part-programmability-in-sharepoint-integration"></a>Programmabilité du composant WebPart Visionneuse de rapports dans l'intégration SharePoint
  Le composant WebPart Visionneuse de rapports est un contrôle serveur qui contient un ensemble d’interfaces de programmation d’applications (API) publiques permettant aux développeurs de créer des applications SharePoint personnalisées. Vous pouvez créer des composants WebPart personnalisés qui fournissent les paramètres et le chemin du rapport au composant WebPart Visionneuse de rapports à l’aide de connexions de composants WebPart. Vous pouvez également incorporer le composant WebPart dans une page WebPart SharePoint personnalisée et le personnaliser en utilisant l'API publique.  
  
## <a name="connecting-to-report-viewer-web-part-with-custom-web-parts"></a>Connexion au composant WebPart Visionneuse de rapports avec des composants WebPart personnalisés  
 Le composant WebPart Visionneuse de rapports est un consommateur de connexions aux composants WebPart SharePoint qui implémentent <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> ou T:Microsoft.SharePoint.WebPartPages.IFilterValues. Un composant WebPart <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>, tel que le composant WebPart **Documents**, peut fournir le chemin d’un rapport à un composant WebPart Visionneuse de rapports s’il est placé dans la même page de composant WebPart que le composant WebPart Visionneuse de rapports. De la même façon, un composant WebPart T:Microsoft.SharePoint.WebPartPages.IFilterValues, tel que le composant **Filtre Texte** ou **Filtre Choix** peut fournir un paramètre de rapport à un composant WebPart Visionneuse de rapports s’il est placé dans la même page de composant WebPart que le composant WebPart Visionneuse de rapports.  
  
### <a name="implementing-a-report-path-provider-with-iwebpartrow"></a>Implémentation d'un fournisseur de chemins d'accès au rapport avec IWebPartRow  
 Pour fournir un chemin d'accès au rapport au composant WebPart Visionneuse de rapports via des connexions WebPart, procédez comme suit :  
  
1.  Créez un composant WebPart qui implémente l'interface <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>.  
  
2.  Ajoutez le composant WebPart à la même page WebPart que celle du composant WebPart Visionneuse de rapports.  
  
3.  Connectez votre composant WebPart au composant WebPart Visionneuse de rapports dans l'interface utilisateur du concepteur WebPart.  
  
    > [!NOTE]  
    >  Vous ne pouvez connecter qu’un seul composant WebPart <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> au composant WebPart Visionneuse de rapports à la fois, et vous ne pouvez pas connecter simultanément un composant WebPart <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> et un composant WebPart T:Microsoft.SharePoint.WebPartPages.IFilterValues au composant WebPart Visionneuse de rapports.  
  
 Pour que votre composant WebPart <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> fonctionne correctement avec T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart, vous devez effectuer les opérations suivantes dans la méthode <xref:System.Web.UI.WebControls.WebParts.IWebPartRow.GetRowData%2A> :  
  
-   Appelez la méthode de rappel avec un objet <xref:System.Data.DataRowView> en tant que paramètre d'entrée.  
  
-   Assurez-vous que l'objet <xref:System.Data.DataRowView> contient une colonne appelée « DocUrl » qui inclut le chemin d'accès au rapport.  
  
    > [!NOTE]  
    >  Le composant WebPart Visionneuse de rapports dans le complément pour [!INCLUDE[offSPServ](../includes/offspserv-md.md)] 2010 prend également en charge la réception des données de chemin d'accès au rapport par le biais de la colonne « FileRef ».  
  
### <a name="implementing-a-report-parameter-provider-with-ifiltervalues"></a>Implémentation d'un fournisseur de paramètres de rapport avec IFilterValues  
 Un composant WebPart qui implémente T:Microsoft.SharePoint.WebPartPages.IFilterValues peut fournir une valeur de paramètre au composant WebPart Visionneuse de rapports. La valeur de paramètre envoyée au composant WebPart Visionneuse de rapports est soumise aux mêmes restrictions appliquées au paramètre de rapport comme indiqué dans la définition de rapport (type de données, valeurs valides, etc.).  
  
 Pour fournir un paramètre de rapport au composant WebPart Visionneuse de rapports, procédez comme suit :  
  
1.  Créez un composant WebPart qui implémente l’interface T:Microsoft.SharePoint.WebPartPages.IFilterValues.  
  
2.  Ajoutez le composant WebPart à la même page que T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart.  
  
3.  Connectez votre composant WebPart T:Microsoft.SharePoint.WebPartPages.IFilterValues au composant WebPart Visionneuse de rapports dans l’interface utilisateur web du concepteur WebPart.  
  
    > [!NOTE]  
    >  Vous pouvez connecter plusieurs composants WebPart T:Microsoft.SharePoint.WebPartPages.IFilterValues à la fois au composant WebPart Visionneuse de rapports. Toutefois, vous ne pouvez pas connecter simultanément un composant WebPart <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> et un composant WebPart T:Microsoft.SharePoint.WebPartPages.IFilterValues Web Part au composant WebPart Visionneuse de rapports.  
  
  

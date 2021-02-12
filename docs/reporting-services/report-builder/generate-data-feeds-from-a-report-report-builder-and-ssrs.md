---
title: Générer des flux de données à partir d’un rapport (Générateur de rapports) | Microsoft Docs
description: Vous pouvez générer des flux de données conformes à Atom à partir de rapports paginés. Utilisez les flux dans des applications, telles que Power Pivot ou Power BI, qui sont capables de consommer des flux de données.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: e68baae2-9f2a-4f13-9179-9ac7f29111c5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61d02d7c5bf3e4de8199ebdc59f1c6f8ed157b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015649"
---
# <a name="generate-data-feeds-from-a-report-report-builder-and-ssrs"></a>Générer des flux de données à partir d'un rapport (Générateur de rapports et SSRS)

Vous pouvez générer des flux de données conformes à Atom à partir de rapports paginés, puis utiliser ces flux de données dans des applications, comme Power Pivot ou Power BI, qui sont capables d’utiliser des flux de données.  
  
 L'extension de rendu Atom [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] génère un document de service Atom qui répertorie les flux de données disponibles d'un rapport. Le document répertorie au moins un flux de données pour chaque région de données du rapport. Selon le type de région de données et les données affichées par cette région, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut générer plusieurs flux de données à partir d'une région de données.  
  
 Le document de service Atom contient un identificateur unique pour chaque flux de données. Par ailleurs, vous utilisez cet identificateur dans une URL afin d'afficher le contenu du flux de données.  
  
 Pour plus d’informations, consultez [Génération de flux de données à partir de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-generate-an-atom-service-document"></a>Pour générer un document de service Atom  
  
1.  du portail web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , naviguez jusqu’au rapport que vous souhaitez utiliser pour générer des flux de données.  
  
2.  Cliquez sur le rapport.  
  
     Le rapport s'exécute.  
  
3.  Dans la barre d’outils, cliquez sur l’icône **Exporter vers un flux de données** .  
  
     Un message s'affiche pour vous demander si vous souhaitez enregistrer ou ouvrir le document Atom qui contient le flux de données.  
  
4.  Cliquez sur **Enregistrer** pour enregistrer le document dans le système de fichiers, ou cliquez sur **Ouvrir** pour afficher le contenu du document avant de l'enregistrer. **Par défaut, le document s'ouvre dans un navigateur.**  
  
5.  Accédez à l'emplacement d'enregistrement du document.  
  
6.  Modifiez éventuellement le nom du document.  
  
    > [!NOTE]  
    >  Par défaut, le nom du document correspond au nom du rapport.  
  
7.  Assurez-vous que le type du document est **Fichier ATOMSVC**, puis cliquez sur **Enregistrer**.  
  
8.  Si vous le souhaitez, ouvrez le fichier .atomsvc dans un navigateur, un éditeur de texte ou un éditeur XML.  
  
### <a name="to-view-an-atom-compliant-data-feed"></a>Pour afficher un flux de données conforme à Atom  
  
1.  Si le document de service Atom n'est pas déjà ouvert, recherchez ce dernier et ouvrez-le dans un navigateur tel qu'Internet Explorer.  
  
2.  Copiez l'URL du flux de données que vous souhaitez afficher à partir du document de service Atom dans le navigateur.  
  
     Le format de l'URL est le suivant :  
  
     `https://<server name>/ReportServer?%2f<ReportName>rs%3aCommand=Render&rs%3aFormat=ATOM&rc%3aDataFeed=<Identifier>`  
  
3.  Appuyez sur Entrée.  
  
     Un message s'affiche pour vous demander si vous souhaitez enregistrer ou ouvrir le document Atom qui contient le flux de données.  
  
4.  Cliquez sur **Enregistrer** pour enregistrer le document dans le système de fichiers, ou cliquez sur **Ouvrir** pour afficher le flux de données avant de l'enregistrer.  
  
5.  Accédez à l'emplacement d'enregistrement du document.  
  
6.  Modifiez éventuellement le nom du document.  
  
    > [!NOTE]  
    >  Par défaut, le nom du document correspond au nom du rapport. Si le document de service Atom contient plusieurs flux, ils utilisent tous le même nom par défaut, à savoir le nom du rapport. Pour les différencier, renommez-les à l'aide de noms explicites.  
  
7.  Assurez-vous que le type du document est **Fichier ATOM**, puis cliquez sur **Enregistrer**.  
  
8.  Si vous le souhaitez, ouvrez le fichier .atom dans un navigateur, un éditeur de texte ou un éditeur XML.  

## <a name="next-steps"></a>Étapes suivantes

[Exporter des rapports](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

---
title: 'Procédure : Déployer un élément de rapport personnalisé | Microsoft Docs'
description: Découvrez comment déployer un élément de rapport personnalisé. Vous allez modifier les fichiers de configuration du serveur de rapports et copier les assemblys des composants dans les dossiers appropriés.
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, deploying
ms.assetid: 80e97b0d-e355-4240-aebd-08cbc84089ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 240be4e7d05e4daac27e15bf5f8a99ec6bc27563
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061904"
---
# <a name="how-to-deploy-a-custom-report-item"></a>Procédure : Déployer un élément de rapport personnalisé
  Pour déployer un élément de rapport personnalisé dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous devez d'abord modifier les fichiers de configuration du serveur de rapports, puis copier les assemblys correspondant aux composants de conception et d'exécution dans les dossiers d'application appropriés, et ce à la fois pour le Concepteur de rapports et le serveur de rapports.  
  
### <a name="to-deploy-a-custom-report-item"></a>Pour déployer un élément de rapport personnalisé  
  
1.  Modifiez le fichier Rsreportdesigner.config afin de configurer les composants de conception et d'exécution de l'élément de rapport personnalisé à utiliser dans le concepteur. Notez que l’entrée **ReportItemName** doit correspondre à l’attribut **CustomReportItemAttribute** utilisé dans votre classe **CustomReportItemDesigner**. Par exemple :  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    <ReportItemDesigner>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsDesigner, PolygonsDesigner" />  
    </ReportItemDesigner>  
    <ReportItemConverter>  
       <Converter Source="Chart" Target="Polygons" Type="PolygonsCRI.PolygonsConverter, PolygonsDesigner" />  
    </ReportItemConverter>  
    ```  
  
2.  Modifiez le fichier Rsreportserver.config pour inscrire le composant d'exécution de l'élément de rapport personnalisé. Par exemple :  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    ```  
  
3.  Modifiez le fichier Rssrvpolicy.config pour ajouter un **CodeGroup** octroyant les autorisations adéquates à l’élément de rapport personnalisé. Par exemple :  
  
    ```  
    <CodeGroup   
       class="UnionCodeGroup"   
       version="1"   
       PermissionSetName="FullTrust"  
       Description="This code group grants MyCustomReportItem.dll FullTrust permission. ">  
       <IMembershipCondition   
          class="UrlMembershipCondition"  
          version="1"  
       Url="C:\Program Files\Microsoft SQL Server\ MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin\MyCustomReportItem.dll" />  
    </CodeGroup>  
    ```  
  
4.  Copiez le fichier DLL correspondant au composant d'exécution de l'élément de rapport personnalisé dans les répertoires bin du serveur de rapports qui se trouvent à l'emplacement suivant : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies et \Program Files\Microsoft SQL Server\MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin.  
  
5.  Copiez le fichier DLL correspondant au composant de conception de l'élément de rapport personnalisé dans le répertoire PrivateAssemblies, lequel se trouve à l'emplacement suivant : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
## <a name="see-also"></a>Voir aussi  
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Bibliothèques de classes d'éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
  
  

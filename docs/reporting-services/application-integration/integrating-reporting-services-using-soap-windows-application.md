---
title: Utilisation de l'API SOAP dans les applications Windows
description: Vous pouvez accéder aux fonctionnalités du serveur de rapports avec l’API SOAP de Reporting Services. Accédez au service web dans une application Windows en effectuant des appels au service.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- rendered reports [Reporting Services]
- Windows applications [Reporting Services]
- Windows Forms [Reporting Services]
- SOAP [Reporting Services], Windows applications
ms.assetid: e4804792-20cd-4df2-9257-fb958ff447b4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a91f7cd3a95b722db1c595767ed71900bc55af55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082558"
---
# <a name="integrating-reporting-services-using-soap---windows-application"></a>Intégration de Reporting Services à l’aide de SOAP - Application Windows
  Vous pouvez accéder aux fonctionnalités complètes du serveur de rapports via l'API SOAP de Reporting Services. L'API SOAP est un service Web et, en tant que tel, est facilement accessible afin de fournir des fonctionnalités de création de rapports d'entreprise à vos applications de gestion personnalisées. Pour accéder au service Web dans une application Windows, il suffit d'écrire un code qui permet d'appeler le service. À l’aide du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vous pouvez générer une classe proxy qui expose les propriétés et méthodes du service web et vous permet d’utiliser une infrastructure et des outils familiers pour générer des applications métier basées sur la technologie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="integrating-report-management-functionality-using-windows-forms"></a>Intégration de fonctionnalités de gestion de rapports à l'aide de Windows Forms  
 Contrairement à l'accès URL, l'API SOAP expose le jeu complet des fonctions de gestion qui sont disponibles via le serveur de rapports. Cela signifie que toutes les fonctionnalités d'administration du Gestionnaire de rapports sont disponibles pour les développeurs via SOAP. Ainsi, vous pouvez développer un outil de gestion et d'administration complet à l'aide de Windows Forms. Par exemple, dans votre application Windows, vous pouvez souhaiter permettre à vos utilisateurs d'extraire le contenu de l'espace de noms du serveur de rapports. Pour cela, vous pouvez utiliser la méthode <xref:ReportService2010.ReportingService2010.ListChildren%2A> du service Web pour répertorier tous les éléments contenus dans la base de données du serveur de rapports, puis utiliser un contrôle ListView, TreeView ou ComboBox pour présenter ces éléments à vos utilisateurs. Vous pouvez utiliser le code de service Web suivant pour extraire la liste actuelle des rapports disponibles dans le dossier Mes rapports d'un utilisateur lorsqu'il clique sur un bouton sur un formulaire :  
  
```vb  
' Button click event that retrieves a list of reports from  
' the My Reports folder and displays them in a combo box  
Private Sub listReportsButton_Click(sender As Object, e As System.EventArgs)  
   ' Create a new Web service object and set credentials  
   ' to Windows Authentication  
   Dim rs As New ReportingService2010()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return the list of items in My Reports  
   Dim items As CatalogItem() = rs.ListChildren("/Adventureworks 2008 Sample Reports", False)  
  
   Dim ci As CatalogItem  
   For Each ci In  items  
      ' If the item is a report, add it to   
      ' a combo box  
      If ci.TypeName = "Report" Then  
         catalogComboBox.Items.Add(ci.Name)  
      End If  
   Next ci  
End Sub 'listReportsButton_Click  
```  
  
```csharp  
// Button click event that retrieves a list of reports from  
// the My Reports folder and displays them in a combo box  
private void listReportsButton_Click(object sender, System.EventArgs e)  
{  
   // Create a new Web service object and set credentials  
   // to Windows Authentication  
   ReportingService2010 rs = new ReportingService2010();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return the list of items in My Reports  
   CatalogItem[] items = rs.ListChildren("/Adventureworks 2008 Sample Reports", false);  
  
   foreach (CatalogItem ci in items)  
   {  
      // If the item is a report, add it to   
      // a combo box  
      if (ci.TypeName == "Report")  
         catalogComboBox.Items.Add(ci.Name);  
   }  
}  
```  
  
 À partir de là, vous pouvez permettre aux utilisateurs de sélectionner le rapport dans la liste déroulante afin de le prévisualiser sur le formulaire à l'aide d'un contrôle de navigateur Web ou d'un contrôle Image.  
  
## <a name="enabling-report-viewing-and-navigation-using-windows-forms"></a>Activation de la consultation des rapports et de la navigation à l'aide de Windows Forms  
 Il existe deux méthodes disponibles pour intégrer des rapports dans vos applications Windows Forms.  
  
 Vous pouvez utiliser l'API SOAP pour effectuer le rendu des rapports dans tous les formats de rendu pris en charge à l'aide de la méthode <xref:ReportExecution2005.ReportExecutionService.Render%2A>. Il existe de légers inconvénients à l'activation de la consultation des rapports et de la navigation via SOAP :  
  
-   Vous ne pouvez pas tirer parti des fonctionnalités intégrées de la barre d'outils Rapports incluse dans la Visionneuse HTML via l'accès URL.  
  
-   Si vous effectuez un rendu au format HTML, vous devez effectuer le rendu des images ou ressources séparément en tant que flux supplémentaires à l'aide de la méthode <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A>.  
  
-   Il existe un léger avantage, en termes de performances, en faveur du rendu de rapports à l'aide de l'accès URL plutôt qu'à l'aide de l'API SOAP.  
  
 Toutefois, la méthode <xref:ReportExecution2005.ReportExecutionService.Render%2A> de l'API SOAP peut être utilisée pour effectuer le rendu de rapports et les enregistrer sous divers formats de sortie par programme. Il s'agit d'un avantage par rapport à l'accès URL, qui requiert l'intervention de l'utilisateur. Lorsque vous effectuez le rendu d'un rapport à l'aide de la méthode <xref:ReportExecution2005.ReportExecutionService.Render%2A> de l'API SOAP, vous pouvez le faire dans tous les formats de sortie pris en charge.  
  
 Vous pouvez également utiliser les contrôles Visionneuse de rapports en distribution libre inclus dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsOrcas](../../includes/vsorcas-md.md)]. Les contrôles Visionneuse de rapports facilitent l'incorporation de fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans les applications personnalisées. Les contrôles Visionneuse de rapports sont destinés aux développeurs qui souhaitent fournir des rapports prédéfinis et entièrement créés dans un ensemble de fonctionnalités d'une application (par exemple, une application de gestion de sites web peut contenir des rapports qui comportent une analyse du parcours des internautes sur les sites web de sociétés). L'incorporation des contrôles dans une application offre une solution plus rationnelle que l'ajout de composants serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans le déploiement de votre application. Les contrôles offrent les fonctionnalités des rapports sans la prise en charge de la création, de la publication, de la distribution et de la fourniture qu'offre [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Il existe deux versions des contrôles Visionneuse de rapports : une version est destinée aux applications clientes Windows, l'autre aux applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Les contrôles prennent en charge les modes de traitement local et distant. En mode de traitement local, votre application fournit la définition de rapport et les datasets et déclenche le traitement des rapports. En mode de traitement distant, la récupération des données et le traitement des rapports sont effectués sur le serveur de rapports et le contrôle est utilisé à des fins d'affichage et de navigation dans les rapports. Ce modèle vous permet de créer des applications puissantes à l'échelle d'un Bureau ou d'une entreprise.  
  
 Les contrôles Visionneuse de rapports sont décrits dans l'aide en ligne de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Pour plus d'informations, consultez la documentation produit de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Intégration de Reporting Services dans des applications](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Utilisation de l’API SOAP dans une application web](../../reporting-services/application-integration/integrating-reporting-services-using-soap-web-application.md)  
  
  

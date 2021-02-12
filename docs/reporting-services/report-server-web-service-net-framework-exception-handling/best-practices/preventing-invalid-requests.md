---
title: Éviter les requêtes non valides | Microsoft Docs
description: Découvrez comment éviter les requêtes non valides en analysant le flux de votre application et en veillant à ce que les requêtes qui sont envoyées au serveur de rapports soient valides.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- invalid requests [Reporting Services]
- exceptions [Reporting Services], invalid requests
- valid requests [Reporting Services]
ms.assetid: 4a4a2d97-4c10-43a9-8298-ef5a820ea549
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1b52b2f8285175c082c83b01fc9b59332ec2fb5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023499"
---
# <a name="preventing-invalid-requests"></a>Éviter les demandes non valides
  Vous pouvez éviter que certains types d'exceptions soient levés en analysant le flux de votre application et en veillant à ce que les demandes qui sont envoyées au serveur de rapports soient valides. Par exemple, dans les applications qui permettent aux utilisateurs d'ajouter ou de mettre à jour le nom d'un rapport, d'une source de données ou d'autres éléments de serveur de rapports, vous devez valider le texte entré par un utilisateur. Vous devez toujours vérifier que la demande ne contient pas de caractères réservés avant de l'envoyer à un serveur de rapports. Utilisez des instructions **if** conditionnelles ou d’autres constructions logiques dans votre code pour avertir l’utilisateur que les conditions nécessaires à l’envoi de demandes au serveur de rapports n’ont pas été réunies.  
  
 Dans l'exemple C# simplifié suivant, un message d'erreur convivial s'affiche lorsque les utilisateurs essaient de créer un rapport dont le nom contient une barre oblique (/).  
  
```  
// C#  
private void PublishReport()  
{  
   int index;  
   string reservedChar;  
   string message;  
  
   // Check the text value of the name text box for "/",  
   // a reserved character  
   index = nameTextBox.Text.IndexOf(@"/");  
  
   if ( index != -1) // The text contains the character  
   {  
      reservedChar = nameTextBox.Text.Substring(index, 1);  
      // Build a user-friendly error message  
      message = "The name of the report cannot contain the reserved character " +  
         "\"" + reservedChar + "\". " +  
         "Please enter a valid name for the report. " +  
         "For more information about reserved characters, " +  
         "see the help documentation";  
  
      MessageBox.Show(message, "Invalid Input Error");  
   }  
   else // Publish the report  
   {  
      Byte[] definition = null;  
      Warning[] warnings = {};  
      string name = nameTextBox.Text;  
  
      FileStream stream = File.OpenRead("MyReport.rdl");  
      definition = new Byte[stream.Length];  
      stream.Read(definition, 0, (int) stream.Length);  
      stream.Close();  
      // Create report with user-defined name  
      rs.CreateCatalogItem("Report", name, "/Samples", false, definition, null, out warnings);  
      MessageBox.Show("Report: {0} created successfully", name);  
   }  
}  
```  
  
 Pour plus d’informations sur les types d’erreurs qui peuvent être évitées avant l’envoi de demandes au serveur de rapports, consultez [Table d’erreurs SoapException](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/soapexception-errors-table.md). Pour plus d’informations sur la manière d’améliorer l’exemple précédent à l’aide de blocs try/catch, consultez [Utilisation des blocs Try/Catch](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/using-try-and-catch-blocks.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation de la gestion des exceptions dans Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)  
  
  

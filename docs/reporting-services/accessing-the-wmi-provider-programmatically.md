---
title: Accès au fournisseur WMI par programmation | Microsoft Docs
description: Découvrez ce qu’est un fournisseur WMI, comment trouver une instance de serveur de rapports et comment accéder au fournisseur WMI par programmation.
ms.date: 11/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 67bd266b-1484-4863-8152-060a993420a9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1886b2520c8b5a2228e67aa38e1dd6091a023b33
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87246363"
---
# <a name="accessing-the-wmi-provider-programmatically"></a>Accès au fournisseur WMI par programmation

## <a name="wmi-provider-overview"></a>Vue d'ensemble du fournisseur WMI  
 L’espace de noms utilisé pour obtenir des informations sur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] dans les exemples de code contenus dans cette rubrique est **System.Management**, qui provient du [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)]. L’espace de noms **System.Management** fournit un ensemble de classes de code managé par le biais desquelles les applications [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] peuvent accéder à des informations de gestion et les manipuler. Pour plus d’informations sur l’utilisation des classes WMI de Reporting Services à l’aide de l’espace de noms **System.Management**, consultez « Accès aux informations de gestion avec System.Management » dans le SDK [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)].  
  
## <a name="finding-a-report-server-instance"></a>Recherche d'une instance de serveur de rapports  
 La méthode privilégiée pour rechercher des informations sur vos installations de serveur de rapports consiste à énumérer la collection d'instances WMI. L'exemple suivant indique comment rechercher des propriétés sur chaque instance de serveur de rapports en créant une collection et en effectuant une boucle dans la collection pour afficher les propriétés.  
  
```vb  
Imports System  
Imports System.Management  
Imports System.IO  
  
Module Module1  
    Sub Main()  
        Const WmiNamespace As String = "\\<ServerName>\root\Microsoft\SqlServer\ReportServer\<InstanceName>\v10\Admin"  
        Const WmiRSClass As String = _  
           "\\<ServerName>\root\Microsoft\SqlServer\ReportServer\<InstanceName>\v13\admin:MSReportServer_ConfigurationSetting"  
  
        Dim serverClass As ManagementClass  
        Dim scope As ManagementScope  
        scope = New ManagementScope(WmiNamespace)  
        'Connect to the Reporting Services namespace.  
        scope.Connect()  
  
        'Create the server class.  
        serverClass = New ManagementClass(WmiRSClass)  
        'Connect to the management object.  
        serverClass.Get()  
        If serverClass Is Nothing Then Throw New Exception("No class found")  
  
        'Loop through the instances of the server class.  
        Dim instances As ManagementObjectCollection = serverClass.GetInstances()  
        Dim instance As ManagementObject  
        For Each instance In instances  
            Console.Out.WriteLine("Instance Detected")  
            Dim instProps As PropertyDataCollection = instance.Properties  
            Dim prop As PropertyData  
            For Each prop In instProps  
                Dim name As String = prop.Name  
                Dim val As Object = prop.Value  
                Console.Out.Write("Property Name: " + name)  
                If val Is Nothing Then  
                    Console.Out.WriteLine("     Value: <null>")  
                Else  
                    Console.Out.WriteLine("     Value: " + val.ToString())  
                End If  
            Next  
        Next  
  
        Console.WriteLine("--- Press any key ---")  
        Console.ReadKey()  
  
    End Sub  
End Module  
```  
  
```csharp  
using System;  
using System.Management;  
using System.IO;  
[assembly: CLSCompliant(true)]  
  
class Class1  
{  
    [STAThread]  
    static void Main(string[] args)  
    {  
        const string WmiNamespace = @"\\<ServerName>\root\Microsoft\SqlServer\ReportServer\<InstanceName>\v10\Admin";  
        const string WmiRSClass =  
          @"\\<ServerName>\root\Microsoft\SqlServer\ReportServer\<InstanceName>\v13\admin:MSReportServer_ConfigurationSetting";  
        ManagementClass serverClass;  
        ManagementScope scope;  
        scope = new ManagementScope(WmiNamespace);  
  
        // Connect to the Reporting Services namespace.  
        scope.Connect();  
        // Create the server class.  
        serverClass = new ManagementClass(WmiRSClass);  
        // Connect to the management object.  
        serverClass.Get();  
        if (serverClass == null)  
            throw new Exception("No class found");  
  
        // Loop through the instances of the server class.  
        ManagementObjectCollection instances = serverClass.GetInstances();  
  
        foreach (ManagementObject instance in instances)  
        {  
            Console.Out.WriteLine("Instance Detected");  
            PropertyDataCollection instProps = instance.Properties;  
            foreach (PropertyData prop in instProps)  
            {  
                string name = prop.Name;  
                object val = prop.Value;  
                Console.Out.Write("Property Name: " + name);  
                if (val != null)  
                    Console.Out.WriteLine("     Value: " + val.ToString());  
                else  
                    Console.Out.WriteLine("     Value: <null>");  
            }  
        }  
        Console.WriteLine("\n--- Press any key ---");  
        Console.ReadKey();  
    }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Accéder au fournisseur WMI de Reporting Services](../reporting-services/tools/access-the-reporting-services-wmi-provider.md)   
 [Fichier de configuration RSReportServer.config](../reporting-services/report-server/rsreportserver-config-configuration-file.md)  
  
  

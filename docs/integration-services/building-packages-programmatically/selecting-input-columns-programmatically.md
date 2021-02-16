---
description: Sélection de colonnes d'entrée par programme
title: Sélection de colonnes d’entrée par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- input column mapping
- data flow task [Integration Services], components
- data flow task [Integration Services], column mapping
- mapping input columns [Integration Services]
- column mapping [Integration Services]
- data flow [Integration Services], column mapping
- data flow [Integration Services], components
ms.assetid: b53b110a-dcf4-4464-ae98-81e892ab74c3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c23014385a57bf17ca77bf11e9614f6b6ea1f76f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355035"
---
# <a name="selecting-input-columns-programmatically"></a>Sélection de colonnes d'entrée par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir connecté des composants par programme, sélectionnez les colonnes des composants en amont que vous transformerez ou passerez aux composants en aval. Si vous ne sélectionnez pas de colonnes d'entrée pour votre composant, le composant ne reçoit pas de lignes de la part de la tâche de flux de données.  
  
## <a name="selecting-columns"></a>Sélection des colonnes  
 Appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100.GetVirtualInput%2A> pour extraire la liste des colonnes disponibles d'un composant en amont, puis appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.SetUsageType%2A> de l'instance du composant au moment de la conception pour sélectionner des colonnes dans la collection de colonnes d'entrée virtuelle. Lorsque vous appelez cette méthode, le composant crée une colonne d'entrée dans sa collection de colonnes d'entrée dont l'ID de lignage est identique à la colonne correspondante dans la collection de sortie du composant en amont.  
  
 Ne sélectionnez pas de colonnes en appelant directement la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSVirtualInput100.SetUsageType%2A> de l'objet d'entrée virtuel, parce que celle-ci passe outre la capacité du composant à rejeter des colonnes en raison de types de données ou d'autres propriétés inappropriés.  
  
## <a name="sample"></a>Exemple  
 L'exemple de code suivant montre comment utiliser l'instance au moment de la conception d'un composant pour sélectionner les colonnes d'un composant.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      // Create a package and add a Data Flow task.  
      Package package = new Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Add an OLE DB connection manager to the package.  
      ConnectionManager conMgr = package.Connections.Add("OLEDB");  
      conMgr.ConnectionString = "Provider=SQLOLEDB.1;" +  
        "Data Source=<servername>;Initial Catalog=AdventureWorks;" +  
        "Integrated Security=SSPI;";  
      conMgr.Name = "SSIS Connection Manager for OLE DB";  
      conMgr.Description = "OLE DB connection to the AdventureWorks database.";  
  
      // Create and configure an OLE DB source component.    
      IDTSComponentMetaData100 source =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      source.ComponentClassID = "DTSAdapter.OleDbSource";  
      // Create the design-time instance of the source.  
      CManagedComponentWrapper srcDesignTime = source.Instantiate();  
      // The ProvideComponentProperties method creates a default output.  
      srcDesignTime.ProvideComponentProperties();  
      // Assign the connection manager.  
      source.RuntimeConnectionCollection[0].ConnectionManager =  
        DtsConvert.GetExtendedInterface(conMgr);  
      // Set the custom properties of the source.  
      srcDesignTime.SetComponentProperty("AccessMode", 2);  
      srcDesignTime.SetComponentProperty("SqlCommand",  
        "Select * from Production.Product");  
  
      // Connect to the data source,  
      //  and then update the metadata for the source.  
      srcDesignTime.AcquireConnections(null);  
      srcDesignTime.ReinitializeMetaData();  
      srcDesignTime.ReleaseConnections();  
  
      // Create and configure an OLE DB destination.  
      IDTSComponentMetaData100 destination =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      destination.ComponentClassID = "DTSAdapter.OleDbDestination";  
      // Create the design-time instance of the destination.  
      CManagedComponentWrapper destDesignTime = destination.Instantiate();  
      // The ProvideComponentProperties method creates a default input.  
      destDesignTime.ProvideComponentProperties();  
  
      // Create the path from source to destination.  
      IDTSPath100 path = dataFlowTask.PathCollection.New();  
      path.AttachPathAndPropagateNotifications(source.OutputCollection[0],  
        destination.InputCollection[0]);  
  
      // Get the destination's default input and virtual input.  
      IDTSInput100 input = destination.InputCollection[0];  
      IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
      // Iterate through the virtual input column collection.  
      foreach (IDTSVirtualInputColumn100 vColumn in vInput.VirtualInputColumnCollection)  
      {  
        // Call the SetUsageType method of the destination  
        //  to add each available virtual input column as an input column.  
        destDesignTime.SetUsageType(  
           input.ID, vInput, vColumn.LineageID, DTSUsageType.UT_READONLY);  
      }  
  
      // Verify that the columns have been added to the input.  
      foreach (IDTSInputColumn100 inputColumn in destination.InputCollection[0].InputColumnCollection)  
        Console.WriteLine(inputColumn.Name);  
      Console.Read();  
  
      // Add other components to the data flow and connect them.  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    ' Create a package and add a Data Flow task.  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Add an OLE DB connection manager to the package.  
    Dim conMgr As ConnectionManager = package.Connections.Add("OLEDB")  
    conMgr.ConnectionString = "Provider=SQLOLEDB.1;" & _  
      "Data Source=<servername>;Initial Catalog=AdventureWorks;" & _  
      "Integrated Security=SSPI;"  
    conMgr.Name = "SSIS Connection Manager for OLE DB"  
    conMgr.Description = "OLE DB connection to the AdventureWorks database."  
  
    ' Create and configure an OLE DB source component.    
    Dim source As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New  
    source.ComponentClassID = "DTSAdapter.OleDbSource"  
    ' Create the design-time instance of the source.  
    Dim srcDesignTime As CManagedComponentWrapper = source.Instantiate  
    ' The ProvideComponentProperties method creates a default output.  
    srcDesignTime.ProvideComponentProperties()  
    ' Assign the connection manager.  
    source.RuntimeConnectionCollection(0).ConnectionManager = _  
      DtsConvert.GetExtendedInterface(conMgr)  
    ' Set the custom properties of the source.  
    srcDesignTime.SetComponentProperty("AccessMode", 2)  
    srcDesignTime.SetComponentProperty("SqlCommand", _  
      "Select * from Production.Product")  
  
    ' Connect to the data source,  
    '  and then update the metadata for the source.  
    srcDesignTime.AcquireConnections(Nothing)  
    srcDesignTime.ReinitializeMetaData()  
    srcDesignTime.ReleaseConnections()  
  
    ' Create and configure an OLE DB destination.  
    Dim destination As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New  
    destination.ComponentClassID = "DTSAdapter.OleDbDestination"  
    ' Create the design-time instance of the destination.  
    Dim destDesignTime As CManagedComponentWrapper = destination.Instantiate  
    ' The ProvideComponentProperties method creates a default input.  
    destDesignTime.ProvideComponentProperties()  
  
    ' Create the path from source to destination.  
    Dim path As IDTSPath100 = dataFlowTask.PathCollection.New  
    path.AttachPathAndPropagateNotifications(source.OutputCollection(0), _  
      destination.InputCollection(0))  
  
    ' Get the destination's default input and virtual input.  
    Dim input As IDTSInput100 = destination.InputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput  
  
    ' Iterate through the virtual input column collection.  
    For Each vColumn As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
      ' Call the SetUsageType method of the destination  
      '  to add each available virtual input column as an input column.  
      destDesignTime.SetUsageType(input.ID, vInput, vColumn.LineageID, DTSUsageType.UT_READONLY)  
    Next  
  
    ' Verify that the columns have been added to the input.  
    For Each inputColumn As IDTSInputColumn100 In destination.InputCollection(0).InputColumnCollection  
      Console.WriteLine(inputColumn.Name)  
    Next  
    Console.Read()  
  
    ' Add other components to the data flow and connect them.  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrement d'un package par programme](../../integration-services/building-packages-programmatically/saving-a-package-programmatically.md)  
  
  

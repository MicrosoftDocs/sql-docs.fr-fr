---
description: Connexion de composants de flux de données par programme
title: Connexion de composants de flux de données par programmation | Microsoft Docs
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
- data flow task [Integration Services], components
- paths [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: 404ecab7-7698-447b-93d6-dd256beb11ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 73e8eb7b506050489581ad6b4d00b02a51049f48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355071"
---
# <a name="connecting-data-flow-components-programmatically"></a>Connexion de composants de flux de données par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir ajouté des composants à la tâche de flux de données, vous devez les connecter pour créer une arborescence d'exécution qui représente le flux de données des sources vers des destinations en passant par des transformations. Ces objets <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> permettent de connecter les composants du flux de données.  
  
## <a name="creating-a-path"></a>Création d'un chemin d'accès  
 Appelez la méthode New de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.PathCollection%2A> de l’interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipe> pour créer un chemin et l’ajouter à la collection de chemins dans la tâche de flux de données. Cette méthode retourne un nouvel objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> déconnecté, que vous utilisez alors pour connecter deux composants.  
  
 Appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100.AttachPathAndPropagateNotifications%2A> pour connecter le chemin d'accès et indiquer aux composants faisant partie du chemin d'accès qu'ils ont été connectés. Cette méthode accepte un <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> du composant en amont et un <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> du composant en aval en tant que paramètres. Par défaut, l'appel à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> du composant crée une entrée unique pour les composants qui possèdent des entrées, et une sortie unique pour les composants qui possèdent des sorties. L'exemple suivant utilise cette sortie par défaut de la source et cette entrée par défaut de la destination.  
  
## <a name="next-step"></a>étape suivante  
 Après avoir établi un chemin entre deux composants, l’étape suivante consiste à mapper les colonnes d’entrée dans le composant en aval, comme indiqué dans la nouvelle rubrique, [Sélection de colonnes d’entrée par programmation](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md).  
  
## <a name="sample"></a>Exemple  
 L'exemple de code suivant indique comment établir un chemin d'accès entre deux composants.  
  
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
      Package package = new Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Create the source component.    
      IDTSComponentMetaData100 source =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      source.ComponentClassID = "DTSAdapter.OleDbSource";  
      CManagedComponentWrapper srcDesignTime = source.Instantiate();  
      srcDesignTime.ProvideComponentProperties();  
  
      // Create the destination component.  
      IDTSComponentMetaData100 destination =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      destination.ComponentClassID = "DTSAdapter.OleDbDestination";  
      CManagedComponentWrapper destDesignTime = destination.Instantiate();  
      destDesignTime.ProvideComponentProperties();  
  
      // Create the path.  
      IDTSPath100 path = dataFlowTask.PathCollection.New();  
      path.AttachPathAndPropagateNotifications(source.OutputCollection[0],  
        destination.InputCollection[0]);  
    }  
  }  
```  
  
 }  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Create the source component.    
    Dim source As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    source.ComponentClassID = "DTSAdapter.OleDbSource"  
    Dim srcDesignTime As CManagedComponentWrapper = source.Instantiate()  
    srcDesignTime.ProvideComponentProperties()  
  
    ' Create the destination component.  
    Dim destination As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    destination.ComponentClassID = "DTSAdapter.OleDbDestination"  
    Dim destDesignTime As CManagedComponentWrapper = destination.Instantiate()  
    destDesignTime.ProvideComponentProperties()  
  
    ' Create the path.  
    Dim path As IDTSPath100 = dataFlowTask.PathCollection.New()  
    path.AttachPathAndPropagateNotifications(source.OutputCollection(0), _  
      destination.InputCollection(0))  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sélection de colonnes d'entrée par programme](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
  
  

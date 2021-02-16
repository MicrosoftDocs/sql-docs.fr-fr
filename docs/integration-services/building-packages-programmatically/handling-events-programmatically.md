---
description: Gestion d'événements par programme
title: Gestion d’événements par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- Integration Services packages, events
- SQL Server Integration Services packages, events
- SSIS events, programmatically handling
- packages [Integration Services], events
- DtsEventHandler object
- SSIS packages, events
- SSIS events
- events [Integration Services], programmatically
- tasks [Integration Services], events
- IDTSEvents interface
ms.assetid: 0f00bd66-efd5-4f12-9e1c-36195f739332
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a2800cbfc74bf87225f93d480d54777134be8f37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355054"
---
# <a name="handling-events-programmatically"></a>Gestion d'événements par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le Runtime [!INCLUDE[ssIS](../../includes/ssis-md.md)] fournit une collection d'événements qui se produisent avant, pendant et après la validation et l'exécution d'un package. Ces événements peuvent être capturés de deux manières. La première méthode consiste à implémenter l’interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> dans une classe et à fournir la classe en tant que paramètre aux méthodes **Execute** et **Validate** du package. La deuxième méthode consiste à créer des objets <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>, qui peuvent contenir d'autres objets [!INCLUDE[ssIS](../../includes/ssis-md.md)], tels que des tâches et des boucles, exécutés lorsqu'un événement se produit dans <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>. Cette section décrit ces deux méthodes et fournit des exemples de code pour illustrer leur utilisation.  
  
## <a name="receiving-idtsevents-callbacks"></a>Réception de rappels IDTSEvents  
 Les développeurs qui créent et exécutent des packages par programme peuvent recevoir des notifications d'événements au cours du processus de validation et d'exécution à l'aide de l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>. Cette opération s’effectue en créant une classe qui implémente l’interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> et en fournissant cette classe en tant que paramètre aux méthodes **Validate** et **Execute** d’un package. Les méthodes de la classe sont ensuite appelées par le moteur d'exécution lorsque les événements se produisent.  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> est une classe qui implémente déjà l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> ; par conséquent, une autre manière d'implémenter directement l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> consiste à la dériver de la classe <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> et à substituer les événements spécifiques auxquels vous souhaitez répondre. Vous fournissez alors votre classe en tant que paramètre aux méthodes **Validate** et **Execute** de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Package> pour recevoir des rappels d’événements.  
  
 L'exemple de code suivant présente une classe qui dérive de <xref:Microsoft.SqlServer.Dts.Runtime.DefaultEvents> et qui substitue la méthode <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnPreExecute%2A>. Puis, la classe est fournie en tant que paramètre aux méthodes **Validate** et **Execute** du package.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      MyEventsClass eventsClass = new MyEventsClass();  
  
      p.Validate(null, null, eventsClass, null);  
      p.Execute(null, null, eventsClass, null, null);  
  
      Console.Read();  
    }  
  }  
  class MyEventsClass : DefaultEvents  
  {  
    public override void OnPreExecute(Executable exec, ref bool fireAgain)  
    {  
      // TODO: Add custom code to handle the event.  
      Console.WriteLine("The PreExecute event of the " +  
        exec.ToString() + " has been raised.");  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim eventsClass As MyEventsClass = New MyEventsClass()  
  
    p.Validate(Nothing, Nothing, eventsClass, Nothing)  
    p.Execute(Nothing, Nothing, eventsClass, Nothing, Nothing)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
Class MyEventsClass  
  Inherits DefaultEvents  
  
  Public Overrides Sub OnPreExecute(ByVal exec As Executable, ByRef fireAgain As Boolean)  
  
    ' TODO: Add custom code to handle the event.  
    Console.WriteLine("The PreExecute event of the " & _  
      exec.ToString() & " has been raised.")  
  
  End Sub  
  
End Class  
```  
  
## <a name="creating-dtseventhandler-objects"></a>Création d'objets DtsEventHandler  
 Le moteur d'exécution fournit un système de gestion et de notification d'événements fiable et très souple par le biais de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>. Ces objets vous permettent de concevoir des flux de travail complets dans le gestionnaire d'événements, qui s'exécutent uniquement lorsque l'événement auquel le gestionnaire d'événements appartient se produit. L'objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> est un conteneur qui s'exécute lors du déclenchement de l'événement correspondant sur son objet parent. Cette architecture vous permet de créer des flux de travail isolés qui s'exécutent en réponse à des événements qui se produisent sur un conteneur. Comme les objets <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> sont synchrones, l'exécution s'arrête tant que les gestionnaires d'événements attachés à l'événement ne sont pas retournés.  
  
 Le code suivant montre comment créer un objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>. Le code ajoute <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> à la collection <xref:Microsoft.SqlServer.Dts.Runtime.Package.Executables%2A> du package, puis crée un objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> pour l'événement <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A> de la tâche. <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> est ajouté au gestionnaire d'événements, qui s'exécute lorsque l'événement <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnError%2A> se produit pour le premier <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>. Cet exemple suppose que vous disposez d'un fichier nommé C:\Windows\Temp\DemoFile.txt à des fins de test. La première fois que vous exécutez l'exemple, le fichier est correctement copié et le gestionnaire d'événements n'est pas appelé. Lorsque vous exécutez l’exemple une seconde fois, le premier <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> ne parvient pas à copier le fichier (car la valeur de <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask.OverwriteDestinationFile%2A> est **false**), le gestionnaire d’événements est appelé, le deuxième <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask> supprime le fichier source et le package signale un échec en raison de l’erreur qui s’est produite.  
  
## <a name="example"></a> Exemple  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string f = "DemoFile.txt";  
      Executable e;  
      TaskHost th;  
      FileSystemTask fst;  
  
      Package p = new Package();  
  
      p.Variables.Add("sourceFile", true, String.Empty,  
        @"C:\Windows\Temp\" + f);  
      p.Variables.Add("destinationFile", true, String.Empty,  
        Path.Combine(Directory.GetCurrentDirectory(), f));  
  
      // Create a first File System task and add it to the package.  
      // This task tries to copy a file from one folder to another.  
      e = p.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask1";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.CopyFile;  
      fst.OverwriteDestinationFile = false;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
      fst.Destination = "destinationFile";  
      fst.IsDestinationPathVariable = true;  
  
      // Add an event handler for the FileSystemTask's OnError event.  
      DtsEventHandler ehOnError = (DtsEventHandler)th.EventHandlers.Add("OnError");  
  
      // Create a second File System task and add it to the event handler.  
      // This task deletes the source file if the event handler is called.  
      e = ehOnError.Executables.Add("STOCK:FileSystemTask");  
      th = e as TaskHost;  
      th.Name = "FileSystemTask2";  
      fst = th.InnerObject as FileSystemTask;  
  
      fst.Operation = DTSFileSystemOperation.DeleteFile;  
      fst.Source = "sourceFile";  
      fst.IsSourcePathVariable = true;  
  
      DTSExecResult vr = p.Validate(null, null, null, null);  
      Console.WriteLine("ValidationResult = " + vr.ToString());  
      if (vr == DTSExecResult.Success)  
      {  
        DTSExecResult er = p.Execute(null, null, null, null, null);  
        Console.WriteLine("ExecutionResult = " + er.ToString());  
        if (er == DTSExecResult.Failure)  
          if (!File.Exists(@"C:\Windows\Temp\" + f))  
            Console.WriteLine("Source file has been deleted by the event handler.");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim f As String = "DemoFile.txt"  
    Dim e As Executable  
    Dim th As TaskHost  
    Dim fst As FileSystemTask  
  
    Dim p As Package = New Package()  
  
    p.Variables.Add("sourceFile", True, String.Empty, _  
      "C:\Windows\Temp\" & f)  
    p.Variables.Add("destinationFile", True, String.Empty, _  
      Path.Combine(Directory.GetCurrentDirectory(), f))  
  
    ' Create a first File System task and add it to the package.  
    ' This task tries to copy a file from one folder to another.  
    e = p.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.CopyFile  
    fst.OverwriteDestinationFile = False  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
    fst.Destination = "destinationFile"  
    fst.IsDestinationPathVariable = True  
  
    ' Add an event handler for the FileSystemTask's OnError event.  
    Dim ehOnError As DtsEventHandler = CType(th.EventHandlers.Add("OnError"), DtsEventHandler)  
  
    ' Create a second File System task and add it to the event handler.  
    ' This task deletes the source file if the event handler is called.  
    e = ehOnError.Executables.Add("STOCK:FileSystemTask")  
    th = CType(e, TaskHost)  
    th.Name = "FileSystemTask1"  
    fst = CType(th.InnerObject, FileSystemTask)  
  
    fst.Operation = DTSFileSystemOperation.DeleteFile  
    fst.Source = "sourceFile"  
    fst.IsSourcePathVariable = True  
  
    Dim vr As DTSExecResult = p.Validate(Nothing, Nothing, Nothing, Nothing)  
    Console.WriteLine("ValidationResult = " + vr.ToString())  
    If vr = DTSExecResult.Success Then  
      Dim er As DTSExecResult = p.Execute(Nothing, Nothing, Nothing, Nothing, Nothing)  
      Console.WriteLine("ExecutionResult = " + er.ToString())  
      If er = DTSExecResult.Failure Then  
        If Not File.Exists("C:\Windows\Temp\" + f) Then  
          Console.WriteLine("Source file has been deleted by the event handler.")  
        End If  
      End If  
    End If  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaires d’événements Integration Services &#40;SSIS&#41](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Ajouter un gestionnaire d’événements à un package](../integration-services-ssis-event-handlers.md)  
  

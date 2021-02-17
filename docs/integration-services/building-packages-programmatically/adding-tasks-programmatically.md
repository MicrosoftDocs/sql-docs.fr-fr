---
description: Ajout de tâches par programme
title: Ajout de tâches par programmation | Microsoft Docs
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
- tasks [Integration Services], packages
- adding package tasks
ms.assetid: 5d4652d5-228c-4238-905c-346dd8503fdf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1650323cede5f91401faa97e820917b2549d5050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355108"
---
# <a name="adding-tasks-programmatically"></a>Ajout de tâches par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il est possible d'ajouter des tâches aux types d'objets suivants dans le moteur d'exécution :  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Package>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Sequence>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>  
  
 Ces classes, considérées comme des conteneurs, héritent toutes de la propriété <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSequence.Executables%2A>. Des conteneurs peuvent contenir une collection de tâches, qui sont des objets exécutables traités par le runtime au cours de l'exécution du conteneur. L'ordre d'exécution des objets dans la collection est déterminé par n'importe quel ensemble <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> sur chaque tâche dans les conteneurs. Les contraintes de précédence permettent la création de branches d'exécution basées sur le succès, l'échec ou l'achèvement d'un <xref:Microsoft.SqlServer.Dts.Runtime.Executable> dans la collection.  
  
 Chaque conteneur possède une collection <xref:Microsoft.SqlServer.Dts.Runtime.Executables> qui contient les objets <xref:Microsoft.SqlServer.Dts.Runtime.Executable> individuels. Chaque tâche exécutable hérite des méthodes <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A> et les implémente. Ces deux méthodes sont appelées par le moteur d'exécution pour traiter chaque <xref:Microsoft.SqlServer.Dts.Runtime.Executable>.  
  
 Pour ajouter une tâche à un package, vous avez besoin d'un conteneur avec une collection <xref:Microsoft.SqlServer.Dts.Runtime.Executables> existante. Généralement, la tâche ajoutée à la collection est un package. Pour ajouter la nouvelle tâche exécutable dans la collection pour ce conteneur, vous devez appeler la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. La méthode possède un seul paramètre, une chaîne, qui contient le CLSID, le PROGID, le moniker STOCK, ou le <xref:Microsoft.SqlServer.Dts.Runtime.TaskInfo.CreationName%2A> de la tâche que vous ajoutez.  
  
## <a name="task-names"></a>Nom des tâches  
 Bien qu’il soit possible de spécifier une tâche par son nom ou son ID, le moniker **STOCK** est le paramètre le plus souvent utilisé dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. Pour ajouter une tâche à un fichier exécutable identifié par le moniker **STOCK**, utilisez la syntaxe suivante :  
  
```csharp  
Executable exec = package.Executables.Add("STOCK:BulkInsertTask");  
  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add("STOCK:BulkInsertTask")  
  
```  
  
 La liste suivante présente les noms de chaque tâche figurant à la suite du moniker **STOCK**.  
  
-   ActiveXScriptTask  
  
-   BulkInsertTask  
  
-   ExecuteProcessTask  
  
-   ExecutePackageTask  
  
-   Exec80PackageTask  
  
-   FileSystemTask  
  
-   FTPTask  
  
-   MSMQTask  
  
-   PipelineTask  
  
-   ScriptTask  
  
-   SendMailTask  
  
-   SQLTask  
  
-   TransferStoredProceduresTask  
  
-   TransferLoginsTask  
  
-   TransferErrorMessagesTask  
  
-   TransferJobsTask  
  
-   TransferObjectsTask  
  
-   TransferDatabaseTask  
  
-   WebServiceTask  
  
-   WmiDataReaderTask  
  
-   WmiEventWatcherTask  
  
-   XMLTask  
  
 Si vous préférez une syntaxe plus explicite, ou si la tâche à ajouter n'a pas de moniker STOCK, vous pouvez ajouter la tâche au fichier exécutable à l'aide de son nom long. Cette syntaxe requiert que vous spécifiiez également le numéro de version de la tâche.  
  
```csharp  
Executable exec = package.Executables.Add(  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " +  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " +  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91");  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add( _  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " & _  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " & _  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91")  
```  
  
 Vous pouvez obtenir le nom long de la tâche par programmation, sans devoir spécifier la version de la tâche, à l’aide de la propriété **AssemblyQualifiedName** de la classe, comme indiqué dans l’exemple suivant. Cet exemple requiert une référence à l'assembly Microsoft.SqlServer.SQLTask.  
  
```csharp  
using Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask;  
...  
      Executable exec = package.Executables.Add(  
        typeof(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName);  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask  
...  
    Dim exec As Executable = package.Executables.Add( _  
      GetType(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName)  
```  
  
 L’exemple de code suivant montre comment créer une collection <xref:Microsoft.SqlServer.Dts.Runtime.Executables> à partir d’un nouveau package, puis ajouter une tâche de système de fichiers et une tâche d’insertion en bloc à la collection, à l’aide de leur moniker **STOCK**. Cet exemple requiert une référence aux assemblys Microsoft.SqlServer.FileSystemTask et Microsoft.SqlServer.BulkInsertTask.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thBulkInsertTask = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        Console.WriteLine("Type {0}", taskHost.InnerObject.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thBulkInsertTask As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      Console.WriteLine("Type {0}", taskHost.InnerObject.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Exemple de sortie :**  
  
 `Type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
## <a name="taskhost-container"></a>Conteneur TaskHost  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> est un conteneur très important pour la programmation, même s'il n'apparaît pas dans l'interface utilisateur graphique. Cette classe sert de wrapper pour chaque tâche. Les tâches qui, à l'aide de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>, sont ajoutées au package en tant qu'objet <xref:Microsoft.SqlServer.Dts.Runtime.Executable> peuvent être converties en objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Lorsqu'une tâche est convertie en <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, vous pouvez lui appliquer des propriétés et des méthodes supplémentaires. Par ailleurs, la tâche elle-même est accessible par l'intermédiaire de la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Selon vos besoins, vous pouvez décider de conserver la tâche sous la forme d'un objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> afin de pouvoir vous servir des propriétés de la tâche par le biais de la collection <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>. <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> a pour avantage de vous permettre d'écrire plus de code générique. Si vous avez besoin d'utiliser du code très spécifique pour une tâche, vous devez effectuer un cast de la tâche en son objet approprié.  
  
 L'exemple de code suivant indique comment effectuer un cast de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, thBulkInsertTask, qui contient <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>, en objet <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>.  
  
```csharp  
BulkInsertTask myTask = thBulkInsertTask.InnerObject as BulkInsertTask;  
```  
  
```vb  
Dim myTask As BulkInsertTask = CType(thBulkInsertTask.InnerObject, BulkInsertTask)  
```  
  
 L'exemple de code suivant montre comment effectuer un cast du fichier exécutable en <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, puis comment utiliser la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> pour identifier le type de fichier exécutable que contient l'hôte.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask1 = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thFileSystemTask2 = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask)  
        {  
          // Do work with FileSystemTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        else if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask)  
        {  
          // Do work with BulkInsertTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        // Add additional statements to check InnerObject, if desired.  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask1 As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thFileSystemTask2 As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      If TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask Then  
        ' Do work with FileSystemTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      ElseIf TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask Then  
        ' Do work with BulkInsertTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      End If  
      ' Add additional statements to check InnerObject, if desired.  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Exemple de sortie :**  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
 L'instruction <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A> retourne un fichier exécutable converti en objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> à partir de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.Executable> créé récemment.  
  
 Pour définir des propriétés ou appeler des méthodes sur le nouvel objet, vous disposez de deux options :  
  
1.  Utilisez la collection <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Par exemple, pour obtenir une propriété à partir de l’objet, utilisez `th.Properties["propertyname"].GetValue(th))`. Pour définir une propriété, utilisez `th.Properties["propertyname"].SetValue(th, <value>);`.  
  
2.  Effectuez un cast de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> vers la classe de tâche. Par exemple, pour effectuer un cast de la tâche d'insertion en bloc en <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask> une fois qu'elle a été ajoutée à un package en tant que <xref:Microsoft.SqlServer.Dts.Runtime.Executable>, puis convertie en <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, utilisez `BulkInsertTask myTask = th.InnerObject as BulkInsertTask;`.  
  
 Utiliser la classe <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> dans le code, au lieu d'effectuer un cast vers la classe spécifique à une tâche présente les avantages suivants :  
  
-   Le fournisseur <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost><xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> ne requiert pas de référence à l’assembly dans le code.  
  
-   Vous pouvez coder des routines génériques qui fonctionnent avec toutes les tâches, car vous n'avez pas besoin de connaître le nom de la tâche au moment de la compilation. Ces routines génériques incluent des méthodes dans lesquelles vous pouvez passer le nom de la tâche et dont le code fonctionne avec toutes les tâches. C'est une méthode pratique pour écrire un code de test.  
  
 Effectuer un cast à partir de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> vers la classe spécifique à une tâche présente les avantages suivants :  
  
-   Le projet Visual Studio propose la saisie semi-automatique des instructions (IntelliSense).  
  
-   Le code peut s'exécuter plus rapidement.  
  
-   Les objets spécifiques à une tâche prennent en charge la liaison anticipée et les optimisations qui en résultent. Pour plus d'informations sur la liaison anticipée et tardive, consultez la rubrique sur les liaisons anticipées et les liaisons tardives dans les concepts de langage de Visual Basic.  
  
 L'exemple de code suivant développe le concept de réutilisation du code de tâche. Au lieu d'effectuer un cast des tâches en leurs équivalents dans une classe spécifique, l'exemple de code montre comment effectuer un cast du fichier exécutable en <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, puis il utilise <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> pour écrire du code générique pour toutes les tâches.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
  
      string[] tasks = { "STOCK:SQLTask", "STOCK:ScriptTask",   
        "STOCK:ExecuteProcessTask", "STOCK:PipelineTask",   
        "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask" };  
  
      foreach (string s in tasks)  
      {  
        TaskHost taskhost = package.Executables.Add(s) as TaskHost;  
        DtsProperties props = taskhost.Properties;  
        Console.WriteLine("Enumerating properties on " + taskhost.Name);  
        Console.WriteLine(" TaskHost.InnerObject is " + taskhost.InnerObject.ToString());  
        Console.WriteLine();  
  
        foreach (DtsProperty prop in props)  
        {  
          Console.WriteLine("Properties for " + prop.Name);  
          Console.WriteLine("Name : " + prop.Name);  
          Console.WriteLine("Type : " + prop.Type.ToString());  
          Console.WriteLine("Readable : " + prop.Get.ToString());  
          Console.WriteLine("Writable : " + prop.Set.ToString());  
          Console.WriteLine();  
        }  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package = New Package()  
  
    Dim tasks() As String = New String() {"STOCK:SQLTask", "STOCK:ScriptTask", _  
              "STOCK:ExecuteProcessTask", "STOCK:PipelineTask", _  
              "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask"}  
  
    For Each s As String In tasks  
  
      Dim taskhost As TaskHost = CType(package.Executables.Add(s), TaskHost)  
      Dim props As DtsProperties = taskhost.Properties  
      Console.WriteLine("Enumerating properties on " & taskhost.Name)  
      Console.WriteLine(" TaskHost.InnerObject is " & taskhost.InnerObject.ToString())  
      Console.WriteLine()  
  
      For Each prop As DtsProperty In props  
        Console.WriteLine("Properties for " + prop.Name)  
        Console.WriteLine(" Name : " + prop.Name)  
        Console.WriteLine(" Type : " + prop.Type.ToString())  
        Console.WriteLine(" Readable : " + prop.Get.ToString())  
        Console.WriteLine(" Writable : " + prop.Set.ToString())  
        Console.WriteLine()  
      Next  
  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [EzAPI - Updated for SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), sur blogs.msdn.com.  

## <a name="see-also"></a>Voir aussi  
 [Connexion de tâches par programme](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
  
  

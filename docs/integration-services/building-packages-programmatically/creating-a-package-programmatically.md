---
description: Création d'un package par programme
title: Création d’un package par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- SSIS packages, creating
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
ms.assetid: e44bcc70-32d3-43e8-a84b-29aef819d5d3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f7d955978a089a3a9c68420658dcc27ae3f12dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355082"
---
# <a name="creating-a-package-programmatically"></a>Création d'un package par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'objet <xref:Microsoft.SqlServer.Dts.Runtime.Package> correspond au conteneur de niveau supérieur de tous les autres objets inclus dans une solution de projet [!INCLUDE[ssIS](../../includes/ssis-md.md)]. En tant que conteneur de niveau supérieur, le package est le premier objet créé, puis des objets suivants lui sont ajoutés, puis ils sont exécutés dans le contexte du package. Le package lui-même ne déplace pas et ne transforme pas de données. Il se repose sur les tâches qu'il contient pour effectuer le travail. Les tâches effectuent la plupart du travail d'un package et définissent ses fonctionnalités. Un package est créé et exécuté avec seulement trois lignes de code, mais différentes tâches et objets <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> sont ajoutés pour lui donner des fonctionnalités supplémentaires. Cette section décrit comment créer un package par programme. Elle ne fournit pas d'informations sur la manière de créer les tâches ou les objets <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>. Ceux-ci sont traités dans les sections ultérieures.  
  
## <a name="example"></a>Exemple  
 Pour écrire du code à l'aide de l'environnement de développement intégré Visual Studio, une référence à Microsoft.SqlServer.ManagedDTS.DLL est requise afin de créer une instruction `using` (`Imports` dans Visual Basic .NET) dans Microsoft.SqlServer.Dts.Runtime. L'exemple de code suivant présente la création d'un package vide.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package;  
      package = new Package();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package  
    package = New Package  
  
  End Sub  
  
End Module  
```  
  
 Pour compiler et exécuter l'exemple, appuyez sur F5 dans Visual Studio. Pour générer le code à l’aide du compilateur C#, **csc.exe**, à l’invite de commandes à compiler, utilisez la commande et les références de fichiers suivantes, en remplaçant *\<filename>* par le nom du fichier .cs ou .vb et en lui attribuant une valeur *\<outputfilename>* de votre choix.  
  
 **csc /target:library /out: \<outputfilename>.dll \<filename>.cs /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll**  
  
 Pour générer le code à l’aide du compilateur Visual Basic .NET, **vbc.exe**, à l’invite de commandes de compilation, utilisez la commande et les références de fichiers suivantes.  
  
 **vbc /target:library /out: \<outputfilename>.dll \<filename>.vb /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll**  
  
 Vous pouvez également créer un package en chargeant un package existant enregistré sur le disque, dans le système de fichiers ou dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La différence est que l’objet <xref:Microsoft.SqlServer.Dts.Runtime.Application> est d’abord créé, puis l’objet de package est renseigné par l’une des méthodes surchargées de l’application : **LoadPackage** pour les fichiers plats, **LoadFromSQLServer** pour les packages enregistrés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou <xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromDtsServer%2A> pour les packages enregistrés dans le système de fichiers. L'exemple suivant charge un package existant à partir du disque, puis consulte plusieurs propriétés sur le package.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class ApplicationTests  
  {  
    static void Main(string[] args)  
    {  
      // The variable pkg points to the location of the  
      // ExecuteProcess package sample that was installed with  
      // the SSIS samples.  
      string pkg = @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx";  
  
      Application app = new Application();  
      Package p = app.LoadPackage(pkg, null);  
  
      // Now that the package is loaded, we can query on  
      // its properties.  
      int n = p.Configurations.Count;  
      DtsProperty p2 = p.Properties["VersionGUID"];  
      DTSProtectionLevel pl = p.ProtectionLevel;  
  
      Console.WriteLine("Number of configurations = " + n.ToString());  
      Console.WriteLine("VersionGUID = " + (string)p2.GetValue(p));  
      Console.WriteLine("ProtectionLevel = " + pl.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module ApplicationTests  
  
  Sub Main()  
  
    ' The variable pkg points to the location of the  
    ' ExecuteProcess package sample that was installed with  
    ' the SSIS samples.  
    Dim pkg As String = _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx"  
  
    Dim app As Application = New Application()  
    Dim p As Package = app.LoadPackage(pkg, Nothing)  
  
    ' Now that the package is loaded, we can query on  
    ' its properties.  
    Dim n As Integer = p.Configurations.Count  
    Dim p2 As DtsProperty = p.Properties("VersionGUID")  
    Dim pl As DTSProtectionLevel = p.ProtectionLevel  
  
    Console.WriteLine("Number of configurations = " & n.ToString())  
    Console.WriteLine("VersionGUID = " & CType(p2.GetValue(p), String))  
    Console.WriteLine("ProtectionLevel = " & pl.ToString())  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Exemple de sortie :**  
  
 `Number of configurations = 2`  
  
 `VersionGUID = {09016682-89B8-4406-AAC9-AF1E527FF50F}`  
  
 `ProtectionLevel = DontSaveSensitive`  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Entrée de blog, [API Sample - OleDB source and OleDB destination](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553), sur blogs.msdn.com.  
  
-   Entrée de blog, [EzAPI - Updated for SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), sur blogs.msdn.com.  
  
## <a name="see-also"></a>Voir aussi  
 [Ajout de tâches par programmation](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
  
  

---
description: Scripts
title: Scripts | Documents Microsoft
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- dependencies [SMO]
- scripts [SMO]
ms.assetid: 13a35511-3987-426b-a3b7-3b2e83900dc7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3aff8bbdc42ca0ecd5ae7e8139ec4ec78f7b3fd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352706"
---
# <a name="scripting"></a>Scripts
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Les scripts dans SMO sont contrôlés par l' <xref:Microsoft.SqlServer.Management.Smo.Scripter> objet et ses objets enfants, ou par la méthode de **script** sur des objets individuels. L' <xref:Microsoft.SqlServer.Management.Smo.Scripter> objet contrôle le mappage à partir des relations de dépendance pour les objets sur une instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 L'écriture de scripts avancés à l'aide de l'objet <xref:Microsoft.SqlServer.Management.Smo.Scripter> et ses objets enfants est un processus en trois phases :  
  
1.  Découverte  
  
2.  Génération de la liste  
  
3.  Génération du script  

 La phase de découverte utilise l'objet <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker>. À partir d'une liste URN d'objets, la méthode <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker.DiscoverDependencies%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker> retourne un objet <xref:Microsoft.SqlServer.Management.Smo.DependencyTree> pour les objets de la liste URN. Le paramètre booléen *fParents* est utilisé pour déterminer si les parents ou les enfants de l’objet spécifié doivent être découverts. L'arborescence des dépendances peut être modifiée à ce stade.  
  
 Dans la phase de la génération de la liste, l'arborescence est transmise et la liste résultante est retournée. Cette liste d'objets suit l'ordre d'écriture du script et peut être manipulée.  
  
 Les phases de génération de la liste utilisent la méthode <xref:Microsoft.SqlServer.Management.Smo.DependencyWalker.WalkDependencies%2A> pour retourner un objet <xref:Microsoft.SqlServer.Management.Smo.DependencyTree>. L'objet <xref:Microsoft.SqlServer.Management.Smo.DependencyTree> peut être modifié à ce stade.  
  
 Au cours de la troisième et dernière phase, un script est généré à l'aide de la liste et des options spécifiées. Le résultat est retourné sous la forme d'un objet système <xref:System.Collections.Specialized.StringCollection>. Au cours de cette phase, les noms des objets dépendants sont extraits de la collection Items des objets et propriétés <xref:Microsoft.SqlServer.Management.Smo.DependencyTree>, comme <xref:Microsoft.SqlServer.Management.Smo.DependencyTree.NumberOfSiblings%2A> et <xref:Microsoft.SqlServer.Management.Smo.DependencyTree.FirstChild%2A>.  
  
## <a name="example"></a> Exemple  
 Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un projet Visual C&#35; Smo dans Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
 Cet exemple de code requiert une instruction **Imports** pour l’espace de noms System. Collections. Specialized. Insérez-la avec les autres instructions Imports, avant toute autre déclaration dans l'application.  
  
```  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Common  
Imports System.Collections.Specialized  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-visual-basic"></a>Écriture sous forme de scripts des dépendances pour une base de données en Visual Basic  
 Cet exemple de code montre comment découvrir les dépendances et parcourir la liste pour afficher les résultats.  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll   
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
  
Public Class A  
   Public Shared Sub Main()  
      ' database name  
      Dim dbName As [String] = "AdventureWorksLT2012"   ' database name  
  
      ' Connect to the local, default instance of SQL Server.   
      Dim srv As New Server()  
  
      ' Reference the database.    
      Dim db As Database = srv.Databases(dbName)  
  
      ' Define a Scripter object and set the required scripting options.   
      Dim scrp As New Scripter(srv)  
      scrp.Options.ScriptDrops = False  
      scrp.Options.WithDependencies = True  
      scrp.Options.Indexes = True   ' To include indexes  
      scrp.Options.DriAllConstraints = True   ' to include referential constraints in the script  
  
      ' Iterate through the tables in database and script each one. Display the script.  
      For Each tb As Table In db.Tables  
         ' check if the table is not a system table  
         If tb.IsSystemObject = False Then  
            Console.WriteLine("-- Scripting for table " + tb.Name)  
  
            ' Generating script for table tb  
            Dim sc As System.Collections.Specialized.StringCollection = scrp.Script(New Urn() {tb.Urn})  
            For Each st As String In sc  
               Console.WriteLine(st)  
            Next  
            Console.WriteLine("--")  
         End If  
      Next  
   End Sub  
End Class  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-visual-c"></a>Écriture sous forme de scripts des dépendances pour une base de données en Visual C#  
 Cet exemple de code montre comment découvrir les dépendances et parcourir la liste pour afficher les résultats.  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll   
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using System;  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
  
public class A {  
   public static void Main() {   
      String dbName = "AdventureWorksLT2012"; // database name  
  
      // Connect to the local, default instance of SQL Server.   
      Server srv = new Server();  
  
      // Reference the database.    
      Database db = srv.Databases[dbName];  
  
      // Define a Scripter object and set the required scripting options.   
      Scripter scrp = new Scripter(srv);  
      scrp.Options.ScriptDrops = false;  
      scrp.Options.WithDependencies = true;  
      scrp.Options.Indexes = true;   // To include indexes  
      scrp.Options.DriAllConstraints = true;   // to include referential constraints in the script  
  
      // Iterate through the tables in database and script each one. Display the script.     
      foreach (Table tb in db.Tables) {   
         // check if the table is not a system table  
         if (tb.IsSystemObject == false) {  
            Console.WriteLine("-- Scripting for table " + tb.Name);  
  
            // Generating script for table tb  
            System.Collections.Specialized.StringCollection sc = scrp.Script(new Urn[]{tb.Urn});  
            foreach (string st in sc) {  
               Console.WriteLine(st);  
            }  
            Console.WriteLine("--");  
         }  
      }   
   }  
}  
```  
  
## <a name="scripting-out-the-dependencies-for-a-database-in-powershell"></a>Écriture sous forme de scripts des dépendances pour une base de données dans PowerShell  
 Cet exemple de code montre comment découvrir les dépendances et parcourir la liste pour afficher les résultats.  
  
```  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\default  
  
# Create a Scripter object and set the required scripting options.  
$scrp = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Scripter -ArgumentList (Get-Item .)  
$scrp.Options.ScriptDrops = $false  
$scrp.Options.WithDependencies = $true  
$scrp.Options.IncludeIfNotExists = $true  
  
# Set the path context to the tables in AdventureWorks2012.  
  
CD Databases\AdventureWorks2012\Tables  
  
foreach ($Item in Get-ChildItem)  
 {    
 $scrp.Script($Item)  
 }  
```  
  
  

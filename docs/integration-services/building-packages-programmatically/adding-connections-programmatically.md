---
description: Ajout de connexions par programme
title: Ajout de connexions par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- connection managers [Integration Services], packages
- Integration Services packages, connections
- connections [Integration Services], packages
- SSIS packages, connections
- packages [Integration Services], connections
- intrinsic properties [Integration Services]
- SQL Server Integration Services packages, connections
- ConnectionManager class
- SSIS connection managers
- adding package connections
ms.assetid: d90716d1-4c65-466c-b82c-4aabbee1e3e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3d15ad56ceb2568f2071e4a4170e95afac24e886
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344925"
---
# <a name="adding-connections-programmatically"></a>Ajout de connexions par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La classe <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> représente des connexions physiques aux sources de données externes. La classe <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> isole du runtime les détails d'implémentation de la connexion. Elle permet au runtime d'interagir avec chaque gestionnaire de connexions de façon cohérente et prévisible. Les gestionnaires de connexions contiennent un jeu de propriétés stock que toutes les connexions ont en commun, telles que les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ID%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>. Toutefois, les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> sont habituellement les seules propriétés requises pour configurer un gestionnaire de connexions. Contrairement à d’autres paradigmes de programmation, où les classes de connexion exposent des méthodes telles que **Open** ou **Connect** pour établir physiquement une connexion à la source de données, le moteur d’exécution gère toutes les connexions du package pendant qu’il s’exécute.  
  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.Connections> est une collection des gestionnaires de connexions qui ont été ajoutés à ce package et qui sont disponibles au moment de l'exécution. Vous pouvez ajouter d'autres gestionnaires de connexions à la collection en utilisant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> de la collection, et en fournissant une chaîne qui indique le type de gestionnaire de connexions. La méthode <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> renvoie l'instance <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> ajoutée au package.  
  
## <a name="intrinsic-properties"></a>Propriétés intrinsèques  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> expose un jeu de propriétés communes à toutes les connexions. Toutefois, vous avez parfois besoin d'accéder aux propriétés qui sont uniques au type de connexion spécifique. La collection <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Properties%2A> de la classe <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> fournit l'accès à ces propriétés. Les propriétés peuvent être extraites de la collection à l’aide de l’indexeur ou du nom de propriété et de la méthode **GetValue**, et les valeurs sont définies à l’aide de la méthode **SetValue**. Les propriétés des propriétés de l'objet de connexion sous-jacente peuvent également être définies via l'acquisition d'une instance réelle de l'objet et la définition directe de ses propriétés. Pour obtenir la connexion sous-jacente, utilisez la propriété <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.InnerObject%2A> du gestionnaire de connexions. La ligne de code suivante présente une ligne C# qui crée un gestionnaire de connexions ADO.NET qui possède la classe sous-jacente <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.ConnectionManagerAdoNetClass>.  
  
 `ConnectionManagerAdoNetClass cmado = cm.InnerObject as ConnectionManagerAdoNet;`  
  
 Elle convertit l'objet du gestionnaire de connexions managées en son objet de connexion sous-jacente. Si vous utilisez C++, la méthode **QueryInterface** de l’objet <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> est appelée et l’interface de l’objet de connexion sous-jacent est demandée.  
  
 Le tableau suivant répertorie les gestionnaires de connexions fournis avec [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. et la chaîne utilisée dans l'instruction `package.Connections.Add("xxx")`. Pour obtenir la liste de tous les gestionnaires de connexions, consultez [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
|String|Gestionnaire de connexions|  
|------------|------------------------|  
|"OLEDB"|Gestionnaire de connexions pour les connexions OLE DB.|  
|"ODBC"|Gestionnaire de connexions pour les connexions ODBC.|  
|"ADO"|Gestionnaire de connexions pour les connexions ADO.|  
|"ADO.NET:SQL"|Gestionnaire de connexions pour les connexions ADO.NET (fournisseur de données SQL).|  
|"ADO.NET:OLEDB"|Gestionnaire de connexions pour les connexions ADO.NET (fournisseur de données OLE DB).|  
|"FLATFILE"|Gestionnaire de connexions pour les connexions de fichiers plats.|  
|"FILE"|Gestionnaire de connexions pour les connexions de fichiers.|  
|"MULTIFLATFILE"|Gestionnaire de connexions pour les connexions de fichiers plats multiples.|  
|"MULTIFILE"|Gestionnaire de connexions pour les connexions de fichiers multiples.|  
|"SQLMOBILE"|Gestionnaire de connexions pour les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.|  
|"MSOLAP100"|Gestionnaire de connexions pour les connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|"FTP"|Gestionnaire de connexions pour les connexions FTP.|  
|"HTTP"|Gestionnaire de connexions pour les connexions HTTP.|  
|"MSMQ"|Gestionnaire de connexions pour les connexions Message Queuing (également appelées MSMQ).|  
|"SMTP"|Gestionnaire de connexions pour les connexions SMTP.|  
|"WMI"|Gestionnaire de connexions pour les connexions WMI (Microsoft Windows Management Instrumentation).|  
  
 L'exemple de code suivant montre l'ajout d'une connexion OLE DB et FILE à la collection <xref:Microsoft.SqlServer.Dts.Runtime.Package.Connections%2A> d'un objet <xref:Microsoft.SqlServer.Dts.Runtime.Package>. L'exemple définit ensuite les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A>.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      // Create a package, and retrieve its connections.  
      Package pkg = new Package();  
      Connections pkgConns = pkg.Connections;  
  
      // Add an OLE DB connection to the package, using the   
      // method defined in the AddConnection class.  
      CreateConnection myOLEDBConn = new CreateConnection();  
      myOLEDBConn.CreateOLEDBConnection(pkg);  
  
      // View the new connection in the package.  
      Console.WriteLine("Connection description: {0}",  
         pkg.Connections["SSIS Connection Manager for OLE DB"].Description);  
  
      // Add a second connection to the package.  
      CreateConnection myFileConn = new CreateConnection();  
      myFileConn.CreateFileConnection(pkg);  
  
      // View the second connection in the package.  
      Console.WriteLine("Connection description: {0}",  
        pkg.Connections["SSIS Connection Manager for Files"].Description);  
  
      Console.WriteLine();  
      Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count);  
  
      Console.Read();  
    }  
  }  
  // <summary>  
  // This class contains the definitions for multiple  
  // connection managers.  
  // </summary>  
  public class CreateConnection  
  {  
    // Private data.  
    private ConnectionManager ConMgr;  
  
    // Class definition for OLE DB Provider.  
    public void CreateOLEDBConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("OLEDB");  
      ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" +  
        "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" +  
        "Data Source=(local);";  
      ConMgr.Name = "SSIS Connection Manager for OLE DB";  
      ConMgr.Description = "OLE DB connection to the AdventureWorks database.";  
    }  
    public void CreateFileConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("File");  
      ConMgr.ConnectionString = @"\\<yourserver>\<yourfolder>\books.xml";  
      ConMgr.Name = "SSIS Connection Manager for Files";  
      ConMgr.Description = "Flat File connection";  
    }  
  }  
  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    ' Create a package, and retrieve its connections.  
    Dim pkg As New Package()  
    Dim pkgConns As Connections = pkg.Connections  
  
    ' Add an OLE DB connection to the package, using the   
    ' method defined in the AddConnection class.  
    Dim myOLEDBConn As New CreateConnection()  
    myOLEDBConn.CreateOLEDBConnection(pkg)  
  
    ' View the new connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for OLE DB").Description)  
  
    ' Add a second connection to the package.  
    Dim myFileConn As New CreateConnection()  
    myFileConn.CreateFileConnection(pkg)  
  
    ' View the second connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for Files").Description)  
  
    Console.WriteLine()  
    Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
' This class contains the definitions for multiple  
' connection managers.  
  
Public Class CreateConnection  
  ' Private data.  
  Private ConMgr As ConnectionManager  
  
  ' Class definition for OLE DB provider.  
  Public Sub CreateOLEDBConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("OLEDB")  
    ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" & _  
      "Data Source=(local);"  
    ConMgr.Name = "SSIS Connection Manager for OLE DB"  
    ConMgr.Description = "OLE DB connection to the AdventureWorks database."  
  End Sub  
  
  Public Sub CreateFileConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("File")  
    ConMgr.ConnectionString = "\\<yourserver>\<yourfolder>\books.xml"  
    ConMgr.Name = "SSIS Connection Manager for Files"  
    ConMgr.Description = "Flat File connection"  
  End Sub  
  
End Class  
```  
  
 **Exemple de sortie :**  
  
 `Connection description: OLE DB connection to the AdventureWorks database.`  
  
 `Connection description: Flat File connection.`  
  
 `Number of connections in package: 2`  
  
## <a name="external-resources"></a>Ressources externes  
 Article technique, [Connection Strings](https://go.microsoft.com/fwlink/?LinkId=220743), sur carlprothman.net.  
  
## <a name="see-also"></a>Voir aussi  
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Créer des gestionnaires de connexions](../connection-manager/integration-services-ssis-connections.md)  
  

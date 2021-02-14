---
description: Implémentation de la recherche en texte intégral
title: Implémentation de la recherche en texte intégral
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- full-text search [SMO]
ms.assetid: 9ce9ad9c-f671-4760-90b5-e0c8ca051473
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 828b677210b00dae3a09d4f73b67fb73f0470edd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352502"
---
# <a name="implementing-full-text-search"></a>Implémentation de la recherche en texte intégral
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  La recherche en texte intégral est disponible par instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et est représentée dans SMO par l'objet <xref:Microsoft.SqlServer.Management.Smo.Server.FullTextService%2A>. L' <xref:Microsoft.SqlServer.Management.Smo.FullTextService> objet réside sous l’objet **serveur** . Il est utilisé pour gérer les options de configuration pour le service de recherche en texte intégral de [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. L'objet <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalogCollection> appartient à l'objet <xref:Microsoft.SqlServer.Management.Smo.Database> et c'est une collection d'objets <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> qui représentent des catalogues de texte intégral définis pour la base de données. Vous ne pouvez avoir qu'un seul index de recherche en texte intégral défini pour chaque table, contrairement aux index normaux. Il est représenté par un objet <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> dans l'objet <xref:Microsoft.SqlServer.Management.Smo.Table>.  
  
 Pour créer un service de recherche en texte intégral, vous devez avoir un catalogue de texte intégral défini sur la base de données et un index de recherche en texte intégral défini sur l'une des tables de la base de données.  
  
 En premier lieu, créez un catalogue de texte intégral sur la base de données en appelant le constructeur <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> et en spécifiant le nom de catalogue. Puis, créez l'index de recherche en texte intégral en appelant le constructeur et en spécifiant la table sur laquelle il doit être créé. Vous pouvez ajouter des colonnes d'index pour l'index de recherche en texte intégral, en utilisant l'objet <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> et en fournissant le nom de la colonne dans la table. Ensuite, définissez la propriété <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.CatalogName%2A> sur le catalogue que vous avez créé. Enfin, appelez la méthode <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.Create%2A> et créez l'index de recherche en texte intégral sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a> Exemple  
 Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un projet Visual C&#35; Smo dans Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-full-text-search-service-in-visual-basic"></a>Création d'un service de recherche en texte intégral en Visual Basic  
 Cet exemple de code crée un catalogue de recherche en texte intégral pour la table `ProductCategory` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Il crée alors un index de recherche en texte intégral sur la colonne Name dans la table `ProductCategory` . L'index de recherche en texte intégral requiert qu'un index unique soit défini au préalable sur la colonne.  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.SqlEnum.dll   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll   
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      ' Connect to the local, default instance of SQL Server.  
      Dim srv As Server = Nothing  
      srv = New Server()  
  
      ' Reference the AdventureWorks database.  
      Dim db As Database = Nothing  
      db = srv.Databases("AdventureWorks")  
  
      ' Reference the ProductCategory table.  
      Dim tb As Table = Nothing  
      tb = db.Tables("ProductCategory", "Production")  
  
      ' Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      Dim ftc As FullTextCatalog = Nothing  
      ftc = New FullTextCatalog(db, "Test_Catalog")  
      ftc.IsDefault = True  
  
      ' Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create()  
  
      ' Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      Dim fti As FullTextIndex = Nothing  
      fti = New FullTextIndex(tb)  
  
      ' Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      Dim ftic As FullTextIndexColumn = Nothing  
      ftic = New FullTextIndexColumn(fti, "Name")  
  
      ' Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic)  
      fti.ChangeTracking = ChangeTracking.Automatic  
  
      ' Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
      ' Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog"  
  
      ' Create the Full Text Search index on the instance of SQL Server.  
      fti.Create()  
   End Sub  
End Class  
```  
  
## <a name="creating-a-full-text-search-service-in-visual-c"></a>Création d'un service de recherche en texte intégral en Visual C#  
 Cet exemple de code crée un catalogue de recherche en texte intégral pour la table `ProductCategory` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Il crée alors un index de recherche en texte intégral sur la colonne Name dans la table `ProductCategory` . L'index de recherche en texte intégral requiert qu'un index unique soit défini au préalable sur la colonne.  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.SqlEnum.dll   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll   
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
using Microsoft.SqlServer.Management.Common;  
  
public class A {  
   public static void Main() {  
      // Connect to the local, default instance of SQL Server.  
      Server srv = default(Server);  
      srv = new Server();  
  
      // Reference the AdventureWorks database.  
      Database db = default(Database);  
      db = srv.Databases ["AdventureWorks"];  
  
      // Reference the ProductCategory table.  
      Table tb = default(Table);  
      tb = db.Tables["ProductCategory", "Production"];  
  
      // Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      FullTextCatalog ftc = default(FullTextCatalog);  
      ftc = new FullTextCatalog(db, "Test_Catalog");  
      ftc.IsDefault = true;  
  
      // Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create();  
  
      // Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      FullTextIndex fti = default(FullTextIndex);  
      fti = new FullTextIndex(tb);  
  
      // Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      FullTextIndexColumn ftic = default(FullTextIndexColumn);  
      ftic = new FullTextIndexColumn(fti, "Name");  
  
      // Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic);  
      fti.ChangeTracking = ChangeTracking.Automatic;  
  
      // Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name";  
  
      // Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog";  
  
      // Create the Full Text Search index on the instance of SQL Server.  
      fti.Create();  
   }  
}  
```  
  
## <a name="creating-a-full-text-search-service-in-powershell"></a>Création d'un service de recherche en texte intégral dans PowerShell  
 Cet exemple de code crée un catalogue de recherche en texte intégral pour la table `ProductCategory` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] . Il crée alors un index de recherche en texte intégral sur la colonne Name dans la table `ProductCategory` . L'index de recherche en texte intégral requiert qu'un index unique soit défini au préalable sur la colonne.  
  
```  
# Example of implementing a full text search on the default instance.  
# Set the path context to the local, default instance of SQL Server and database tables  
  
CD \sql\localhost\default\databases  
$db = get-item AdventureWorks2012  
  
CD AdventureWorks\tables  
  
#Get a reference to the table  
$tb = get-item Production.ProductCategory  
  
# Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
  
$ftc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextCatalog -argumentlist $db, "Test_Catalog2"  
$ftc.IsDefault = $true  
  
# Create the Full Text Search catalog on the instance of SQL Server.  
$ftc.Create()  
  
# Define a FullTextIndex object variable by supplying the parent table argument in the constructor.  
$fti = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndex -argumentlist $tb  
  
#  Define a FullTextIndexColumn object variable by supplying the parent index   
#  and column name arguments in the constructor.  
  
$ftic = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndexColumn -argumentlist $fti, "Name"  
  
# Add the indexed column to the index.  
$fti.IndexedColumns.Add($ftic)  
  
# Set change tracking  
$fti.ChangeTracking = [Microsoft.SqlServer.Management.SMO.ChangeTracking]::Automatic  
  
# Specify the unique index on the table that is required by the Full Text Search index.  
$fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
# Specify the catalog associated with the index.  
$fti.CatalogName = "Test_Catalog2"  
  
# Create the Full Text Search Index  
$fti.Create()  
```  
  
  

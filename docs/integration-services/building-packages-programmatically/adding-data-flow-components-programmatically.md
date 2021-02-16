---
description: Ajout de composants de flux de données par programme
title: Ajout de composants de flux de données par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow task [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: c06065cf-43e5-4b6b-9824-7309d7f5e35e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd3a122993913d524547d6c9e57945956f1099de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351003"
---
# <a name="adding-data-flow-components-programmatically"></a>Ajout de composants de flux de données par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Lorsque vous générez un flux de données, vous commencez par ajouter des composants. Puis vous configurez ces composants et vous les connectez ensemble pour établir le flux de données au moment de l'exécution. Cette section décrit l'ajout d'un composant à la tâche de flux de données, lequel permet de créer l'instance du moment de la conception du composant, puis de configurer ce composant. Pour plus d’informations sur la connexion de composants, consultez [Connexion de composants de flux de données par programmation](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
## <a name="adding-a-component"></a>Ajout d'un composant  
 Appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaDataCollection100.New%2A> de la collection <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.ComponentMetaDataCollection%2A> pour créer un composant et l'ajouter à la tâche de flux de données. Cette méthode renvoie l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> du composant. Toutefois, à ce stade, l'objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> ne contient pas d'informations spécifiques à un composant. Définissez la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> pour identifier le type de composant. La tâche de flux de données utilise la valeur de cette propriété pour créer une instance du composant au moment de l'exécution.  
  
 La valeur spécifiée dans la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> peut être le CLSID, le PROGID ou la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A> du composant. Le CLSID s'affiche normalement dans la fenêtre Propriétés en tant que valeur de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> du composant. Pour plus d’informations sur l’obtention de cette propriété et d’autres propriétés des composants disponibles, consultez [Découverte des composants de flux de données par programmation](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md).  
  
## <a name="adding-a-managed-component"></a>Ajout d'un composant managé  
 Vous ne pouvez pas utiliser le CLSID ou le PROGID pour ajouter l'un des composants de flux de données managées au flux de données, parce que ces valeurs pointent vers un wrapper et non vers le composant lui-même. Vous pouvez utiliser plutôt la propriété **CreationName** ou **AssemblyQualifiedName** comme indiqué dans l’exemple suivant.  
  
 Si vous envisagez d’utiliser la propriété **AssemblyQualifiedName**, vous devez ajouter à votre projet [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] une référence à l’assembly qui contient le composant managé. Ces assemblys ne sont pas répertoriés sous l’onglet .NET de la boîte de dialogue **Ajouter une référence**. Vous devez généralement rechercher l’assembly dans le dossier **C:\Program Files\Microsoft SQL Server\100\DTS\PipelineComponents**.  
  
 Les composants de flux de données managées intégrés incluent :  
  
-   Source [!INCLUDE[vstecado](../../includes/vstecado-md.md)]  
  
-   Source XML  
  
-   destination DataReader  
  
-   Destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact  
  
-   Composant Script  
  
 L'exemple de code suivant présente les deux façons d'ajouter un composant managé au flux de données :  
  
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
      Microsoft.SqlServer.Dts.Runtime.Package package = new Microsoft.SqlServer.Dts.Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Microsoft.SqlServer.Dts.Runtime.TaskHost thMainPipe = (Microsoft.SqlServer.Dts.Runtime.TaskHost)e;  
      MainPipe dataFlowTask = (MainPipe)thMainPipe.InnerObject;  
  
      // The Application object will be used to obtain the CreationName  
      //  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
      Application app = new Application();  
  
      // Add a first ADO NET source to the data flow.  
      //  The CreationName property requires an Application instance.  
      IDTSComponentMetaData100 component1 = dataFlowTask.ComponentMetaDataCollection.New();  
      component1.Name = "DataReader Source";  
      component1.ComponentClassID = app.PipelineComponentInfos["DataReader Source"].CreationName;  
  
      // Add a second ADO NET source to the data flow.  
      //  The AssemblyQualifiedName property requires a reference to the assembly.  
      IDTSComponentMetaData100 component2 = dataFlowTask.ComponentMetaDataCollection.New();  
      component2.Name = "DataReader Source";  
      component2.ComponentClassID = typeof(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName;  
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
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' The Application object will be used to obtain the CreationName  
    '  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
    Dim app As New Application()  
  
    ' Add a first ADO NET source to the data flow.  
    '  The CreationName property requires an Application instance.  
    Dim component1 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component1.Name = "DataReader Source"  
    component1.ComponentClassID = app.PipelineComponentInfos("DataReader Source").CreationName  
  
    ' Add a second ADO NET source to the data flow.  
    '  The AssemblyQualifiedName property requires a reference to the assembly.  
    Dim component2 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component2.Name = "DataReader Source"  
    component2.ComponentClassID = _  
      GetType(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName  
  
  End Sub  
  
End Module  
```  
  
## <a name="creating-the-design-time-instance-of-the-component"></a>Création de l'instance de conception du composant  
 Appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A> pour créer l'instance de conception du composant identifié par la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>. Cette méthode renvoie l'objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper>, qui est le wrapper managé de l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>.  
  
 Dès que possible, vous devez modifier un composant en utilisant les méthodes de l'instance de conception plutôt qu'en modifiant les métadonnées du composant directement. Bien qu'il existe des éléments à définir directement dans les métadonnées, tels que les connexions, il est généralement déconseillé de modifier les métadonnées directement parce que vous passez outre la capacité du composant à surveiller et valider les modifications.  
  
## <a name="assigning-connections"></a>Assignation de connexions  
 Certains composants, tels que le composant source OLE DB, requièrent une connexion à des données externes et utilisent un objet <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> existant dans le package dans ce but. La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnectionCollection100.Count%2A> de la collection <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> indique le nombre d'objets <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> requis au moment de l'exécution par le composant. Si le nombre est supérieur à zéro, le composant requiert une connexion. Assignez un gestionnaire de connexions depuis le package vers le composant en spécifiant les propriétés <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.ConnectionManager%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.Name%2A> de la première connexion dans la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>. Notez que le nom du gestionnaire de connexions figurant dans la collection de connexions au moment de l’exécution doit correspondre à celui du gestionnaire de connexions référencé depuis le package.  
  
## <a name="setting-the-values-of-custom-properties"></a>Définition des valeurs des propriétés personnalisées  
 Après avoir créé l'instance de conception du composant, appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>. Cette méthode est similaire à un constructeur parce qu'elle initialise un composant nouvellement créé en créant ses propriétés personnalisées et ses objets d'entrée et de sortie. N'appelez pas la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A> plusieurs fois sur un composant, parce que le composant risque de se réinitialiser et de perdre toutes les modifications précédemment apportées à ses métadonnées.  
  
 La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.CustomPropertyCollection%2A> du composant contient une collection d'objets <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> propre au composant. Contrairement à d'autres modèles de programmation, où les propriétés d'un objet sont toujours visibles sur l'objet, les composants renseignent uniquement leurs collections de propriétés personnalisées lorsque vous appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>. Après avoir appelé la méthode, utilisez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.SetComponentProperty%2A> de l'instance de conception du composant pour assigner des valeurs à ses propriétés personnalisées. Cette méthode accepte une paire nom/valeur qui identifie la propriété personnalisée et fournit sa nouvelle valeur.  
  
## <a name="initializing-output-columns"></a>Initialisation de colonnes de sortie  
 Après avoir ajouté un composant à la tâche et l'avoir configuré, initialisez la collection de colonnes dans l'objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> de l'objet. Cette étape s'avère particulièrement utile pour les composants source, mais risque de ne pas initialiser les colonnes pour les composants de transformation et de destination parce qu'ils dépendent généralement des colonnes qu'ils reçoivent des composants en amont.  
  
 Appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A> pour initialiser les colonnes dans les sorties d'un composant source. Étant donné que les composants ne se connectent pas automatiquement aux sources de données externes, appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.AcquireConnections%2A> avant d'appeler la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A> pour fournir au composant un accès à sa source de données externe et la capacité de renseigner ses métadonnées de colonne. Enfin, appelez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReleaseConnections%2A> pour libérer la connexion.  
  
## <a name="next-step"></a>étape suivante  
 Après avoir ajouté et configuré le composant, l’étape suivante consiste à créer des chemins entre les composants, ce que décrit la rubrique [Création d’un chemin entre deux composants](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
## <a name="sample"></a>Exemple  
 L'exemple de code suivant ajoute le composant source OLE DB à une tâche de flux de données, crée une instance de conception du composant et configure les propriétés du composant. Cet exemple requiert une référence supplémentaire à l'assembly Microsoft.SqlServer.DTSRuntimeWrap.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Runtime.Package package = new Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Runtime.TaskHost thMainPipe = e as Runtime.TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Add an OLEDB connection manager to the package.  
      ConnectionManager cm = package.Connections.Add("OLEDB");  
      cm.Name = "OLEDB ConnectionManager";  
      cm.ConnectionString = "Data Source=(local);" +   
        "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" +   
        "Integrated Security=SSPI;"  
  
      // Add an OLE DB source to the data flow.  
      IDTSComponentMetaData100 component =   
        dataFlowTask.ComponentMetaDataCollection.New();  
      component.Name = "OLEDBSource";  
      component.ComponentClassID = "DTSAdapter.OleDbSource.1";  
      // You can also use the CLSID of the component instead of the PROGID.  
      //component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
      // Get the design time instance of the component.  
      CManagedComponentWrapper instance = component.Instantiate();  
  
      // Initialize the component  
      instance.ProvideComponentProperties();  
  
      // Specify the connection manager.  
      if (component.RuntimeConnectionCollection.Count > 0)  
      {  
        component.RuntimeConnectionCollection[0].ConnectionManager =   
          DtsConvert.GetExtendedInterface(package.Connections[0]);  
        component.RuntimeConnectionCollection[0].ConnectionManagerID =   
          package.Connections[0].ID;      }  
  
      // Set the custom properties.  
      instance.SetComponentProperty("AccessMode", 2);  
      instance.SetComponentProperty("SqlCommand",   
        "Select * from Production.Product");  
  
      // Reinitialize the metadata.  
      instance.AcquireConnections(null);  
      instance.ReinitializeMetaData();  
      instance.ReleaseConnections();  
  
      // Add other components to the data flow and connect them.  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
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
  
    ' Add an OLEDB connection manager to the package.  
    Dim cm As ConnectionManager = package.Connections.Add("OLEDB")  
    cm.Name = "OLEDB ConnectionManager"  
    cm.ConnectionString = "Data Source=(local);" & _  
      "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;"  
  
    ' Add an OLE DB source to the data flow.  
    Dim component As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component.Name = "OLEDBSource"  
    component.ComponentClassID = "DTSAdapter.OleDbSource.1"  
    ' You can also use the CLSID of the component instead of the PROGID.  
    'component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
    ' Get the design time instance of the component.  
    Dim instance As CManagedComponentWrapper = component.Instantiate()  
  
    ' Initialize the component.  
    instance.ProvideComponentProperties()  
  
    ' Specify the connection manager.  
    If component.RuntimeConnectionCollection.Count > 0 Then  
      component.RuntimeConnectionCollection(0).ConnectionManager = _  
        DtsConvert.GetExtendedInterface(package.Connections(0))  
      component.RuntimeConnectionCollection(0).ConnectionManagerID = _  
        package.Connections(0).ID  
    End If  
  
    ' Set the custom properties.  
    instance.SetComponentProperty("AccessMode", 2)  
    instance.SetComponentProperty("SqlCommand", _  
      "Select * from Production.Product")  
  
    ' Reinitialize the metadata.  
    instance.AcquireConnections(vbNull)  
    instance.ReinitializeMetaData()  
    instance.ReleaseConnections()  
  
    ' Add other components to the data flow and connect them.  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [EzAPI - Updated for SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), sur blogs.msdn.com.  

## <a name="see-also"></a>Voir aussi  
 [Connexion de composants de flux de données par programme](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
  
  

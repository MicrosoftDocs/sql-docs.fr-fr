---
description: Développement d'une interface utilisateur pour une tâche personnalisée
title: Développement d’une interface utilisateur pour une tâche personnalisée | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom user interfaces [Integration Services]
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- custom tasks [Integration Services], user interface
- custom user interface [Integration Services], custom tasks
- user interface [Integration Services]
- SSIS custom tasks, user interface
ms.assetid: 1e940cd1-c5f8-4527-b678-e89ba5dc398a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f0536913b55a3e798b7306f5bd2fa36d115e5f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337695"
---
# <a name="developing-a-user-interface-for-a-custom-task"></a>Développement d'une interface utilisateur pour une tâche personnalisée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Le modèle objet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] fournit aux développeurs de tâches personnalisées la capacité de créer facilement une interface utilisateur personnalisée pour une tâche, qui peut ensuite être intégrée et affichée dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. L'interface utilisateur fournit des informations utiles dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] et aide les utilisateurs à configurer correctement les propriétés et les paramètres de la tâche personnalisée.  
  
 Le développement d'une interface utilisateur personnalisée pour une tâche implique l'utilisation de deux classes importantes. Le tableau suivant décrit ces classes.  
  
|Classe|Description|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|Attribut qui identifie une tâche managée et fournit des informations au moment de la conception par le biais de ses propriétés pour contrôler la manière dont le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] affiche les objets et interagit avec eux.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|Interface utilisée par la tâche pour s'associer à son interface utilisateur personnalisée.|  
  
 Cette section décrit le rôle de l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> et de l'interface <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> lorsque vous développez une interface utilisateur pour une tâche personnalisée. Elle fournit également des informations sur la manière de créer, intégrer, déployer et déboguer la tâche dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 Le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] fournit plusieurs points d’entrée dans l’interface utilisateur pour la tâche : l’utilisateur peut sélectionner **Modifier** dans le menu contextuel, double-cliquer sur la tâche ou cliquer sur le lien **Afficher l’éditeur** en bas de la feuille de propriétés. Lorsque l'utilisateur accède à l'un de ces points d'entrée, le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] recherche et charge l'assembly qui contient l'interface utilisateur pour la tâche. L'interface utilisateur de la tâche est chargée de créer la boîte de dialogue des propriétés affichée pour l'utilisateur dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
 Une tâche et son interface utilisateur sont des entités distinctes. Elles doivent être implémentées dans des assemblys séparés pour réduire les tâches de localisation, de déploiement et de maintenance. La DLL n'a généralement pas connaissance de son interface utilisateur et ne charge ni n'appelle de données la concernant, à l'exception des informations contenues dans les valeurs d'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> codées dans la tâche. Il s'agit du seul lien qui unit une tâche et son interface utilisateur.  
  
## <a name="the-dtstask-attribute"></a>Attribut DtsTask  
 L'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> est inclus dans le code de classe de tâche pour associer une tâche à son interface utilisateur. Le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilise les propriétés de l'attribut pour déterminer comment afficher la tâche. Ces propriétés incluent le nom à afficher et l'icône, le cas échéant.  
  
 Le tableau suivant décrit les propriétés de l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>.  
  
|Property|Description|  
|--------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>|Affiche le nom de la tâche dans la boîte à outils Flux de contrôle.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A>|Description de la tâche (héritée de <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute>). Cette propriété est affichée dans des info-bulles.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>|Icône affichée dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)].|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.RequiredProductLevel%2A>|Si vous utilisez cette propriété, attribuez-lui l'une des valeurs de l'énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSProductLevel>. Par exemple, `RequiredProductLevel = DTSProductLevel.None`.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskContact%2A>|Contient des informations de contact au cas où la tâche nécessite un support technique.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskType%2A>|Assigne un type à la tâche.|  
|Attribute.TypeId|Dans le cadre d'une implémentation dans une classe dérivée, obtient un identificateur unique pour cet attribut. Pour plus d’informations, consultez la propriété **Attribute.TypeID** dans la bibliothèque de classes .NET Framework.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>|Nom de type de l'assembly utilisé par le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] pour charger l'assembly. Cette propriété permet de rechercher l'assembly de l'interface utilisateur pour la tâche.|  
  
 L'exemple de code suivant présente le <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>, au-dessus de la définition de classe.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
 Le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilise la propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> de l'attribut qui inclut le nom, le nom de type, la version, la culture et le jeton de clé publique de l'assembly, pour rechercher l'assembly dans le Global Assembly Cache (GAC) et le charger à l'usage du concepteur.  
  
 Une fois que l'assembly a été trouvé, le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilise les autres propriétés de l'attribut pour afficher des informations supplémentaires sur la tâche dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)], telles que le nom, l'icône et la description de la tâche.  
  
 Les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> spécifient la manière dont la tâche est présentée à l'utilisateur. La propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> contient l'ID de ressource de l'icône incorporé dans l'assembly de l'interface utilisateur. Le concepteur charge la ressource icône par ID à partir de l'assembly et l'affiche en regard du nom de la tâche dans la boîte à outils et dans l'aire du concepteur lorsque la tâche est ajoutée à un package. Si une tâche ne fournit pas de ressource icône, le concepteur utilise une icône par défaut pour la tâche.  
  
## <a name="the-idtstaskui-interface"></a>Interface IDTSTaskUI  
 L'interface <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> définit la collection de méthodes et de propriétés appelées par le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] pour initialiser et afficher l'interface utilisateur associée à la tâche. Lorsque l'interface utilisateur d'une tâche est appelée, le concepteur appelle la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.Initialize%2A>, implémentée par l'interface utilisateur de la tâche lorsque vous l'avez écrite, puis fournit les collections <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> et <xref:Microsoft.SqlServer.Dts.Runtime.Connections> de la tâche et du package, respectivement, en tant que paramètres. Ces collections sont stockées localement et utilisées par la suite dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>.  
  
 Le concepteur appelle la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> pour demander la fenêtre affichée dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. La tâche crée une instance de la fenêtre qui contient l'interface utilisateur de la tâche et retourne l'interface utilisateur au concepteur afin qu'elle soit affichée. En général, les objets <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> et <xref:Microsoft.SqlServer.Dts.Runtime.Connections> sont fournis à la fenêtre par le biais d'un constructeur surchargé afin qu'ils puissent être utilisés pour configurer la tâche.  
  
 Pour afficher l'interface utilisateur de la tâche, le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] appelle la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> associée. L'interface utilisateur de la tâche retourne le Windows Form de cette méthode et le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] affiche ce formulaire sous le forme d'une boîte de dialogue modale. Lorsque le formulaire est fermé, le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] examine la valeur de la propriété **DialogResult** du formulaire pour déterminer si la tâche a été modifiée et si ces modifications doivent être enregistrées. Si la propriété **DialogResult** a la valeur **OK**, le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] appelle les méthodes de persistance de la tâche pour enregistrer les modifications ; sinon, les modifications sont supprimées.  
  
 L'exemple de code suivant implémente l'interface <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> et suppose l'existence d'une classe Windows Form nommée SampleTaskForm.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Design;  
  
namespace Sample  
{  
   public class HelloWorldTaskUI : IDtsTaskUI  
   {  
      TaskHost   taskHost;  
      Connections connections;  
      public void Initialize(TaskHost taskHost, IServiceProvider serviceProvider)  
      {  
         this.taskHost = taskHost;  
         IDtsConnectionService cs = serviceProvider.GetService  
         ( typeof( IDtsConnectionService ) ) as   IDtsConnectionService;   
         this.connections = cs.GetConnections();  
      }  
      public ContainerControl GetView()  
      {  
        return new HelloWorldTaskForm(this.taskHost, this.connections);  
      }  
     public void Delete(IWin32Window parentWindow)  
     {  
     }  
     public void New(IWin32Window parentWindow)  
     {  
     }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Design  
Imports System.Windows.Forms  
  
Public Class HelloWorldTaskUI  
  Implements IDtsTaskUI  
  
  Dim taskHost As TaskHost  
  Dim connections As Connections  
  
  Public Sub Initialize(ByVal taskHost As TaskHost, ByVal serviceProvider As IServiceProvider) _  
    Implements IDtsTaskUI.Initialize  
  
    Dim cs As IDtsConnectionService  
  
    Me.taskHost = taskHost  
    cs = DirectCast(serviceProvider.GetService(GetType(IDtsConnectionService)), IDtsConnectionService)  
    Me.connections = cs.GetConnections()  
  
  End Sub  
  
  Public Function GetView() As ContainerControl _  
    Implements IDtsTaskUI.GetView  
  
    Return New HelloWorldTaskForm(Me.taskHost, Me.connections)  
  
  End Function  
  
  Public Sub Delete(ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.Delete  
  
  End Sub  
  
  Public Sub [New](ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.[New]  
  
  End Sub  
  
End Class  
```  
 
## <a name="see-also"></a>Voir aussi  
 [Création d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Codage d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Développement d’une interface utilisateur pour une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  

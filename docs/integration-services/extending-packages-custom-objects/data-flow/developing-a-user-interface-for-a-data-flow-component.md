---
description: Développement d'une interface utilisateur pour un composant de flux de données
title: Développement d’une interface utilisateur pour un composant de flux de données | Microsoft Docs
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
- data flow components [Integration Services], custom editors
- user interfaces [Integration Services]
- custom data flow components [Integration Services], custom editors
- custom component editors [Integration Services]
- IDtsComponentUI interface
- UITypeName property
- custom user interface [Integration Services], custom data flow component
- editors [Integration Services]
ms.assetid: 10b829a1-609b-42e3-9070-cfe5a2bb698c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfe2ebf4e63237f9e5039188c5f9fec883fd797e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343707"
---
# <a name="developing-a-user-interface-for-a-data-flow-component"></a>Développement d'une interface utilisateur pour un composant de flux de données

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Les développeurs de composants peuvent fournir une interface utilisateur personnalisée pour un composant. Cette interface s'affiche dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] lors de la modification du composant. Grâce à l'implémentation d'une interface utilisateur personnalisée, vous êtes averti lorsque le composant est ajouté ou supprimé d'une tâche de flux de données, et lorsque de l'aide est demandée pour le composant.  
  
 Si vous ne fournissez pas d'interface utilisateur personnalisée pour votre composant, les utilisateurs peuvent encore configurer le composant et ses propriétés personnalisées à l'aide de l'éditeur avancé. Vous pouvez vous assurer que l'éditeur avancé permet aux utilisateurs de modifier convenablement des valeurs de propriété personnalisées à l'aide des propriétés <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> le cas échéant. Pour plus d’informations, consultez « Création de propriétés personnalisées » dans [Méthodes de conception d’un composant de flux de données](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md).  
  
## <a name="setting-the-uitypename-property"></a>Définition de la propriété UITypeName  
 Pour fournir une interface utilisateur personnalisée, le développeur doit attribuer à la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> le nom d'une classe qui implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. Lorsque cette propriété est définie par le composant, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] charge et appelle l’interface utilisateur personnalisée lorsque le composant est modifié dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> est une chaîne délimitée par des virgules qui identifie le nom qualifié complet du type. La liste suivante affiche, dans l'ordre, les éléments qui identifient le type :  
  
-   Nom de type  
  
-   Nom de l'assembly  
  
-   Version de fichier  
  
-   Culture  
  
-   Jeton de clé publique  
  
 L'exemple de code suivant montre une classe dérivée de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> et spécifie la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>.  
  
```csharp  
[DtsPipelineComponent(  
DisplayName="SampleComponent",  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...",  
ComponentType = ComponentType.Transform)]  
public class SampleComponent : PipelineComponent  
{  
//TODO: Implement the component here.  
}  
```  
  
```vb  
<DtsPipelineComponent(DisplayName="SampleComponent", _  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...", ComponentType=ComponentType.Transform)> _   
Public Class SampleComponent   
 Inherits PipelineComponent   
End Class  
```  
  
## <a name="implementing-the-idtscomponentui-interface"></a>Implémentation de l'interface IDtsComponentUI  
 L'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> contient des méthodes que le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] appelle lorsqu'un composant est ajouté, supprimé ou modifié. Les développeurs de composants peuvent fournir du code dans leur implémentation de ces méthodes pour interagir avec les utilisateurs des composants.  
  
 Cette classe est généralement implémentée dans un assembly distinct du composant lui-même. Bien qu'elle ne soit pas obligatoire, l'utilisation d'un assembly séparé permet au développeur de générer et déployer le composant et l'interface utilisateur indépendamment l'un de l'autre et de limiter l'encombrement binaire du composant.  
  
 L'implémentation d'une interface utilisateur personnalisée permet au développeur de composants de mieux contrôler le composant lorsqu'il est modifié dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Par exemple, un composant peut ajouter du code à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A>, appelée lorsqu'un composant est initialement ajouté à une tâche de flux de données, et afficher un Assistant qui guide l'utilisateur à travers la configuration initiale du composant.  
  
 Après avoir créé une classe qui implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>, vous devez ajouter du code pour répondre à l'interaction de l'utilisateur avec le composant. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> fournit l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> du composant et est appelée avant les méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>. Cette référence doit être stockée dans une variable membre privée et utilisée ensuite pour modifier les métadonnées du composant.  
  
## <a name="modifying-a-component-and-persisting-changes"></a>Modification d'un composant et persistance des modifications  
 L'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> est fournie en tant que paramètre à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>. Cette référence doit être mise en cache dans une variable membre par le code d'interface utilisateur, puis utilisée pour modifier le composant en réponse à l'interaction de l'utilisateur avec l'interface utilisateur.  
  
 Bien que vous puissiez modifier le composant directement à partir de l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, il est préférable de créer une instance de <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> à l'aide de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A>. Lorsque vous modifiez directement le composant à l'aide de l'interface, vous contournez les dispositifs de protection de la validation du composant. L'utilisation de l'instance du composant au moment de la conception par le biais de <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> présente l'avantage de garantir que le composant peut contrôler les modifications qui lui sont appliquées.  
  
 La valeur de retour de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> détermine si les modifications apportées à un composant sont conservées ou supprimées. Lorsque cette méthode retourne **false**, toutes les modifications sont ignorées ; **true** conserve les modifications apportées au composant et marque le package comme devant être enregistré.  
  
### <a name="using-the-services-of-the-ssis-designer"></a>Utilisation des services du concepteur SSIS  
 Le paramètre **IServiceProvider** de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> fournit l’accès aux services suivants du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] :  
  
|Service|Description|  
|-------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsClipboardService>|Permet de déterminer si le composant a été généré dans le cadre d'une opération copier/coller ou couper/coller.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionService>|Permet d'accéder aux connexions existantes ou de créer des connexions dans le package.|  
|<xref:Microsoft.SqlServer.Dts.Design.IErrorCollectionService>|Permet de capturer des événements à partir de composants de flux de données lorsque vous devez capturer l'ensemble des erreurs et avertissements déclenchés par le composant au lieu de ne recevoir que la dernière erreur ou le dernier avertissement.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsVariableService>|Permet d'accéder aux variables existantes ou de créer des variables dans le package.|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsPipelineEnvironmentService>|Permet aux composants de flux de données d'accéder à la tâche de flux de données parente et aux autres composants dans le flux de données. Cette fonctionnalité pourrait être utilisée pour développer un composant semblable à l'Assistant Dimension à variation lente qui crée et connecte des composants de flux de données supplémentaires si nécessaire.|  
  
 Ces services fournissent aux développeurs de composants la capacité d'accéder à des objets (ou d'en créer) dans le package dans lequel le composant est chargé.  
  
## <a name="sample"></a>Exemple  
 L'exemple de code suivant montre l'intégration d'une classe d'interface utilisateur personnalisée qui implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>, et un Windows Form qui fait office d'éditeur pour un composant.  
  
### <a name="custom-user-interface-class"></a>Classe d'interface utilisateur personnalisée  
 Le code suivant montre la classe qui implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> crée l'éditeur de composant, puis affiche le formulaire. La valeur de retour du formulaire détermine si les modifications apportées au composant sont rendues persistantes.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline.Design;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public class SampleComponentUI : IDtsComponentUI  
    {  
        IDTSComponentMetaData100 md;  
        IServiceProvider sp;  
  
        public void Help(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void New(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void Delete(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Variables vars, Connections cons)  
        {  
            // Create and display the form for the user interface.  
            SampleComponentUIForm componentEditor = new SampleComponentUIForm(cons, vars, md);  
  
            DialogResult result  = componentEditor.ShowDialog(parentWindow);  
  
            if (result == DialogResult.OK)  
                return true;  
  
            return false;  
        }  
        public void Initialize(IDTSComponentMetaData100 dtsComponentMetadata, IServiceProvider serviceProvider)  
        {  
            // Store the component metadata.  
            this.md = dtsComponentMetadata;  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Windows.Forms   
Imports Microsoft.SqlServer.Dts.Runtime   
Imports Microsoft.SqlServer.Dts.Pipeline.Design   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Class SampleComponentUI   
 Implements IDtsComponentUI   
   Private md As IDTSComponentMetaData100   
   Private sp As IServiceProvider   
  
   Public Sub Help(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub New(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub Delete(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal vars As Variables, ByVal cons As Connections) As Boolean   
     ' Create and display the form for the user interface.  
     Dim componentEditor As SampleComponentUIForm = New SampleComponentUIForm(cons, vars, md)   
     Dim result As DialogResult = componentEditor.ShowDialog(parentWindow)   
     If result = DialogResult.OK Then   
       Return True   
     End If   
     Return False   
   End Function   
  
   Public Sub Initialize(ByVal dtsComponentMetadata As IDTSComponentMetaData100, ByVal serviceProvider As IServiceProvider)   
     Me.md = dtsComponentMetadata   
   End Sub   
 End Class   
  
End Namespace  
```  
  
### <a name="custom-editor"></a>Éditeur personnalisé  
 Le code suivant montre l'implémentation du Windows Form affiché lors de l'appel à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>.  
  
```csharp  
using System;  
using System.Drawing;  
using System.Collections;  
using System.ComponentModel;  
using System.Windows.Forms;  
using System.Data;  
  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public partial class SampleComponentUIForm : System.Windows.Forms.Form  
    {  
        private Connections connections;  
        private Variables variables;  
        private IDTSComponentMetaData100 metaData;  
        private CManagedComponentWrapper designTimeInstance;  
        private System.ComponentModel.IContainer components = null;  
  
        public SampleComponentUIForm( Connections cons, Variables vars, IDTSComponentMetaData100 md)  
        {  
            variables = vars;  
            connections = cons;  
            metaData = md;  
        }  
  
        private void btnOk_Click(object sender, System.EventArgs e)  
        {  
            if (designTimeInstance == null)  
                designTimeInstance = metaData.Instantiate();  
  
            designTimeInstance.SetComponentProperty( "CustomProperty", txtCustomPropertyValue.Text);  
  
            this.Close();  
        }  
  
        private void btnCancel_Click(object sender, System.EventArgs e)  
        {  
            this.Close();  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Drawing   
Imports System.Collections   
Imports System.ComponentModel   
Imports System.Windows.Forms   
Imports System.Data   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Partial Class SampleComponentUIForm   
  Inherits System.Windows.Forms.Form   
   Private connections As Connections   
   Private variables As Variables   
   Private metaData As IDTSComponentMetaData100   
   Private designTimeInstance As CManagedComponentWrapper   
   Private components As System.ComponentModel.IContainer = Nothing   
  
   Public Sub New(ByVal cons As Connections, ByVal vars As Variables, ByVal md As IDTSComponentMetaData100)   
     variables = vars   
     connections = cons   
     metaData = md   
   End Sub   
  
   Private Sub btnOk_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     If designTimeInstance Is Nothing Then   
       designTimeInstance = metaData.Instantiate   
     End If   
     designTimeInstance.SetComponentProperty("CustomProperty", txtCustomPropertyValue.Text)   
     Me.Close   
   End Sub   
  
   Private Sub btnCancel_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     Me.Close   
   End Sub   
 End Class   
  
End Namespace  
```
  
## <a name="see-also"></a>Voir aussi  
 [Création d'un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/creating-a-custom-data-flow-component.md)  
  
  

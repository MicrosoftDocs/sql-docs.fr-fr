---
description: Validation d'un composant de flux de données
title: Validation d’un composant de flux de données | Microsoft Docs
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
- ReinitializeMetaData method
- Validate method
- component validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- data flow components [Integration Services], validating
- validation [Integration Services]
ms.assetid: 1a7d5925-b387-4e31-af7f-c7f3c5151040
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fc86f379cdbd324c47adcbdb9b610399ca2f28af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354386"
---
# <a name="validating-a-data-flow-component"></a>Validation d'un composant de flux de données

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> est fournie pour empêcher l'exécution d'un composant qui n'est pas configuré correctement. Cette méthode vous permet de vérifier qu'un composant dispose du nombre d'objets d'entrée et de sortie attendu, que les valeurs des propriétés personnalisées du composant sont acceptables et que toutes les connexions nécessaires sont spécifiées. Elle vous permet également de vérifier que les colonnes dans les collections d'entrée et de sortie contiennent des types de données corrects et que le <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType> de chaque colonne est défini de manière appropriée pour le composant. L'implémentation de la classe de base contribue au processus de validation en contrôlant la collection de colonnes d'entrée du composant et en vérifiant que chaque colonne de la collection fait référence à une colonne dans le <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputCollection100> du composant en amont.  
  
## <a name="validate-method"></a>Validate, méthode  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> est appelée de manière répétée lorsqu'un composant est modifié dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Vous pouvez fournir des informations au concepteur et aux utilisateurs du composant par le biais de la valeur de retour de l'énumération <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus>, et en publiant des avertissements et des erreurs. L'énumération <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus> contient trois valeurs correspondant à différents stades de défaillance, et une quatrième, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISVALID>, qui indique si le composant est correctement configuré et prêt à être exécuté.  
  
 La valeur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> indique qu'il existe une erreur dans <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> et que le composant peut réparer les erreurs. Si un composant rencontre une erreur de métadonnées qu'il peut réparer, il ne doit pas la corriger dans la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> et la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> ne doit pas être modifiée au cours de la validation. Au lieu de cela, la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> doit retourner <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> uniquement, et le composant doit réparer l'erreur lors d'un appel à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>, tel que décrit plus loin dans cette section.  
  
 La valeur <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISBROKEN> indique que le composant contient une erreur qui peut être corrigée en modifiant le composant dans le concepteur. L'erreur provient généralement d'une propriété personnalisée ou d'une connexion requise non spécifiée ou définie de manière incorrecte.  
  
 La valeur d'erreur finale est <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISCORRUPT>, ce qui indique que le composant a découvert des erreurs qui ne peuvent se produire que si la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> a été modifiée directement, en modifiant le package XML ou en utilisant le modèle objet. Par exemple, ce type d'erreur se produit lorsqu'un composant a ajouté une seule entrée, mais que la validation découvre que plusieurs entrées existent dans <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. Les erreurs qui génèrent cette valeur de retour ne peuvent être réparées qu’en réinitialisant le composant à l’aide du bouton **Réinitialiser** de la boîte de dialogue **Éditeur avancé**.  
  
 En plus de retourner des valeurs d'erreur, les composants fournissent des informations en publiant des avertissements ou des erreurs lors de la validation. Les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> fournissent ce mécanisme. Lorsque ces méthodes sont appelées, ces événements sont publiés dans la fenêtre **Liste d’erreurs** de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Les développeurs de composants peuvent ensuite fournir des commentaires directement aux utilisateurs sur les erreurs rencontrées et, le cas échéant, la manière de les corriger.  
  
 L'exemple de code suivant montre une implémentation substituée de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    bool pbCancel = false;  
  
    // Validate that there is one input.  
    if (ComponentMetaData.InputCollection.Count != 1)  
    {  
    ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, out pbCancel);  
    return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Validate that the UserName custom property is set.  
    if (ComponentMetaData.CustomPropertyCollection["UserName"].Value == null || ((string)ComponentMetaData.CustomPropertyCollection["UserName"].Value).Length == 0)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISBROKEN;  
    }  
  
    // Validate that there is one output.  
    if (ComponentMetaData.OutputCollection.Count != 1)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Let the base class verify that the input column reflects the output   
    // of the upstream component.  
    return base.Validate();  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
  
 Dim pbCancel As Boolean = False  
  
 ' Validate that there is one input.  
 If Not (ComponentMetaData.InputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Validate that the UserName custom property is set.  
 If ComponentMetaData.CustomPropertyCollection("UserName").Value Is Nothing OrElse CType(ComponentMetaData.CustomPropertyCollection("UserName").Value, String).Length = 0 Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISBROKEN   
 End If   
  
 ' Validate that there is one output.  
 If Not (ComponentMetaData.OutputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Let the base class verify that the input column reflects the output   
 ' of the upstream component.  
  
 Return MyBase.Validate   
  
End Function  
```  
  
## <a name="reinitializemetadata-method"></a>Méthode ReinitializeMetaData  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> est appelée par le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] à chaque fois que vous modifiez un composant qui retourne <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> à partir de sa méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>. Les composants doivent contenir du code qui détecte et corrige les problèmes identifiés par le composant au cours de la validation.  
  
 L'exemple suivant montre un composant qui détecte des problèmes au cours de la validation et les corrige dans la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
```csharp  
private bool areInputColumnsValid = true;  
public override DTSValidationStatus Validate()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
    bool Cancel = false;  
    foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
    {  
        try  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
        }  
        catch  
        {  
            ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, out Cancel);  
            areInputColumnsValid = false;  
            return DTSValidationStatus.VS_NEEDSNEWMETADATA;  
        }  
    }  
  
    return DTSValidationStatus.VS_ISVALID;  
}  
public override void ReinitializeMetaData()  
{  
    if (!areInputColumnsValid)  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection[0];  
        IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
        foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
  
            if (vColumn == null)  
                input.InputColumnCollection.RemoveObjectByID(column.ID);  
        }  
        areInputColumnsValid = true;  
    }  
}  
```  
  
```vb  
Private areInputColumnsValid As Boolean = True   
  
Public  Overrides Function Validate() As DTSValidationStatus   
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
 Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
 Dim Cancel As Boolean = False   
 For Each column As IDTSInputColumn100 In input.InputColumnCollection   
   Try   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
   Catch   
     ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, Cancel)   
     areInputColumnsValid = False   
     Return DTSValidationStatus.VS_NEEDSNEWMETADATA   
   End Try   
 Next   
 Return DTSValidationStatus.VS_ISVALID   
End Function   
  
Public  Overrides Sub ReinitializeMetaData()   
 If Not areInputColumnsValid Then   
   Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
   Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
   For Each column As IDTSInputColumn100 In input.InputColumnCollection   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
     If vColumn Is Nothing Then   
       input.InputColumnCollection.RemoveObjectByID(column.ID)   
     End If   
   Next   
   areInputColumnsValid = True   
 End If   
End Sub  
```  
  

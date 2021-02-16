---
description: Développement d'un composant de transformation personnalisé à sorties synchrones
title: Développement d’un composant de transformation personnalisé avec des sorties synchrones | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], transformation components
- ProcessInput method
- buffer allocations [Integration Services]
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- data flow components [Integration Services], transformation components
ms.assetid: b694d21f-9919-402d-9192-666c6449b0b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 87a53ff91f4ae7d6237fb3f9640420dd964b607f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348236"
---
# <a name="developing-a-custom-transformation-component-with-synchronous-outputs"></a>Développement d'un composant de transformation personnalisé à sorties synchrones

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Les composants de transformation à sorties synchrones reçoivent des lignes en provenance des composants en amont, puis lisent ou modifient les valeurs comprises dans les colonnes de ces lignes alors qu'ils transfèrent les lignes aux composants en aval. Ils peuvent également définir des colonnes de sortie supplémentaires dérivées des colonnes fournies par les composants en amont, mais ils n'ajoutent pas de lignes au flux de données. Pour plus d’informations sur la différence entre les composants synchrones et asynchrones, consultez [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Ce type de composant convient aux tâches dans lesquelles les données sont modifiées en ligne à mesure qu'elles sont fournies au composant et dans lesquelles le composant n'a pas à consulter toutes les lignes avant de les traiter. Il s'agit du composant le plus facile à développer parce que les transformations à sorties synchrones ne se connectent pas en général à des sources de données externes, gèrent des colonnes de métadonnées externes ou ajoutent des lignes aux tampons de sortie.  
  
 La création d'un composant de transformation à sorties synchrones implique d'ajouter un objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> qui contiendra des colonnes en amont sélectionnées pour le composant, et un objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> qui peut contenir des colonnes dérivées créées par le composant. Il inclut également l'implémentation des méthodes de conception, ainsi que la fourniture du code qui lit ou modifie les colonnes dans les lignes du tampon entrantes pendant l'exécution.  
  
 Cette section fournit les informations requises pour implémenter un composant de transformation personnalisé et des exemples de code pour vous aider à mieux comprendre les concepts.  
  
## <a name="design-time"></a>Moment de la conception  
 Le code du moment de la conception de ce composant implique la création des entrées et sorties, l'ajout de toutes colonnes de sortie supplémentaires que le composant génère et la validation de la configuration du composant.  
  
### <a name="creating-the-component"></a>Création du composant  
 Les entrées, sorties et propriétés personnalisées du composant sont en général créées pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>. Il existe deux manières d'ajouter l'entrée et la sortie d'un composant de transformation à sorties synchrones. Vous pouvez utiliser l'implémentation de la classe de base de la méthode, puis modifier l'entrée et la sortie par défaut qu'elle crée, ou vous pouvez ajouter explicitement l'entrée et la sortie vous-même.  
  
 L'exemple de code suivant présente une implémentation de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> qui ajoute explicitement les objets d'entrée et de sortie. L'appel de la classe de base qui accomplirait la même chose est inclus dans un commentaire.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "SynchronousComponent", ComponentType = ComponentType.Transform)]  
    public class SyncComponent : PipelineComponent  
    {  
  
        public override void ProvideComponentProperties()  
        {  
            // Add the input.  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            // Add the output.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
            output.SynchronousInputID = input.ID;  
  
            // Alternatively, you can let the base class add the input and output  
            // and set the SynchronousInputID of the output to the ID of the input.  
            // base.ProvideComponentProperties();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="SynchronousComponent", ComponentType:=ComponentType.Transform)> _  
Public Class SyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Add the input.  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
        input.Name = "Input"  
  
        ' Add the output.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
        output.SynchronousInputID = Input.ID  
  
        ' Alternatively, you can let the base class add the input and output  
        ' and set the SynchronousInputID of the output to the ID of the input.  
        ' base.ProvideComponentProperties();  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Création et configuration de colonnes de sortie  
 Bien que les composants de transformation à sorties synchrones n'ajoutent pas de lignes aux tampons, ils peuvent ajouter des colonnes de sortie supplémentaires à leur sortie. En général, lorsqu'un composant ajoute une colonne de sortie, les valeurs de la nouvelle colonne de sortie sont dérivées au moment de l'exécution des données contenues dans une ou plusieurs colonnes parmi celles fournies au composant par un composant en amont.  
  
 Après avoir créé une colonne de sortie, ses propriétés de type de données doivent être définies. La définition des propriétés de type de données d'une colonne de sortie requiert une gestion spéciale et s'effectue en appelant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>. Cette méthode est requise parce que les propriétés <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> sont individuellement en lecture seule, parce que chacune dépend des paramètres de l'autre. Cette méthode garantit que les valeurs des propriétés sont définies de manière cohérente et que la tâche de flux de données valide leur définition correcte.  
  
 La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> de la colonne détermine les valeurs définies pour les autres propriétés. Le tableau suivant indique les conditions requises sur les propriétés dépendantes de chaque propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. Les types de données non répertoriés ont leurs propriétés dépendantes définies sur zéro.  
  
|DataType|Longueur|Scale|Précision|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Supérieur à 0 et inférieur ou égal à 28.|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Supérieur à 0 et inférieur ou égale à 28 et inférieur à Précision.|Supérieur ou égal à 1 et inférieur ou égal à 38.|0|  
|DT_BYTES|Supérieur à 0.|0|0|0|  
|DT_STR|Supérieur à 0 et inférieur à 8 000.|0|0|Différent de 0 et une page de codes valide.|  
|DT_WSTR|Supérieur à 0 et inférieur à 4000.|0|0|0|  
  
 Étant donné que les restrictions sur les propriétés de type de données sont basées sur le type de données de la colonne de sortie, vous devez choisir le type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] correct lorsque vous utilisez des types managés. La classe de base fournit trois méthodes d'assistance, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A> qui aident les développeurs de composants managés à sélectionner un type de données [!INCLUDE[ssIS](../../includes/ssis-md.md)] en fonction d'un type managé. Ces méthodes convertissent des types de données managées en types de données [!INCLUDE[ssIS](../../includes/ssis-md.md)], et vice versa.  
  
## <a name="run-time"></a>Moment de l'exécution  
 En général, l’implémentation de la partie exécution du composant se divise en deux tâches : localisation des colonnes d’entrée et de sortie du composant dans le tampon et lecture ou écriture des valeurs de ces colonnes dans les lignes du tampon entrantes.  
  
### <a name="locating-columns-in-the-buffer"></a>Localisation des colonnes dans le tampon  
 Le nombre de colonnes dans les tampons fournis à un composant pendant l'exécution dépassera probablement le nombre de colonnes dans les collections d'entrée ou de sortie du composant. Ce dépassement est dû au fait que chaque tampon contient toutes les colonnes de sortie définies dans les composants d'un flux de données. Pour garantir que les colonnes de tampon sont correctement mises en correspondance avec les colonnes d'entrée ou de sortie, les développeurs de composants doivent utiliser la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Cette méthode localise une colonne dans le tampon spécifié par son ID de lignage. En général, les colonnes sont localisées pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> parce qu'il s'agit de la première méthode d'exécution où la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> devient disponible.  
  
 L'exemple de code suivant présente un composant qui localise des index des colonnes dans sa collection de colonnes d'entrée et de sortie pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>. Les index de colonne sont stockés dans un tableau d'entiers et sont accessibles via le composant pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
int []inputColumns;  
int []outputColumns;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    inputColumns = new int[input.InputColumnCollection.Count];  
    outputColumns = new int[output.OutputColumnCollection.Count];  
  
    for(int col=0; col < input.InputColumnCollection.Count; col++)  
    {  
        IDTSInputColumn100 inputColumn = input.InputColumnCollection[col];  
        inputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID);  
    }  
  
    for(int col=0; col < output.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 outputColumn = output.OutputColumnCollection[col];  
        outputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID);  
    }  
  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ReDim inputColumns(input.InputColumnCollection.Count)  
    ReDim outputColumns(output.OutputColumnCollection.Count)  
  
    For col As Integer = 0 To input.InputColumnCollection.Count  
  
        Dim inputColumn As IDTSInputColumn100 = input.InputColumnCollection(col)  
        inputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID)  
    Next  
  
    For col As Integer = 0 To output.OutputColumnCollection.Count  
  
        Dim outputColumn As IDTSOutputColumn100 = output.OutputColumnCollection(col)  
        outputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID)  
    Next  
  
End Sub  
```  
  
### <a name="processing-rows"></a>Traitement de lignes  
 Les composants reçoivent des objets <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> qui contiennent des lignes et des colonnes dans la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Pendant cette méthode, les lignes comprises dans le tampon sont parcourues, puis les colonnes identifiées pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> sont lues et modifiées. La méthode est appelée à plusieurs reprises par la tâche de flux de données jusqu'à ce qu'aucune ligne ne soit fournie à partir du composant en amont.  
  
 Une colonne individuelle du tampon est lue ou écrite en utilisant la méthode d’accès indexeur de tableau ou l’une des méthodes **Get** ou **Set**. Les méthodes **Get** et **Set** sont plus efficaces et doivent être utilisées lorsque le type de données de la colonne dans le tampon est connu.  
  
 L'exemple de code suivant présente une implémentation de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> qui traite des lignes entrantes.  
  
```csharp  
public override void ProcessInput( int InputID, PipelineBuffer buffer)  
{  
       while( buffer.NextRow())  
       {  
            for(int x=0; x < inputColumns.Length;x++)  
            {  
                if(!buffer.IsNull(inputColumns[x]))  
                {  
                    object columnData = buffer[inputColumns[x]];  
                    // TODO: Modify the column data.  
                    buffer[inputColumns[x]] = columnData;  
                }  
            }  
  
      }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal InputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For x As Integer = 0 To inputColumns.Length  
  
                if buffer.IsNull(inputColumns(x)) = false then  
  
                    Dim columnData As Object = buffer(inputColumns(x))  
                    ' TODO: Modify the column data.  
                    buffer(inputColumns(x)) = columnData  
  
                End If  
            Next  
  
        End While  
End Sub  
```  
  
## <a name="sample"></a>Exemple  
 L'exemple suivant présente un composant de transformation simple à sorties synchrones qui convertit les valeurs de toutes les colonnes de chaîne en majuscule. Cet exemple ne contient pas toutes les méthodes et fonctionnalités présentées dans cette rubrique. Il illustre les méthodes importantes que chaque composant de transformation personnalisé à sorties synchrones doit substituer, mais ne contient pas de code pour la validation au moment de la conception.  
  
```csharp  
using System;  
using System.Collections;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Uppercase  
{  
  [DtsPipelineComponent(DisplayName = "Uppercase")]  
  public class Uppercase : PipelineComponent  
  {  
    ArrayList m_ColumnIndexList = new ArrayList();  
  
    public override void ProvideComponentProperties()  
    {  
      base.ProvideComponentProperties();  
      ComponentMetaData.InputCollection[0].Name = "Uppercase Input";  
      ComponentMetaData.OutputCollection[0].Name = "Uppercase Output";  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        if (column.DataType == DataType.DT_STR || column.DataType == DataType.DT_WSTR)  
        {  
          m_ColumnIndexList.Add((int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID));  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        foreach (int columnIndex in m_ColumnIndexList)  
        {  
          string str = buffer.GetString(columnIndex);  
          buffer.SetString(columnIndex, str.ToUpper());  
        }  
      }  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.Collections   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace Uppercase   
  
 <DtsPipelineComponent(DisplayName="Uppercase")> _   
 Public Class Uppercase   
 Inherits PipelineComponent   
   Private m_ColumnIndexList As ArrayList = New ArrayList   
  
   Public  Overrides Sub ProvideComponentProperties()   
     MyBase.ProvideComponentProperties   
     ComponentMetaData.InputCollection(0).Name = "Uppercase Input"   
     ComponentMetaData.OutputCollection(0).Name = "Uppercase Output"   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     For Each column As IDTSInputColumn100 In inputColumns   
       If column.DataType = DataType.DT_STR OrElse column.DataType = DataType.DT_WSTR Then   
         m_ColumnIndexList.Add(CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer))   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       For Each columnIndex As Integer In m_ColumnIndexList   
         Dim str As String = buffer.GetString(columnIndex)   
         buffer.SetString(columnIndex, str.ToUpper)   
       Next   
     End While   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un composant de transformation personnalisé avec des sorties asynchrones](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)   
 [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Création d'une transformation synchrone à l'aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  

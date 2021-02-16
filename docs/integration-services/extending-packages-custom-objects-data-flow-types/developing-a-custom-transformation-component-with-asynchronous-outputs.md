---
description: Développement d'un composant de transformation personnalisé à sorties asynchrones
title: Développement d’un composant de transformation personnalisé avec des sorties asynchrones | Microsoft Docs
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
- custom data flow components [Integration Services], transformation components
- asynchronous outputs [Integration Services]
- ProcessInput method
- cache [Integration Services]
- buffer allocations [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], transformation components
ms.assetid: 1c3e92c7-a4fa-4fdd-b9ca-ac3069536274
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6eeddfc5c4fff29b27462889d39748c8df2a01e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348256"
---
# <a name="developing-a-custom-transformation-component-with-asynchronous-outputs"></a>Développement d'un composant de transformation personnalisé à sorties asynchrones

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous utilisez un composant à sorties asynchrones lorsqu'une transformation ne peut pas extraire de lignes tant que le composant n'a pas reçu toutes ses lignes d'entrée, ou lorsqu'une transformation ne génère pas exactement une ligne de sortie pour chaque ligne reçue en tant qu'entrée. La transformation d'agrégation, par exemple, ne peut pas calculer une somme de lignes tant qu'elle n'a pas lu toutes les lignes. Par opposition, vous pouvez utiliser un composant à sorties synchrones lorsque vous modifiez chaque ligne de données au moment de leur transfert. Vous pouvez modifier les données de chaque ligne en place ou créer une ou plusieurs colonnes, chacune contenant une valeur pour chaque ligne d'entrée. Pour plus d’informations sur la différence entre les composants synchrones et asynchrones, consultez [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Les composants de transformation à sorties asynchrones sont uniques dans la mesure où ils font office à la fois de composants sources et de composants de destination. Ce type de composant reçoit des lignes des composants en amont et ajoute des lignes utilisées par les composants en aval. Aucun autre composant de flux de données n'effectue ces deux opérations.  
  
 Les colonnes des composants en amont disponibles pour un composant à sorties synchrones sont automatiquement accessibles aux composants situés en aval du composant. Par conséquent, un composant à sorties synchrones n'a pas besoin de définir de colonne de sortie pour fournir des colonnes et des lignes au composant suivant. Les composants à sorties asynchrones, en revanche, doivent définir des colonnes de sortie et fournir des lignes aux composants en aval. Par conséquent, un composant à sorties asynchrones doit exécuter un plus grand nombre de tâches au moment de la conception et de l'exécution, et le développeur de composants doit implémenter davantage de code.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contient plusieurs transformations à sorties asynchrones. Par exemple, la transformation de tri requiert toutes ses lignes avant de pouvoir les trier et elle utilise pour cela des sorties asynchrones. Une fois qu'elle a reçu toutes ses lignes, elle les trie et les ajoute à sa sortie.  
  
 Cette section explique en détail comment développer des transformations à sorties asynchrones. Pour plus d’informations sur le développement de composants sources, consultez [Développement d’un composant source personnalisé](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="design-time"></a>Moment de la conception  
  
### <a name="creating-the-component"></a>Création du composant  
 La propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> sur l'objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> détermine si une sortie est synchrone ou asynchrone. Pour créer une sortie asynchrone, ajoutez la sortie au composant et attribuez la valeur zéro à <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A>. La définition de cette propriété détermine également si la tâche de flux alloue des objets <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> à l'entrée et la sortie du composant, ou si une seule mémoire tampon est allouée et partagée entre les deux objets.  
  
 L'exemple de code suivant affiche un composant qui crée une sortie asynchrone dans son implémentation <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "AsyncComponent",ComponentType = ComponentType.Transform)]  
    public class AsyncComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Call the base class, which adds a synchronous input  
            // and output.  
            base.ProvideComponentProperties();  
  
            // Make the output asynchronous.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
            output.SynchronousInputID = 0;  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="AsyncComponent", ComponentType:=ComponentType.Transform)> _  
Public Class AsyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Call the base class, which adds a synchronous input  
        ' and output.  
        Me.ProvideComponentProperties()  
  
        ' Make the output asynchronous.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
        output.SynchronousInputID = 0  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Création et configuration de colonnes de sortie  
 Comme nous l'avons déjà mentionné, un composant asynchrone ajoute des colonnes à sa collection de colonnes de sortie pour fournir des colonnes aux composants en aval. Vous pouvez choisir entre plusieurs méthodes au moment de la conception, en fonction des besoins du composant. Par exemple, si vous souhaitez transférer toutes les colonnes des composants en amont aux composants en aval, vous devez substituer la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.OnInputPathAttached%2A> pour ajouter les colonnes, car il s'agit de la première méthode dans laquelle les colonnes d'entrée sont accessibles au composant.  
  
 Si le composant crée des colonnes de sortie en fonction des colonnes sélectionnées pour son entrée, substituez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetUsageType%2A> pour sélectionner les colonnes de sortie et indiquer comment elles doivent être utilisées.  
  
 Si un composant à sorties asynchrones crée des colonnes de sortie basées sur les colonnes de composants en amont et que les colonnes disponibles en amont sont modifiées, le composant doit mettre à jour sa collection de colonnes de sortie. Ces modifications doivent être détectées par le composant pendant l'exécution de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> et corrigées pendant l'exécution de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
> [!NOTE]  
>  Lorsqu'une colonne de sortie est supprimée de la collection de colonnes de sortie, les composants en aval dans le flux de données qui font référence à cette colonne sont affectés de façon négative. La colonne de sortie doit être réparée sans être supprimée ni recréée pour empêcher l'arrêt des composants en aval. Par exemple, si le type de données de la colonne a été modifié, vous devez le mettre à jour.  
  
 L'exemple de code suivant montre un composant qui ajoute une colonne de sortie à sa collection de colonnes de sortie pour chaque colonne disponible à partir du composant en amont.  
  
```csharp  
public override void OnInputPathAttached(int inputID)  
{  
   IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
   IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
   IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
   foreach (IDTSVirtualInputColumn100 vCol in vInput.VirtualInputColumnCollection)  
   {  
      IDTSOutputColumn100 outCol = output.OutputColumnCollection.New();  
      outCol.Name = vCol.Name;  
      outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage);  
   }  
}  
```  
  
```vb  
Public Overrides Sub OnInputPathAttached(ByVal inputID As Integer)  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput()  
  
    For Each vCol As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
  
        Dim outCol As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        outCol.Name = vCol.Name  
        outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage)  
  
    Next  
End Sub  
```  
  
## <a name="run-time"></a>Moment de l'exécution  
 Les composants à sorties asynchrones exécutent également une séquence de méthodes au moment de l'exécution qui diffère des autres types de composants. En premier lieu, ce sont les seuls composants qui reçoivent un appel des méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Les composants à sorties asynchrones requièrent également l'accès à toutes les lignes entrantes avant de pouvoir commencer le traitement. Ils doivent donc mettre en cache les lignes d'entrée en interne jusqu'à ce que toutes les lignes aient été lues. Enfin, les composants à sorties asynchrones reçoivent une mémoire tampon d'entrée et une mémoire tampon de sortie, contrairement aux autres composants.  
  
### <a name="understanding-the-buffers"></a>Fonctionnement des mémoires tampons  
 Le composant reçoit la mémoire tampon d'entrée au moment de l'exécution de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Cette mémoire tampon contient les lignes ajoutées par les composants en amont. La mémoire tampon contient également les colonnes de l'entrée du composant, en plus des colonnes qui ont été fournies dans la sortie d'un composant en amont mais qui n'ont pas été ajoutées à la collection d'entrée du composant asynchrone.  
  
 La mémoire tampon de sortie, fournie au composant dans <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, ne contient pas de ligne initialement. Le composant ajoute des lignes à cette mémoire tampon et fournit cette dernière aux composants en aval lorsqu'elle est pleine. La mémoire tampon de sortie contient les colonnes définies dans la collection de colonnes de sortie du composant, en plus des colonnes que d'autres composants en aval ont ajoutées à leurs sorties.  
  
 Ce comportement diffère de celui des composants à sorties synchrones, qui reçoivent une seule mémoire tampon partagée. La mémoire tampon partagée d'un composant à sorties synchrones contient les colonnes d'entrée et de sortie du composant, en plus des colonnes ajoutées aux sorties des composants en amont et en aval.  
  
### <a name="processing-rows"></a>Traitement de lignes  
  
#### <a name="caching-input-rows"></a>Mise en cache de lignes d'entrée  
 Lorsque vous écrivez un composant à sorties asynchrones, vous avez le choix entre trois options pour ajouter des lignes à la mémoire tampon de sortie. Vous pouvez les ajouter au moment de la réception des lignes d'entrée, les mettre en cache jusqu'à ce que le composant ait reçu toutes les lignes du composant en amont ou les ajouter au moment approprié pour le composant. La méthode choisie dépend des exigences du composant. Par exemple, le composant Sort requiert que toutes les lignes en amont soient reçues avant d'être triées. Par conséquent, il attend que toutes les lignes aient été lues avant d'ajouter des lignes à la mémoire tampon de sortie.  
  
 Les lignes reçues dans la mémoire tampon d'entrée doivent être mises en cache en interne par le composant en attendant qu'il soit prêt à les traiter. Les lignes entrantes de la mémoire tampon peuvent être mises en cache dans une table de données, un tableau multidimensionnel ou toute autre structure interne.  
  
#### <a name="adding-output-rows"></a>Ajout de lignes de sortie  
 Si vous ajoutez des lignes à la mémoire tampon de sortie dès leur réception ou une fois que toutes les lignes ont été reçues, vous appelez pour cela la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A> sur la mémoire tampon de sortie. Après avoir ajouté la ligne, vous définissez les valeurs de chaque colonne dans la nouvelle ligne.  
  
 Étant donné que la mémoire tampon de sortie contient parfois plus de colonnes que la collection de colonnes de sortie du composant, vous devez rechercher l'index de la colonne appropriée dans la mémoire tampon avant de définir sa valeur. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> retourne l'index de la colonne dans la ligne de la mémoire tampon avec l'ID de lignage spécifié, qui permet ensuite d'assigner la valeur à la colonne de la mémoire tampon.  
  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, appelée avant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> ou la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> est la première qui permet d'accéder à la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> et qui offre la possibilité de rechercher les index des colonnes dans les mémoires tampons d'entrée et de sortie.  
  
## <a name="sample"></a>Exemple  
 L'exemple suivant montre un composant de transformation simple à sorties asynchrones qui ajoute des lignes à la mémoire tampon de sortie au moment où elles sont reçues. Cet exemple ne contient pas toutes les méthodes et fonctionnalités présentées dans cette rubrique. Il illustre les méthodes importantes que chaque composant de transformation personnalisé à sorties asynchrones doit substituer, mais ne contient pas de code pour la validation au moment de la conception. Par ailleurs, le code dans <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> suppose que la collection de colonnes de sortie possède une colonne pour chaque colonne dans la collection de colonnes d'entrée.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
   [DtsPipelineComponent(DisplayName = "AsynchronousOutput")]  
   public class AsynchronousOutput : PipelineComponent  
   {  
      PipelineBuffer outputBuffer;  
      int[] inputColumnBufferIndexes;  
      int[] outputColumnBufferIndexes;  
  
      public override void ProvideComponentProperties()  
      {  
         // Let the base class add the input and output objects.  
         base.ProvideComponentProperties();  
  
         // Name the input and output, and make the  
         // output asynchronous.  
         ComponentMetaData.InputCollection[0].Name = "Input";  
         ComponentMetaData.OutputCollection[0].Name = "AsyncOutput";  
         ComponentMetaData.OutputCollection[0].SynchronousInputID = 0;  
      }  
      public override void PreExecute()  
      {  
         IDTSInput100 input = ComponentMetaData.InputCollection[0];  
         IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
         inputColumnBufferIndexes = new int[input.InputColumnCollection.Count];  
         outputColumnBufferIndexes = new int[output.OutputColumnCollection.Count];  
  
         for (int col = 0; col < input.InputColumnCollection.Count; col++)  
            inputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[col].LineageID);  
  
         for (int col = 0; col < output.OutputColumnCollection.Count; col++)  
            outputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[col].LineageID);  
  
      }  
  
      public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
      {  
         if (buffers.Length != 0)  
            outputBuffer = buffers[0];  
      }  
      public override void ProcessInput(int inputID, PipelineBuffer buffer)  
      {  
            // Advance the buffer to the next row.  
            while (buffer.NextRow())  
            {  
               // Add a row to the output buffer.  
               outputBuffer.AddRow();  
               for (int x = 0; x < inputColumnBufferIndexes.Length; x++)  
               {  
                  // Copy the data from the input buffer column to the output buffer column.  
                  outputBuffer[outputColumnBufferIndexes[x]] = buffer[inputColumnBufferIndexes[x]];  
               }  
            }  
         if (buffer.EndOfRowset)  
         {  
            // EndOfRowset on the input buffer is true.  
            // Set EndOfRowset on the output buffer.  
            outputBuffer.SetEndOfRowset();  
         }  
      }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="AsynchronousOutput")> _  
    Public Class AsynchronousOutput  
  
        Inherits PipelineComponent  
  
        Private outputBuffer As PipelineBuffer  
        Private inputColumnBufferIndexes As Integer()  
        Private outputColumnBufferIndexes As Integer()  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Let the base class add the input and output objects.  
            Me.ProvideComponentProperties()  
  
            ' Name the input and output, and make the  
            ' output asynchronous.  
            ComponentMetaData.InputCollection(0).Name = "Input"  
            ComponentMetaData.OutputCollection(0).Name = "AsyncOutput"  
            ComponentMetaData.OutputCollection(0).SynchronousInputID = 0  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
            Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
            ReDim inputColumnBufferIndexes(input.InputColumnCollection.Count)  
            ReDim outputColumnBufferIndexes(output.OutputColumnCollection.Count)  
  
            For col As Integer = 0 To input.InputColumnCollection.Count  
                inputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(col).LineageID)  
            Next  
  
            For col As Integer = 0 To output.OutputColumnCollection.Count  
                outputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(col).LineageID)  
            Next  
  
        End Sub  
        Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
            If buffers.Length <> 0 Then  
                outputBuffer = buffers(0)  
            End If  
  
        End Sub  
  
        Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
                ' Advance the buffer to the next row.  
                While (buffer.NextRow())  
  
                    ' Add a row to the output buffer.  
                    outputBuffer.AddRow()  
                    For x As Integer = 0 To inputColumnBufferIndexes.Length  
  
                        ' Copy the data from the input buffer column to the output buffer column.  
                        outputBuffer(outputColumnBufferIndexes(x)) = buffer(inputColumnBufferIndexes(x))  
  
                    Next  
                End While  
  
            If buffer.EndOfRowset = True Then  
                ' EndOfRowset on the input buffer is true.  
                ' Set the end of row set on the output buffer.  
                outputBuffer.SetEndOfRowset()  
            End If  
        End Sub  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un composant de transformation personnalisé à sorties synchrones](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  

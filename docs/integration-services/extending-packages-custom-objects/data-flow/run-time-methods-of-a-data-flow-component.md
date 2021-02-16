---
description: Méthodes d'exécution d'un composant de flux de données
title: Méthodes d’exécution d’un composant de flux de données | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- run-time [Integration Services]
- data flow components [Integration Services], run-time methods
ms.assetid: fd9e4317-18dd-43af-bbdc-79db32183ac4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8fa0106c903a509eb2b4f734b667323b6d71fd74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343629"
---
# <a name="run-time-methods-of-a-data-flow-component"></a>Méthodes d'exécution d'un composant de flux de données

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Au moment de l'exécution, la tâche de flux de données examine la séquence de composants, prépare un plan d'exécution et gère un pool de threads de travail qui exécute le plan de travail. La tâche charge des lignes de données à partir des sources, les traite via des transformations, puis les enregistre dans des destinations.  
  
## <a name="sequence-of-method-execution"></a>Séquence d'exécution des méthodes  
 Pendant l'exécution d'un composant de flux de données, un sous-ensemble des méthodes de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> est appelé. Les méthodes, et l'ordre dans lequel elles sont appelées, sont toujours les mêmes, à l'exception des méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Ces deux méthodes sont appelées en fonction de l'existence et de la configuration des objets <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> d'un composant.  
  
 La liste suivante présente les méthodes dans l'ordre dans lequel elles sont appelées pendant l'exécution d'un composant. Notez que lorsqu'elle est appelée, la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> l'est toujours avant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrepareForExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PostExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Cleanup%2A>  
  
### <a name="primeoutput-method"></a>Méthode PrimeOutput  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> est appelée lorsqu'un composant possède au moins une sortie, attachée à un composant en aval via un objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> et lorsque la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> de la sortie est nulle. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> est appelée pour les composants source et les transformations à sorties asynchrones. Contrairement à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> décrite ci-dessous, la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> est appelée une seule fois pour chaque composant qui la requiert.  
  
### <a name="processinput-method"></a>Méthode ProcessInput  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> est appelée pour les composants qui possèdent au moins une entrée attachée à un composant en amont par un objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100>. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> est appelée pour les composants de destination et les transformations à sorties synchrones. l'<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> est appelée à plusieurs reprises jusqu'à ce qu'il n'y ait plus aucune ligne à traiter à partir des composants en amont.  
  
## <a name="working-with-inputs-and-outputs"></a>Utilisation des entrées et des sorties  
 Au moment de l'exécution, les composants de flux de données effectuent les tâches suivantes :  
  
-   Les composants source ajoutent des lignes.  
  
-   Les composants de transformation à sorties synchrones reçoivent les lignes fournies par les composants source.  
  
-   Les composants de transformation à sorties asynchrones reçoivent des lignes et en ajoutent.  
  
-   Les composants de destination reçoivent des lignes, puis les chargent dans une destination.  
  
 Pendant l'exécution, la tâche de flux de données alloue des objets <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> qui contiennent toutes les colonnes définies dans les collections de colonnes de sortie d'une séquence de composants. Par exemple, si chacun des quatre composants d'une séquence de flux de données ajoute une colonne de sortie à sa collection de colonnes de sortie, le tampon fourni à chaque composant contient quatre colonnes, une pour chaque colonne de sortie par composant. En raison de ce comportement, un composant reçoit parfois des tampons qui contiennent des colonnes qu'il n'utilise pas.  
  
 Étant donné que les tampons reçus par votre composant peuvent contenir des colonnes que le composant n'utilisera pas, vous devez localiser les colonnes que vous souhaitez utiliser dans les collections de colonnes d'entrée et de sortie de votre composant dans le tampon fourni au composant par la tâche de flux de données. Pour ce faire, vous utilisez la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Pour des raisons liées aux performances, cette tâche s'effectue en général pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, plutôt que pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> ou <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> est appelée avant les méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> et correspond à la première possibilité pour un composant d'effectuer ce travail une fois que la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> est devenue disponible pour le composant. Pendant cette méthode, le composant doit localiser ses colonnes dans les tampons et stocker en interne ces informations afin que les colonnes puissent être utilisées dans les méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> ou <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 L'exemple de code suivant montre comment un composant de transformation à sortie synchrone localise ses colonnes d'entrée dans le tampon pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>.  
  
```csharp  
private int []bufferColumnIndex;  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    bufferColumnIndex = new int[input.InputColumnCollection.Count];  
  
    for( int x=0; x < input.InputColumnCollection.Count; x++)  
    {  
        IDTSInputColumn100 column = input.InputColumnCollection[x];  
        bufferColumnIndex[x] = BufferManager.FindColumnByLineageID( input.Buffer, column.LineageID);  
    }  
}  
```  
  
```vb  
Dim bufferColumnIndex As Integer()  
  
    Public Overrides Sub PreExecute()  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
        ReDim bufferColumnIndex(input.InputColumnCollection.Count)  
  
        For x As Integer = 0 To input.InputColumnCollection.Count  
  
            Dim column As IDTSInputColumn100 = input.InputColumnCollection(x)  
            bufferColumnIndex(x) = BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID)  
  
        Next  
  
    End Sub  
```  
  
### <a name="adding-rows"></a>Ajout de lignes  
 Les composants fournissent des lignes aux composants en aval en ajoutant des lignes aux objets <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. La tâche de flux de données fournit un tableau de tampons de sortie, un pour chaque objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> connecté à un composant en aval, en tant que paramètre de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Les composants source et de transformation à sorties asynchrones ajoutent des lignes aux tampons et appellent la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A> lorsqu'ils ont terminé d'ajouter des lignes. La tâche de flux de données gère les tampons de sortie qu'elle fournit aux composants et, lorsque le tampon sature, déplace automatiquement les lignes incluses dans le tampon vers le composant suivant. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> est appelée une fois par composant, contrairement à la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, appelée à plusieurs reprises.  
  
 L'exemple de code suivant montre comment un composant ajoute des lignes à ses tampons de sortie pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, puis appelle la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A>.  
  
```csharp  
public override void PrimeOutput( int outputs, int []outputIDs,PipelineBuffer []buffers)  
{  
    for( int x=0; x < outputs; x++ )  
    {  
        IDTSOutput100 output = ComponentMetaData.OutputCollection.GetObjectByID( outputIDs[x]);  
        PipelineBuffer buffer = buffers[x];  
  
        // TODO: Add rows to the output buffer.  
    }  
    foreach( PipelineBuffer buffer in buffers )  
    {  
        /// Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset();  
    }  
}  
```  
  
```vb  
public overrides sub PrimeOutput( outputs as Integer , outputIDs() as Integer ,buffers() as PipelineBuffer buffers)  
  
    For x As Integer = 0 To outputs.MaxValue  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.GetObjectByID(outputIDs(x))  
        Dim buffer As PipelineBuffer = buffers(x)  
  
        ' TODO: Add rows to the output buffer.  
  
    Next  
  
    For Each buffer As PipelineBuffer In buffers  
  
        ' Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset()  
  
    Next  
  
End Sub  
```  
  
 Pour plus d’informations sur le développement de composants qui ajoutent des lignes aux mémoires tampons de sortie, consultez [Développement d’un composant source personnalisé](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md) et [Développement d’un composant de transformation personnalisé à sorties asynchrones](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md).  
  
### <a name="receiving-rows"></a>Réception de lignes  
 Les composants reçoivent des lignes provenant des composants en amont dans les objets <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. La tâche de flux de données fournit un objet <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> qui contient les lignes ajoutées au flux de données par les composants en amont en tant que paramètre de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Ce tampon d'entrée peut être utilisé pour examiner et modifier les lignes et colonnes dans le tampon, mais il ne permet pas d'ajouter ou de supprimer des lignes. La méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> est appelée à plusieurs reprises jusqu'à ce qu'il n'y ait plus de tampon disponible. Lors du dernier appel, la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> a la valeur **true**. Vous pouvez parcourir la collection de lignes dans le tampon en utilisant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A>, qui permet d'accéder à la ligne suivante du tampon. Cette méthode retourne **false** lorsque le tampon est sur la dernière ligne de la collection. Vous n'avez pas à vérifier la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> sauf si vous devez effectuer une action supplémentaire une fois que les dernières lignes de données ont été traitées.  
  
 Le texte suivant illustre le modèle correct pour l'utilisation de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A> et de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> :  
  
 `while (buffer.NextRow())`  
  
 `{`  
  
 `// Do something with each row.`  
  
 `}`  
  
 `if (buffer.EndOfRowset)`  
  
 `{`  
  
 `// Optionally, do something after all rows have been processed.`  
  
 `}`  
  
 L'exemple de code suivant montre comment un composant traite les lignes dans les tampons d'entrée pendant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
public override void ProcessInput( int inputID, PipelineBuffer buffer )  
{  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
        while( buffer.NextRow())  
        {  
            // TODO: Examine the columns in the current row.  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
        While buffer.NextRow() = True  
  
            ' TODO: Examine the columns in the current row.  
        End While  
  
End Sub  
```  
  
 Pour plus d’informations sur le développement de composants qui reçoivent des lignes dans les mémoires tampons d’entrée, consultez [Développement d’un composant de destination personnalisé](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md) et [Développement d’un composant de transformation personnalisé à sorties asynchrones](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes de conception d'un composant de flux de données](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md)  
  
  

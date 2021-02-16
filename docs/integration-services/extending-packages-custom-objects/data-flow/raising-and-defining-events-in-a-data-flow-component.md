---
description: Déclenchement et définition d'événements dans un composant de flux de données
title: Déclenchement et définition d’événements dans un composant de flux de données | Microsoft Docs
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
- data flow components [Integration Services], events
- events [Integration Services], custom
- custom events [Integration Services]
- custom data flow components [Integration Services], events
- events [Integration Services], raising
- predefined events [Integration Services]
ms.assetid: 1d8c5358-9384-47a8-b7cb-7b0650384119
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d6e0a5d7694323f22a9262df37353f8117b9666
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343663"
---
# <a name="raising-and-defining-events-in-a-data-flow-component"></a>Déclenchement et définition d'événements dans un composant de flux de données

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Les développeurs de composants peuvent déclencher un sous-ensemble des événements définis dans l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> en appelant les méthodes exposées sur la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. Vous pouvez également définir des événements personnalisés à l'aide de la collection <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A>, puis les déclencher pendant l'exécution en utilisant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>. Cette section décrit comment créer et déclencher un événement. Elle fournit également des conseils sur l'opportunité de déclencher des événements au moment de la conception.  
  
## <a name="raising-events"></a>Déclenchement d’événements  
 Les composants déclenchent des événements à l’aide des méthodes **Fire\<X>** de l’interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>. Vous pouvez déclencher des événements pendant la conception et l'exécution de composants. En général, les méthodes <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A> sont appelées pendant la validation, lors de la conception de composants. Ces événements affichent des messages dans le volet **Liste d’erreurs[!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] de**  et fournissent des commentaires aux utilisateurs du composant lorsque ce dernier n’est pas correctement configuré.  
  
 Les composants peuvent également déclencher des événements à tout moment pendant l'exécution. Les événements permettent aux développeurs de composants de fournir des commentaires aux utilisateurs du composant pendant son exécution. L'appel de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> pendant l'exécution risque de provoquer l'échec du package.  
  
## <a name="defining-and-raising-custom-events"></a>Définition et déclenchement d'événements personnalisés  
  
### <a name="defining-a-custom-event"></a>Définition d'un événement personnalisé  
 L'appel de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> de la collection <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A> permet de créer des événements personnalisés. Cette collection est définie par la tâche de flux de données et fournie en tant que propriété au développeur de composants par le biais de la classe de base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>. Cette classe contient des événements personnalisés définis par la tâche de flux de données et des événements personnalisés définis par le composant pendant l'exécution de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>.  
  
 Les événements personnalisés d'un composant ne sont pas conservés dans le package XML. Par conséquent, la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> est appelée pendant la conception et l'exécution pour permettre au composant de définir les événements qu'il déclenche.  
  
 Le paramètre *allowEventHandlers* de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> spécifie si le composant autorise la création d’objets <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> pour l’événement. Notez que les <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> sont synchrones. Par conséquent, le composant ne recommence à s'exécuter qu'après la fin de l'exécution d'un <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> attaché à l'événement personnalisé. Si le paramètre *allowEventHandlers* a la valeur **true**, chaque paramètre de l’événement est mis automatiquement à la disposition des objets <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> par le biais de variables créées et remplies automatiquement par le runtime [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
### <a name="raising-a-custom-event"></a>Déclenchement d'un événement personnalisé  
 Les composants déclenchent des événements personnalisés en appelant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A> et en fournissant le nom, le texte et les paramètres de l'événement. Si le paramètre *allowEventHandlers* a la valeur **true**, tous les <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> créés pour l’événement personnalisé sont exécutés par le moteur d’exécution [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
### <a name="custom-event-sample"></a>Exemple d'événement personnalisé  
 L'exemple de code suivant présente un composant qui définit un événement personnalisé pendant l'exécution de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>, puis qui déclenche l'événement au moment de l'exécution en appelant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>.  
  
```csharp  
public override void RegisterEvents()  
{  
    string [] parameterNames = new string[2]{"RowCount", "StartTime"};  
    ushort [] parameterTypes = new ushort[2]{ DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)};  
    string [] parameterDescriptions = new string[2]{"The number of rows to sort.", "The start time of the Sort operation."};  
    EventInfos.Add("StartingSort","Fires when the component begins sorting the rows.",false,ref parameterNames, ref parameterTypes, ref parameterDescriptions);  
}  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
    while (buffer.NextRow())  
    {  
       // Process buffer rows.  
    }  
  
    IDTSEventInfo100 eventInfo = EventInfos["StartingSort"];  
    object []arguments = new object[2]{buffer.RowCount, DateTime.Now };  
    ComponentMetaData.FireCustomEvent("StartingSort", "Beginning sort operation.", ref arguments, ComponentMetaData.Name, ref FireSortEventAgain);  
}  
```  
  
```vb  
Public  Overrides Sub RegisterEvents()   
  Dim parameterNames As String() = New String(2) {"RowCount", "StartTime"}   
  Dim parameterTypes As System.UInt16() = New System.UInt16(2) {DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)}   
  Dim parameterDescriptions As String() = New String(2) {"The number of rows to sort.", "The start time of the Sort operation."}   
  EventInfos.Add("StartingSort", "Fires when the component begins sorting the rows.", False, parameterNames, parameterTypes, parameterDescriptions)   
End Sub   
  
Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
  While buffer.NextRow   
  End While   
  Dim eventInfo As IDTSEventInfo100 = EventInfos("StartingSort")   
  Dim arguments As Object() = New Object(2) {buffer.RowCount, DateTime.Now}   
  ComponentMetaData.FireCustomEvent("StartingSort", _  
    "Beginning sort operation.", arguments, _  
    ComponentMetaData.Name, FireSortEventAgain)   
End Sub  
```  

## <a name="see-also"></a>Voir aussi  
 [Gestionnaires d’événements Integration Services &#40;SSIS&#41](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Ajouter un gestionnaire d’événements à un package](../../integration-services-ssis-event-handlers.md)  
  

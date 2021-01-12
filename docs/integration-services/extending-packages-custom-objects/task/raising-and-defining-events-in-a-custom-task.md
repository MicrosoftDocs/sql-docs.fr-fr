---
description: Déclenchement et définition d'événements dans une tâche personnalisée
title: Déclenchement et définition d’événements dans une tâche personnalisée | Microsoft Docs
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
- SSIS events, custom
- status information [SQL Server], task events
- custom tasks [Integration Services], events
- SSIS custom tasks, events
- IDTSComponentEvents interface
- events [Integration Services], custom
- events [Integration Services], runtime
- custom events [Integration Services]
- SSIS events, runtime
- IDTSEvents interface
ms.assetid: e0898aa1-e90c-4c4e-99d4-708a76efddfd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1fcafce23542f1d5d1ea1756cdb361d5197286b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091589"
---
# <a name="raising-and-defining-events-in-a-custom-task"></a>Déclenchement et définition d'événements dans une tâche personnalisée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Le moteur d’exécution [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] propose une collection d’événements qui fournissent l’état d’avancement d’une tâche lors de sa validation et de son exécution. L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> définit ces événements et elle est fournie aux tâches en tant que paramètre pour les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A>.  
  
 Il existe un autre jeu d'événements, définis dans l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents>, déclenchée de la part de la tâche par l'objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. L'objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> déclenche des événements qui se produisent avant et après la validation et l'exécution, tandis que la tâche déclenche des événements qui se produisent pendant l'exécution et la validation.  
  
## <a name="creating-custom-events"></a>Création d'événements personnalisés  
 Les développeurs de tâches personnalisées peuvent définir de nouveaux événements personnalisés en créant un objet <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo> dans leur implémentation substituée de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Task.InitializeTask%2A>. Une fois que l’objet <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo> est créé, il est ajouté à la collection **EventInfos** à l’aide de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A>. La signature de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A> est la suivante :  
  
 `public void Add(string eventName, string description, bool allowEventHandlers, string[] parameterNames, TypeCode[] parameterTypes, string[] parameterDescriptions);`  
  
 L’exemple de code suivant présente la méthode **InitializeTask** d’une tâche personnalisée, où deux événements personnalisés sont créés et leurs propriétés sont définies. Les nouveaux événements sont ensuite ajoutés à la collection <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos>.  
  
 Pour le premier événement personnalisé, *eventName* a la valeur « **OnBeforeIncrement** » et *description* a la valeur « **Fires after the initial value is updated.** ». Le paramètre suivant, la valeur **true** indique que cet événement doit autoriser la création d’un conteneur de gestionnaire d’événements pour gérer l’événement. Le gestionnaire d'événements est un conteneur qui fournit une structure dans un package et des services à des tâches, comme d'autres conteneurs tels que le package, Séquence, ForLoop et ForEachLoop. Lorsque le paramètre *allowEventHandlers* a la valeur **true**, des objets <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> sont créés pour l’événement. Tous les paramètres définis pour l'événement sont maintenant disponibles pour l'objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> dans la collection de variables de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>.  
  
```csharp  
public override void InitializeTask(Connections connections,  
   VariableDispenser variables, IDTSInfoEvents events,  
   IDTSLogging log, EventInfos eventInfos,  
   LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
{  
    this.eventInfos = eventInfos;  
    string[] paramNames = new string[1];  
    TypeCode[] paramTypes = new TypeCode[1]{TypeCode.Int32};  
    string[] paramDescriptions = new string[1];  
  
    paramNames[0] = "InitialValue";  
    paramDescriptions[0] = "The value before it is incremented.";  
  
    this.eventInfos.Add("OnBeforeIncrement",   
      "Fires before the task increments the value.",  
      true,paramNames,paramTypes,paramDescriptions);  
    this.onBeforeIncrement = this.eventInfos["OnBeforeIncrement"];  
  
    paramDescriptions[0] = "The value after it has been incremented.";  
    this.eventInfos.Add("OnAfterIncrement",  
      "Fires after the initial value is updated.",  
      true,paramNames, paramTypes,paramDescriptions);  
    this.onAfterIncrement = this.eventInfos["OnAfterIncrement"];  
}  
```  
  
```vb  
Public Overrides Sub InitializeTask(ByVal connections As Connections, _  
ByVal variables As VariableDispenser, ByVal events As IDTSInfoEvents, _  
ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, _  
ByVal logEntryInfos As LogEntryInfos, ByVal refTracker As ObjectReferenceTracker)   
  
    Dim paramNames(0) As String  
    Dim paramTypes(0) As TypeCode = {TypeCode.Int32}  
    Dim paramDescriptions(0) As String  
  
    Me.eventInfos = eventInfos  
  
    paramNames(0) = "InitialValue"  
    paramDescriptions(0) = "The value before it is incremented."  
  
    Me.eventInfos.Add("OnBeforeIncrement", _  
      "Fires before the task increments the value.", _  
      True, paramNames, paramTypes, paramDescriptions)  
    Me.onBeforeIncrement = Me.eventInfos("OnBeforeIncrement")  
  
    paramDescriptions(0) = "The value after it has been incremented."  
    Me.eventInfos.Add("OnAfterIncrement", _  
      "Fires after the initial value is updated.", True, _  
      paramNames, paramTypes, paramDescriptions)  
    Me.onAfterIncrement = Me.eventInfos("OnAfterIncrement")  
  
End Sub  
```  
  
## <a name="raising-custom-events"></a>Déclenchement d'événements personnalisés  
 Les événements personnalisés sont déclenchés en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>. La ligne de code suivante déclenche un événement personnalisé.  
  
```csharp  
componentEvents.FireCustomEvent(this.onBeforeIncrement.Name,  
   this.onBeforeIncrement.Description, ref arguments,  
   null, ref bFireOnBeforeIncrement);  
```  
  
```vb  
componentEvents.FireCustomEvent(Me.onBeforeIncrement.Name, _  
Me.onBeforeIncrement.Description, arguments, _  
Nothing,  bFireOnBeforeIncrement)  
```  
  
## <a name="sample"></a>Exemple  
 L’exemple suivant montre une tâche qui définit un événement personnalisé dans la méthode **InitializeTask**, ajoute cet événement personnalisé à la collection <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos>, puis le déclenche au cours de sa méthode **Exécute** en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A>.  
  
```csharp  
[DtsTask(DisplayName = "CustomEventTask")]  
    public class CustomEventTask : Task  
    {  
        public override DTSExecResult Execute(Connections connections,   
          VariableDispenser variableDispenser, IDTSComponentEvents componentEvents,  
           IDTSLogging log, object transaction)  
        {  
            bool fireAgain;  
            object[] args = new object[1] { "The value of the parameter." };  
            componentEvents.FireCustomEvent( "MyCustomEvent",   
              "Firing the custom event.", ref args,  
              "CustomEventTask" , ref fireAgain );  
            return DTSExecResult.Success;  
        }  
  
        public override void InitializeTask(Connections connections,  
          VariableDispenser variableDispenser, IDTSInfoEvents events,  
          IDTSLogging log, EventInfos eventInfos,  
          LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
        {  
            string[] names = new string[1] {"Parameter1"};  
            TypeCode[] types = new TypeCode[1] {TypeCode.String};  
            string[] descriptions = new string[1] {"Parameter description." };  
  
            eventInfos.Add("MyCustomEvent",  
             "Fires when my interesting event happens.",  
             true, names, types, descriptions);  
  
        }  
   }  
```  
  
```vb  
<DtsTask(DisplayName = "CustomEventTask")> _   
    Public Class CustomEventTask  
     Inherits Task  
        Public Overrides Function Execute(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser, _  
          ByVal componentEvents As IDTSComponentEvents, _  
          ByVal log As IDTSLogging, ByVal transaction As Object) _  
          As DTSExecResult  
  
            Dim fireAgain As Boolean  
            Dim args() As Object =  New Object(1) {"The value of the parameter."}  
  
            componentEvents.FireCustomEvent("MyCustomEvent", _  
              "Firing the custom event.", args, _  
              "CustomEventTask" ,  fireAgain)  
            Return DTSExecResult.Success  
        End Function  
  
        Public Overrides  Sub InitializeTask(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser,  
          ByVal events As IDTSInfoEvents,  
          ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, ByVal logEnTryInfos As LogEnTryInfos, ByVal refTracker As ObjectReferenceTracker)  
  
            Dim names() As String =  New String(1) {"Parameter1"}  
            Dim types() As TypeCode =  New TypeCode(1) {TypeCode.String}  
            Dim descriptions() As String =  New String(1) {"Parameter description."}  
  
            eventInfos.Add("MyCustomEvent", _  
              "Fires when my interesting event happens.", _  
              True, names, types, descriptions)  
  
        End Sub  
  
    End Class  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaires d’événements Integration Services &#40;SSIS&#41](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Ajouter un gestionnaire d’événements à un package](../../integration-services-ssis-event-handlers.md)  
  

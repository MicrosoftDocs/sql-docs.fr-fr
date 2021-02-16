---
description: Ajout de la prise en charge du débogage dans une tâche personnalisée
title: Ajout de la prise en charge du débogage dans une tâche personnalisée | Microsoft Docs
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
- BreakpointManager class
- breakpoints [Integration Services]
- custom tasks [Integration Services], debugging
- IDTSSuspend interface
- IDTSBreakpointSite interface
- SSIS custom tasks, debugging
- debugging [Integration Services], custom tasks
ms.assetid: 7f06e49b-0b60-4e81-97da-d32dc248264a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 28ba35dc2aecd6c25da114493b8c6e424b77dccf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346530"
---
# <a name="adding-support-for-debugging-in-a-custom-task"></a>Ajout de la prise en charge du débogage dans une tâche personnalisée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Le moteur d'exécution [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] permet aux packages, tâches et autres types de conteneurs d'être suspendus pendant l'exécution à l'aide de points d'arrêt. L'utilisation de points d'arrêt vous permet d'examiner et de corriger les erreurs qui empêchent votre application ou vos tâches de s'exécuter correctement. L'architecture de point d'arrêt permet au client d'évaluer la valeur d'exécution des objets contenus dans le package aux points d'exécution définis pendant la suspension du traitement de la tâche.  
  
 Les développeurs de tâches personnalisées peuvent utiliser cette architecture pour créer des cibles de points d'arrêt personnalisées en utilisant l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> et son interface parente <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> définit l'interaction entre le moteur d'exécution et la tâche pour créer et gérer des sites ou des cibles de points d'arrêt personnalisés. L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> fournit des méthodes et propriétés appelées par le moteur d'exécution pour notifier la tâche de suspendre ou reprendre son exécution.  
  
 Un site ou une cible de point d'arrêt est un point dans l'exécution de la tâche où le traitement peut être suspendu. Les utilisateurs sélectionnent un site de point d’arrêt parmi les sites disponibles dans la boîte de dialogue **Définir des points d’arrêt**. Par exemple, outre les options de point d'arrêt par défaut, le conteneur de boucle Foreach propose l'option « Arrêter au début de chaque itération de la boucle ».  
  
 Lorsqu'une tâche atteint une cible de point d'arrêt pendant l'exécution, elle évalue cette cible de point d'arrêt pour déterminer si un point d'arrêt est activé. Celui-ci indique que l'utilisateur souhaite que l'exécution s'arrête à ce point d'arrêt. Si le point d'arrêt est activé, la tâche déclenche l'événement <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> pour le moteur d'exécution. Le moteur d’exécution répond à l’événement en appelant la méthode **Suspend** de chaque tâche qui est en cours d’exécution dans le package. L’exécution de la tâche reprend lorsque le runtime appelle la méthode **ResumeExecution** de la tâche suspendue.  
  
 Les tâches qui n'utilisent pas de points d'arrêt doivent encore implémenter les interfaces <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> et <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend>. Cette implémentation garantit que la tâche est correctement suspendue lorsque d'autres objets contenus dans le package déclenchent des événements <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
## <a name="idtsbreakpointsite-interface-and-breakpointmanager"></a>Interface IDTSBreakpointSite et BreakpointManager  
 Les tâches créent des cibles de points d'arrêt en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.CreateBreakpointTarget%2A> de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, ce qui permet de fournir un ID entier et une description de chaîne comme paramètres. Lorsque la tâche atteint le point dans son code qui contient une cible de point d'arrêt, elle évalue la cible de point d'arrêt en utilisant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager.IsBreakpointTargetEnabled%2A> pour déterminer si ce point d'arrêt est activé. Si la valeur est **true**, la tâche avertit le moteur d’exécution en déclenchant l’événement <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A>.  
  
 L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite> définit une méthode unique, <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite.AcceptBreakpointManager%2A>, appelée par le moteur d'exécution pendant la création de la tâche. Cette méthode fournit comme paramètre l'objet <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>, utilisé ensuite par la tâche pour créer et gérer ses points d'arrêt. Les tâches doivent stocker l’objet <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager> localement pour pouvoir l’utiliser pendant les méthodes **Validate** et **Execute**.  
  
 L'exemple de code suivant montre comment créer une cible de point d'arrêt en utilisant l'objet <xref:Microsoft.SqlServer.Dts.Runtime.BreakpointManager>. L'exemple permet d'appeler la méthode <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents.OnBreakpointHit%2A> pour déclencher l'événement.  
  
```csharp  
public void AcceptBreakpointManager( BreakpointManager breakPointManager )  
{  
   //   Store the breakpoint manager locally.  
   this.bpm  = breakPointManager;  
}  
public override DTSExecResult Execute( Connections connections,  
  Variables variables, IDTSComponentEvents events,  
  IDTSLogging log, DtsTransaction txn)  
{  
   //   Create a breakpoint.  
   this.bpm.CreateBreakPointTarget( 1 , "A sample breakpoint target." );  
...  
   if( this.bpm.IsBreakpointTargetEnabled( 1 ) == true )  
      events.OnBreakpointHit( this.bpm.GetBreakpointTarget( 1 ) );  
}  
```  
  
```vb  
Public Sub AcceptBreakpointManager(ByVal breakPointManager As BreakpointManager)  
  
   ' Store the breakpoint manager locally.  
   Me.bpm  = breakPointManager  
  
End Sub  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
  ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
  ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' Create a breakpoint.  
   Me.bpm.CreateBreakPointTarget(1 , "A sample breakpoint target.")  
  
   If Me.bpm.IsBreakpointTargetEnabled(1) = True Then  
      events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(1))  
   End If  
  
End Function  
```  
  
## <a name="idtssuspend-interface"></a>Interface IDTSSuspend  
 L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> définit les méthodes appelées par le moteur d'exécution lorsqu'il suspend ou reprend l'exécution d'une tâche. L’interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSuspend> est implémentée par l’interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSBreakpointSite>. Ses méthodes **Suspend** et **ResumeExecution** sont généralement remplacées par la tâche personnalisée. Lorsque le moteur d’exécution reçoit un événement **OnBreakpointHit** d’une tâche, il appelle la méthode **Suspend** de chaque tâche en cours d’exécution pour leur indiquer qu’elles doivent se mettre en pause. Lorsque le client reprend l’exécution, le moteur d’exécution appelle la méthode **ResumeExecution** des tâches suspendues.  
  
 La suspension et la reprise de l'exécution d'une tâche impliquent l'interruption et la reprise du thread d'exécution de la tâche. Dans le code managé, vous devez pour cela utiliser la classe **ManualResetEvent** de l’espace de noms **System.Threading** du .NET Framework.  
  
 L'exemple de code suivant montre la suspension et la reprise de l'exécution d'une tâche. Remarquez que la méthode **Execute** a changé par rapport à l’exemple de code précédent et que le thread d’exécution est mis en pause lors du déclenchement du point d’arrêt.  
  
```csharp  
private ManualResetEvent m_suspended = new ManualResetEvent( true );  
private ManualResetEvent m_canExecute = new ManualResetEvent( true );  
private int   m_suspendRequired = 0;  
private int   m_debugMode = 0;  
  
public override DTSExecResult Execute( Connections connections, Variables variables, IDTSComponentEvents events, IDTSLogging log, DtsTransaction txn)  
{  
   // While a task is not executing, it is suspended.    
   // Now that we are executing,  
   // change to not suspended.  
   ChangeEvent(m_suspended, false);  
  
   // Check for a suspend before doing any work,   
   // in case the suspend and execute calls  
   // were initiated at virtually the same time.  
   CheckAndSuspend();  
   CheckAndFireBreakpoint( componentEvents, 1);  
}  
private void CheckAndSuspend()  
{  
   // Loop until we can execute.    
   // The loop is required rather than a simple If  
   // because there is a time between the return from WaitOne and the  
   // reset that we might receive another Suspend call.    
   // Suspend() will see that we are suspended  
   // and return.  So we need to rewait.  
   while (!m_canExecute.WaitOne(0, false))  
   {  
      ChangeEvent(m_suspended, true);  
      m_canExecute.WaitOne();  
      ChangeEvent(m_suspended, false);  
   }  
}  
private void CheckAndFireBreakpoint(IDTSComponentEvents events, int breakpointID)  
{  
   // If the breakpoint is enabled, fire it.  
   if (m_debugMode != 0 &&    this.bpm.IsBreakpointTargetEnabled(breakpointID))  
   {  
      //   Enter a suspend mode before firing the breakpoint.    
      //   Firing the breakpoint will cause the runtime   
      //   to call Suspend on this task.    
      //   Because we are blocked on the breakpoint,   
      //   we are suspended.  
      ChangeEvent(m_suspended, true);  
      events.OnBreakpointHit(this.bpm.GetBreakpointTarget(breakpointID));  
      ChangeEvent(m_suspended, false);  
   }  
   // Check for a suspension for two reasons:   
   //   1. If we are at a point where we could fire a breakpoint,   
   //      we are at a valid suspend point.  Even if we didn't hit a  
   //      breakpoint, the runtime may have called suspend,   
   //      so check for it.       
   //   2. Between the return from OnBreakpointHit   
   //      and the reset of the event, it is possible to have  
   //      received a suspend call from which we returned because   
   //      we were already suspended.  We need to be sure it is okay  
   //      to continue executing now.  
   CheckAndSuspend();  
}  
static void ChangeEvent(ManualResetEvent e, bool shouldSet)  
{  
   bool succeeded;  
   if (shouldSet)  
      succeeded = e.Set();  
   else  
      succeeded = e.Reset();  
  
   if (!succeeded)  
      throw new Exception("Synchronization object failed.");  
  
}  
public bool SuspendRequired  
{  
   get   {return m_suspendRequired != 0;}  
   set  
   {  
      // This lock is also taken by Suspend().    
      // Because it is possible for the package to be  
      // suspended and resumed in quick succession,   
      // this property might be set before  
      // the actual Suspend() call.    
      // Without the lock, the Suspend() might reset the canExecute  
      // event after we set it to abort the suspension.  
      lock (this)  
      {  
         Interlocked.Exchange(ref m_suspendRequired, value ? 1 : 0);  
         if (!value)  
            ResumeExecution();  
      }  
   }  
}  
public void ResumeExecution()  
{  
   ChangeEvent( m_canExecute,true );  
}  
public void Suspend()  
{  
   // This lock is also taken by the set SuspendRequired method.    
   // It prevents this call from overriding an   
   // aborted suspension.  See comments in set SuspendRequired.  
   lock (this)  
   {  
      // If a Suspend is required, do it.  
      if (m_suspendRequired != 0)  
         ChangeEvent(m_canExecute, false);  
   }  
   // We can't return from Suspend until the task is "suspended".  
   // This can happen one of two ways:   
   // the m_suspended event occurs, indicating that the execute thread  
   // has suspended, or the canExecute flag is set,   
   // indicating that a suspend is no longer required.  
   WaitHandle [] suspendOperationComplete = {m_suspended, m_canExecute};  
   WaitHandle.WaitAny(suspendOperationComplete);  
}  
```  
  
```vb  
Private m_suspended As ManualResetEvent = New ManualResetEvent(True)  
Private m_canExecute As ManualResetEvent = New ManualResetEvent(True)  
Private m_suspendRequired As Integer = 0  
Private m_debugMode As Integer = 0  
  
Public Overrides Function Execute(ByVal connections As Connections, _  
ByVal variables As Variables, ByVal events As IDTSComponentEvents, _  
ByVal log As IDTSLogging, ByVal txn As DtsTransaction) As DTSExecResult  
  
   ' While a task is not executing it is suspended.    
   ' Now that we are executing,  
   ' change to not suspended.  
   ChangeEvent(m_suspended, False)  
  
   ' Check for a suspend before doing any work,   
   ' in case the suspend and execute calls  
   ' were initiated at virtually the same time.  
   CheckAndSuspend()  
   CheckAndFireBreakpoint(componentEvents, 1)  
  
End Function  
  
Private Sub CheckAndSuspend()  
  
   ' Loop until we can execute.    
   ' The loop is required rather than a simple if  
   ' because there is a time between the return from WaitOne and the  
   ' reset that we might receive another Suspend call.    
   ' Suspend() will see that we are suspended  
   ' and return.  So we need to rewait.  
   Do While Not m_canExecute.WaitOne(0, False)  
              ChangeEvent(m_suspended, True)  
              m_canExecute.WaitOne()  
              ChangeEvent(m_suspended, False)  
   Loop  
  
End Sub  
  
Private Sub CheckAndFireBreakpoint(ByVal events As IDTSComponentEvents, _  
ByVal breakpointID As Integer)  
  
   ' If the breakpoint is enabled, fire it.  
   If m_debugMode <> 0 AndAlso Me.bpm.IsBreakpointTargetEnabled(breakpointID) Then  
              '   Enter a suspend mode before firing the breakpoint.    
              '   Firing the breakpoint will cause the runtime   
              '   to call Suspend on this task.    
              '   Because we are blocked on the breakpoint,   
              '   we are suspended.  
              ChangeEvent(m_suspended, True)  
              events.OnBreakpointHit(Me.bpm.GetBreakpointTarget(breakpointID))  
              ChangeEvent(m_suspended, False)  
   End If  
  
   ' Check for a suspension for two reasons:   
   '   1. If we are at a point where we could fire a breakpoint,   
   '         we are at a valid suspend point.  Even if we didn't hit a  
   '         breakpoint, the runtime may have called suspend,   
   '         so check for it.     
   '   2. Between the return from OnBreakpointHit   
   '         and the reset of the event, it is possible to have  
   '         received a suspend call from which we returned because   
   '         we were already suspended.  We need to be sure it is okay  
   '         to continue executing now.  
   CheckAndSuspend()  
  
End Sub  
  
Shared Sub ChangeEvent(ByVal e As ManualResetEvent, ByVal shouldSet As Boolean)  
  
   Dim succeeded As Boolean  
   If shouldSet Then  
              succeeded = e.Set()  
   Else  
              succeeded = e.Reset()  
   End If  
  
   If (Not succeeded) Then  
              Throw New Exception("Synchronization object failed.")  
   End If  
  
End Sub  
  
Public Property SuspendRequired() As Boolean  
   Get  
               Return m_suspendRequired <> 0  
  End Get  
  Set  
    ' This lock is also taken by Suspend().    
     '   Because it is possible for the package to be  
     '   suspended and resumed in quick succession,   
     '   this property might be set before  
     '   the actual Suspend() call.    
     '   Without the lock, the Suspend() might reset the canExecute  
     '   event after we set it to abort the suspension.  
              SyncLock Me  
                         Interlocked.Exchange(m_suspendRequired,IIf(Value, 1, 0))  
                         If (Not Value) Then  
                                    ResumeExecution()  
                         End If  
              End SyncLock  
   End Set  
End Property  
  
Public Sub ResumeExecution()  
   ChangeEvent(m_canExecute,True)  
End Sub  
  
Public Sub Suspend()  
  
   ' This lock is also taken by the set SuspendRequired method.    
   ' It prevents this call from overriding an   
   ' aborted suspension.  See comments in set SuspendRequired.  
   SyncLock Me  
   ' If a Suspend is required, do it.  
              If m_suspendRequired <> 0 Then  
                         ChangeEvent(m_canExecute, False)  
              End If  
   End SyncLock  
   ' We can't return from Suspend until the task is "suspended".  
   ' This can happen one of two ways:   
   ' the m_suspended event occurs, indicating that the execute thread  
   ' has suspended, or the canExecute flag is set,   
   ' indicating that a suspend is no longer required.  
   Dim suspendOperationComplete As WaitHandle() = {m_suspended, m_canExecute}  
   WaitHandle.WaitAny(suspendOperationComplete)  
  
End Sub  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Débogage du flux de contrôle](../../../integration-services/troubleshooting/debugging-control-flow.md)  
  
  

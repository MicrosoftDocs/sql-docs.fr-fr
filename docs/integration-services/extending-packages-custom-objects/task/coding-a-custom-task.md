---
description: Codage d'une tâche personnalisée
title: Codage d’une tâche personnalisée | Microsoft Docs
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
- Validate method
- custom tasks [Integration Services], validating
- validation [Integration Services], design-time tasks
- SSIS custom tasks, validating
ms.assetid: dc224f4f-b339-4eb6-a008-1b4fe0ea4fd2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f336a5969e608d1f2647b02679e87da0a65c1bad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348218"
---
# <a name="coding-a-custom-task"></a>Codage d'une tâche personnalisée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Après avoir créé une classe qui hérite de la classe de base [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task), puis appliqué l’attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> à cette classe, vous devez substituer l’implémentation des propriétés et des méthodes de la classe de base afin de fournir vos fonctionnalités personnalisées.  
  
## <a name="configuring-the-task"></a>Configuration de la tâche  
  
### <a name="validating-the-task"></a>Validation de la tâche  
 Lorsque vous concevez un package [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vous pouvez utiliser la validation pour vérifier des paramètres sur chaque tâche, afin d'intercepter les paramètres incorrects ou inappropriés dès qu'ils sont définis, au lieu de détecter toutes les erreurs uniquement au moment de l'exécution. L'objectif de la validation est de déterminer si la tâche contient des paramètres ou connexions non valides qui l'empêcheront de s'exécuter avec succès. Elle permet de veiller à ce que le package contienne des tâches qui ont de bonnes chances de s'exécuter dès leur premier essai.  
  
 Vous pouvez implémenter la validation à l’aide de la méthode **Validate** dans du code personnalisé. Le moteur d’exécution valide une tâche en appelant la méthode **Validate** sur cette tâche. Il incombe au développeur de la tâche de définir les critères d'une validation de tâche réussie ou non réussie, et de notifier le moteur d'exécution du résultat de cette évaluation.  
  
#### <a name="task-abstract-base-class"></a>Classe de base abstraite d'une tâche  
 La classe de base abstraite [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) fournit la méthode **Validate** que chaque tâche remplace pour définir ses critères de validation. Le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] appelle automatiquement la méthode **Validate** à plusieurs reprises lors de la conception du package, puis fournit des signaux visuels à l’utilisateur lorsque des avertissements ou des erreurs se produisent afin de faciliter l’identification des problèmes liés à la configuration de la tâche. Les tâches fournissent les résultats de la validation en renvoyant une valeur de l'énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>, puis en déclenchant des événements d'avertissement et d'erreur. Ces événements contiennent des informations qui s'affichent pour l'utilisateur dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 Voici quelques exemples pour la validation :  
  
-   Un gestionnaire de connexions valide le nom de fichier spécifique.  
  
-   Un gestionnaire de connexions confirme que le type d'entrée correspond au type attendu, tel qu'un fichier XML.  
  
-   Une tâche qui attend une entrée de base de données vérifie qu'elle ne reçoit pas de données provenant d'une connexion autre qu'une connexion de base de données.  
  
-   Une tâche garantit qu'aucune de ses propriétés ne contredit l'autre jeu de propriétés sur la même tâche.  
  
-   Une tâche garantit que toutes les ressources requises utilisées par la tâche au moment de l'exécution sont disponibles.  
  
 Les performances sont un élément à prendre en compte pour déterminer ce qui est validé et ce qui ne l'est pas. Par exemple, l'entrée d'une tâche peut être une connexion sur un réseau dont la bande passante est faible ou le trafic encombré. La validation risque de nécessiter plusieurs secondes de traitement si vous décidez de valider la disponibilité de la ressource. Une autre validation peut provoquer un aller-retour vers un serveur très demandé et la routine de validation peut être lente. Bien qu'il existe de nombreuses propriétés et paramètres pouvant être validés, tout ne doit pas être validé.  
  
-   Le code inclus dans la méthode **Validate** est également appelé par <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> avant exécution de la tâche et <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> annule l’exécution si la validation échoue.  
  
#### <a name="user-interface-considerations-during-validation"></a>Considérations liées à l'interface utilisateur pendant la validation  
 La [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) comprend une interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> en tant que paramètre de la méthode **Validate**. L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> contient les méthodes appelées par la tâche afin de déclencher des événements pour le moteur d'exécution. Les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> sont appelées lorsqu'un avertissement ou une condition d'erreur se produisent pendant la validation. Ces deux méthodes d'avertissement requièrent les mêmes paramètres, qui incluent un code d'erreur, un composant source, une description, un fichier d'aide et des informations d'aide contextuelles. Le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utilise ces informations pour afficher des signaux visuels sur l'aire de conception. Les signaux visuels fournis par le concepteur incluent une icône d'exclamation qui apparaît en regard de la tâche sur l'aire du concepteur. Ce signal visuel indique à l'utilisateur que la tâche requiert une configuration supplémentaire pour que l'exécution puisse continuer.  
  
 L'icône d'exclamation affiche également une info-bulle qui contient un message d'erreur. Le message d'erreur est fourni par la tâche dans le paramètre de description de l'événement. Les messages d’erreur sont également affichés dans le volet **Liste des tâches** de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], lequel fournit à l’utilisateur un emplacement central pour consulter toutes les erreurs de validation.  
  
#### <a name="validation-example"></a>Exemple de validation  
 L’exemple de code suivant présente une tâche avec une propriété **UserName**. Cette propriété a été spécifiée comme requis pour que la validation réussisse. Si la propriété n'est pas définie, la tâche publie une erreur et retourne l'objet <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Failure> de l'énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>. La méthode **Validate** est encapsulée dans un bloc try/catch et fait échouer la validation si une exception se produit.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string userName = "";  
  
  public override DTSExecResult Validate(Connections connections,  
     VariableDispenser variableDispenser, IDTSComponentEvents events,  
     IDTSLogging log)  
  {  
    try  
    {  
      if (this.userName == "")  
      {  
        //   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0);  
        //   Fail validation.  
        return DTSExecResult.Failure;  
      }  
      //   Return success.  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string UserName  
  {  
    get  
    {  
      return this.userName;  
    }  
    set  
    {  
      this.userName = value;  
    }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _userName As String = ""  
  
  Public Overrides Function Validate(ByVal connections As Connections, _  
     ByVal variableDispenser As VariableDispenser, _  
     ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging) As DTSExecResult  
  
    Try  
      If Me._userName = "" Then  
        '   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0)  
        '   Fail validation.  
        Return DTSExecResult.Failure  
      End If  
      '   Return success.  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property UserName() As String  
    Get  
      Return Me._userName  
    End Get  
    Set(ByVal Value As String)  
      Me._userName = Value  
    End Set  
  End Property  
  
End Class  
```  
  
### <a name="persisting-the-task"></a>Persistance de la tâche  
 En règle générale, vous n'avez pas à implémenter une persistance personnalisée pour une tâche. Une persistance personnalisée est uniquement requise lorsque les propriétés d'un objet utilisent des types de données complexes. Pour plus d’informations, consultez [Développement d’objets personnalisés pour Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md).  
  
## <a name="executing-the-task"></a>Exécution de la tâche  
 Cette section décrit comment utiliser la méthode **Execute** qui est héritée et remplacée par des tâches. Elle explique également les différentes façons de fournir des informations concernant les résultats de l'exécution des tâches.  
  
### <a name="execute-method"></a>Méthode Execute  
 Tâches contenues dans l’exécution d’un package lorsque le runtime [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] appelle leur méthode **Execute**. Les tâches implémentent leurs logique métier et fonctionnalités principales dans cette méthode, puis fournissent les résultats de l’exécution en publiant des messages, en renvoyant une valeur de l’énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> et en remplaçant la propriété **get** de la propriété **ExecutionValue**.  
  
 La classe de base [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) fournit une implémentation par défaut de la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>. Les tâches personnalisées substituent cette méthode pour définir leurs fonctionnalités d'exécution. L'objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> encapsule la tâche, en l'isolant du moteur d'exécution et des autres objets compris dans le package. En raison de cette isolation, la tâche n'a pas connaissance de son emplacement dans le package pour ce qui est de son ordre d'exécution et elle s'exécute uniquement lorsqu'elle est appelée par le runtime. Cette architecture empêche certains problèmes qui peuvent se produire lorsque les tâches modifient le package pendant l'exécution. La tâche peut accéder aux autres objets compris dans le package uniquement via les objets qui lui sont fournis comme paramètres dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>. Ces paramètres permettent aux tâches de déclencher des événements, d'écrire des entrées dans le journal des événements, d'accéder à la collection de variables et d'inscrire des connexions aux sources de données dans les transactions, tout en maintenant quand même l'isolation nécessaire pour garantir la stabilité et la fiabilité du package.  
  
 Le tableau suivant répertorie les paramètres fournis à la tâche dans la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Connections>|Contient une collection d'objets <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> disponibles pour la tâche.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.VariableDispenser>|Contient les variables disponibles pour la tâche. Les tâches utilisent des variables via VariableDispenser ; les tâches n'utilisent pas des variables directement. Le distributeur de variables verrouille et déverrouille les variables, et empêche les blocages ou les remplacements.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>|Contient les méthodes appelées par la tâche pour déclencher des événements pour le moteur d'exécution.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSLogging>|Contient les méthodes et propriétés utilisées par la tâche pour écrire des entrées dans le journal des événements.|  
|Object|Contient l'objet de transaction dont le conteneur fait partie, le cas échéant. Cette valeur est transmise en tant que paramètre à la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> d'un objet <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>.|  
  
### <a name="providing-execution-feedback"></a>Envoi de commentaires d'exécution  
 Les tâches encapsulent leur code dans des blocs **try/catch** pour empêcher le déclenchement d’exceptions sur le moteur d’exécution. Cela permet de veiller à ce que le package finisse l'exécution et ne s'arrête pas de façon inattendue. Toutefois, le moteur d'exécution fournit d'autres mécanismes pour gérer les conditions d'erreur qui peuvent se produire pendant l'exécution d'une tâche. Ceux-ci incluent la publication de messages d'erreur et d'avertissement, le renvoi d'une valeur de la structure <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>, la publication de messages, le renvoi de la valeur <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> et la divulgation d'informations sur les résultats de l'exécution des tâches via la propriété <xref:Microsoft.SqlServer.Dts.Runtime.Task.ExecutionValue%2A>.  
  
 L'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> contient les méthodes <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A>, qui peuvent être appelées par la tâche pour publier des messages d'erreur et d'avertissement sur le moteur d'exécution. Ces deux méthodes requièrent des paramètres tels qu'un code d'erreur, un composant source, une description, un fichier d'aide et des informations d'aide contextuelles. Selon la configuration de la tâche, le runtime répond à ces messages en déclenchant des événements et des points d'arrêt, ou en écrivant des informations dans le journal des événements.  
  
 L'objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> fournit également la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> qui peut être utilisée pour fournir des informations supplémentaires à propos des résultats de l'exécution. Par exemple, si une tâche supprime des lignes d’une table dans le cadre de sa méthode **Execute**, elle peut retourner le nombre de lignes supprimées sous la forme de la valeur de la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A>. En outre, l'objet <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> fournit la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecValueVariable%2A>. Cette propriété permet à l'utilisateur de mapper la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> renvoyée à partir de la tâche vers toute variable visible pour la tâche. Il est alors possible d'utiliser la variable spécifiée pour établir des contraintes de précédence entre les tâches.  
  
### <a name="execution-example"></a>Exemple d'exécution  
 L’exemple de code suivant montre une implémentation de la méthode **Execute** et affiche une propriété **ExecutionValue** remplacée. La tâche supprime le fichier spécifié par la propriété **fileName** de la tâche. La tâche publie un avertissement si le fichier n’existe pas ou si la propriété **fileName** est une chaîne vide. La tâche renvoie une valeur **Boolean** dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> pour indiquer si le fichier a été supprimé.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string fileName = "";  
  private bool fileDeleted = false;  
  
  public override DTSExecResult Execute(Connections cons,  
     VariableDispenser vars, IDTSComponentEvents events,  
     IDTSLogging log, Object txn)  
  {  
    try  
    {  
      if (this.fileName == "")  
      {  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0);  
        this.fileDeleted = false;  
      }  
      else  
      {  
        if (System.IO.File.Exists(this.fileName))  
        {  
          System.IO.File.Delete(this.fileName);  
          this.fileDeleted = true;  
        }  
        else  
          this.fileDeleted = false;  
      }  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string FileName  
  {  
    get { return this.fileName; }  
    set { this.fileName = value; }  
  }  
  public override object ExecutionValue  
  {  
    get { return this.fileDeleted; }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _fileName As String = ""  
  Private _fileDeleted As Boolean = False  
  
  Public Overrides Function Execute(ByVal cons As Connections, _  
     ByVal vars As VariableDispenser, ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging, ByVal txn As Object) As DTSExecResult  
  
    Try  
      If Me._fileName = "" Then  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0)  
        Me._fileDeleted = False  
      Else  
        If System.IO.File.Exists(Me._fileName) Then  
          System.IO.File.Delete(Me._fileName)  
          Me._fileDeleted = True  
        Else  
          Me._fileDeleted = False  
        End If  
      End If  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property FileName() As String  
    Get  
      Return Me._fileName  
    End Get  
    Set(ByVal Value As String)  
      Me._fileName = Value  
    End Set  
  End Property  
  
  Public Overrides ReadOnly Property ExecutionValue() As Object  
    Get  
      Return Me._fileDeleted  
    End Get  
  End Property  
  
End Class  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Codage d’une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Développement d’une interface utilisateur pour une tâche personnalisée](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  

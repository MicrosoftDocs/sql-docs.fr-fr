---
description: Connexion de tâches par programme
title: Connexion de tâches par programmation | Microsoft Docs
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
- tasks [Integration Services], precedence constraints
- precedence constraints [Integration Services], connecting tasks
- constraints [Integration Services]
ms.assetid: 23668e88-cef4-4009-a9cf-38e607eab7a2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 554d6550e73b93e94d192cf66b1209aaa1b27292
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355065"
---
# <a name="connecting-tasks-programmatically"></a>Connexion de tâches par programme

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Une contrainte de précédence, représentée dans le modèle objet par la classe <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>, établit l'ordre dans lequel les objets <xref:Microsoft.SqlServer.Dts.Runtime.Executable> s'exécutent dans un package. La contrainte de précédence permet de rendre l'exécution des conteneurs et des tâches d'un package dépendante du résultat de l'exécution d'une tâche ou d'un conteneur précédent. Les contraintes de précédence sont établies entre des paires d'objets <xref:Microsoft.SqlServer.Dts.Runtime.Executable> en appelant la méthode <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints.Add%2A> de la collection <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints> sur l'objet conteneur. Après avoir créé une contrainte entre deux objets exécutables, vous devez définir la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> pour établir les critères d'exécution du deuxième objet exécutable défini dans la contrainte.  
  
 Vous pouvez utiliser une contrainte ou une expression dans une seule contrainte de précédence, en fonction de la valeur spécifiée pour la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.EvalOp%2A>, tel que décrit dans le tableau suivant :  
  
|Valeur de la propriété EvalOp|Description|  
|----------------------------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Constraint>|Spécifie que le résultat de l'exécution détermine l'exécution de la tâche ou du conteneur contraint. Affectez la valeur souhaitée de l'énumération <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> à la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> de <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Expression>|Spécifie que la valeur d'une expression détermine l'exécution de la tâche ou du conteneur contraint. Définissez la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionAndConstraint>|Spécifie que la contrainte doit produire un résultat et que l'expression doit prendre une valeur, pour que la tâche ou le conteneur contraint s'exécute. Définissez les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>, et attribuez la valeur **true** à la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A>.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionOrConstraint>|Spécifie que la contrainte doit produire un résultat ou que l'expression doit prendre une valeur, pour que la tâche ou le conteneur contraint s'exécute. Définissez les propriétés <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> et <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>, et attribuez la valeur **false** à la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A>.|  
  
 L'exemple de code suivant illustre l'ajout de deux tâches à un package. Un <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> est créé entre elles pour empêcher l'exécution de la deuxième tâche tant que la première n'a pas fini de s'exécuter.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
  
      // Add a File System task.  
      Executable eFileTask1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost1 = eFileTask1 as TaskHost;  
  
      // Add a second File System task.  
      Executable eFileTask2 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost2 = eFileTask2 as TaskHost;  
  
      // Put a precedence constraint between the tasks.  
      // Set the constraint to specify that the second File System task cannot run  
      // until the first File System task finishes.  
      PrecedenceConstraint pcFileTasks =   
        p.PrecedenceConstraints.Add((Executable)thFileHost1, (Executable)thFileHost2);  
      pcFileTasks.Value = DTSExecResult.Completion;  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task.  
    Dim eFileTask1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost1 As TaskHost = CType(eFileTask1, TaskHost)  
  
    ' Add a second File System task.  
    Dim eFileTask2 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost2 As TaskHost = CType(eFileTask2, TaskHost)  
  
    ' Put a precedence constraint between the tasks.  
    ' Set the constraint to specify that the second File System task cannot run  
    ' until the first File System task finishes.  
    Dim pcFileTasks As PrecedenceConstraint = _  
      p.PrecedenceConstraints.Add(CType(thFileHost1, Executable), CType(thFileHost2, Executable))  
    pcFileTasks.Value = DTSExecResult.Completion  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>Voir aussi  
 [Ajout de la tâche de flux de données par programmation](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
  

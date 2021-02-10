---
description: Commandes nommées
title: Commandes nommées | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- named commands [ADO]
- commands [ADO]
ms.assetid: 5a0ec8f9-5ba3-4f9f-b80d-2073aa049586
author: rothja
ms.author: jroth
ms.openlocfilehash: f93c158a5572286e61ceacdaf95c5a0cd2153242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037249"
---
# <a name="named-commands"></a>Commandes nommées
[La création et l’exécution d’une commande simple](./creating-and-executing-a-simple-command.md) montrent une façon d’exécuter une commande. Il existe une autre méthode : vous pouvez en faire une commande nommée, puis appeler cette commande nommée directement sur l’objet de **connexion** (affecté à la propriété **ActiveConnection** de l’objet **Command** ). L’attribution d’un nom à une commande implique l’attribution d’un nom à la propriété **Name** d’un objet **Command** . Par exemple,  
  
```  
objCmd.Name = "GetCustomers"  
objCmd.ActiveConnection = objConn  
objConn.GetCustomers objRs  
```  
  
 La commande nommée agit comme s’il s’agissait d’une « méthode personnalisée » sur l’objet de **connexion** . Le résultat de la commande est retourné en tant que paramètre de sortie de cette « méthode personnalisée ».  
  
 L’exemple suivant illustre cette fonctionnalité.  
  
```  
'BeginNamedCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objRs As New ADODB.Recordset  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
  
    objCmd.CommandText = "SELECT CustomerID, CompanyName FROM Customers"  
    objCmd.CommandType = adCmdText  
  
    'Name the command.  
    objCmd.Name = "GetCustomers"  
  
    objCmd.ActiveConnection = objConn  
  
    ' Execute using Command.Name from the Connection.  
    objConn.GetCustomers objRs  
  
    ' Display.  
    Do While Not objRs.EOF  
        Debug.Print objRs(0) & vbTab & objRs(1)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndNamedCmd  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](../../reference/ado-api/connection-object-ado.md)
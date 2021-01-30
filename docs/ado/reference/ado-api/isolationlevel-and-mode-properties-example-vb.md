---
description: IsolationLevel et mode, exemple de propriétés (VB)
title: IsolationLevel et mode, exemple de propriétés (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Mode property [ADO], Visual Basic example
- IsolationLevel property [ADO], Visual Basic example
ms.assetid: 3382fd41-0aa1-4091-97d3-624403111e07
author: rothja
ms.author: jroth
ms.openlocfilehash: ec4115d32920b3392196e9dc74d9dadf9cc2959d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170938"
---
# <a name="isolationlevel-and-mode-properties-example-vb"></a>IsolationLevel et mode, exemple de propriétés (VB)
Cet exemple utilise la propriété [mode](./mode-property-ado.md) pour ouvrir une connexion exclusive, et la propriété [IsolationLevel](./isolationlevel-property.md) pour ouvrir une transaction exécutée de manière isolée d’autres transactions.  
  
```  
'BeginIsolationLevelVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rstTitles As ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLTitles As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Mode = adModeShareExclusive  
    Cnxn.IsolationLevel = adXactIsolated  
    Cnxn.Open strCnxn  
  
     ' open Titles table  
    Set rstTitles = New ADODB.Recordset  
    strSQLTitles = "titles"  
    rstTitles.Open strSQLTitles, Cnxn, adOpenDynamic, adLockPessimistic, adCmdTable  
  
    Cnxn.BeginTrans  
  
    ' Display connection mode  
    If Cnxn.Mode = adModeShareExclusive Then  
        MsgBox "Connection mode is exclusive."  
    Else  
        MsgBox "Connection mode is not exclusive."  
    End If  
  
    ' Display isolation level  
    If Cnxn.IsolationLevel = adXactIsolated Then  
        MsgBox "Transaction is isolated."  
    Else  
        MsgBox "Transaction is not isolated."  
    End If  
  
    ' Change the type of psychology titles  
    Do Until rstTitles.EOF  
        If Trim(rstTitles!Type) = "psychology" Then  
            rstTitles!Type = "self_help"  
            rstTitles.Update  
        End If  
        rstTitles.MoveNext  
    Loop  
  
    ' Print current data in recordset  
    rstTitles.Requery  
    Do While Not rstTitles.EOF  
        Debug.Print rstTitles!Title & " - " & rstTitles!Type  
        rstTitles.MoveNext  
    Loop  
  
    ' clean up  
    rstTitles.Close  
    Cnxn.RollbackTrans  
    Cnxn.Close  
    Set rstTitles = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstTitles Is Nothing Then  
        If rstTitles.State = adStateOpen Then rstTitles.Close  
    End If  
    Set rstTitles = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then  
            Cnxn.RollbackTrans  
            Cnxn.Close  
        End If  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndIsolationLevelVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](./connection-object-ado.md)   
 [Propriété IsolationLevel](./isolationlevel-property.md)   
 [Mode, propriété (ADO)](./mode-property-ado.md)
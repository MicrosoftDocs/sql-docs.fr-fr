---
description: Delete, exemple de méthode (VB)
title: Delete, exemple de méthode (VB) | Microsoft Docs
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
- Delete method [ADO], Visual Basic example
ms.assetid: 0c80e71b-9e3f-4d05-ab2a-9e78798dad88
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a91cc67b25b3ff017d63e76fbfbc3528957427f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025562"
---
# <a name="delete-method-example-vb"></a>Delete, exemple de méthode (VB)
Cet exemple utilise la méthode [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md) pour supprimer un enregistrement spécifié d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
```  
'BeginDeleteVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim rstRoySched As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLRoySched As String  
  
    Dim strMsg As String  
    Dim strTitleID As String  
    Dim intLoRange As Integer  
    Dim intHiRange As Integer  
    Dim intRoyalty As Integer  
  
     ' open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' open RoySched table with cursor client-side  
    Set rstRoySched = New ADODB.Recordset  
    rstRoySched.CursorLocation = adUseClient  
    rstRoySched.CursorType = adOpenStatic  
    rstRoySched.LockType = adLockBatchOptimistic  
    rstRoySched.Open "SELECT * FROM roysched WHERE royalty = 20", strCnxn, , , adCmdText  
  
    ' Prompt for a record to delete  
    strMsg = "Before delete there are " & rstRoySched.RecordCount & _  
       " titles with 20 percent royalty:" & vbCr & vbCr  
  
    Do While Not rstRoySched.EOF  
       strMsg = strMsg & rstRoySched!title_id & vbCr  
       rstRoySched.MoveNext  
    Loop  
  
    strMsg = strMsg & vbCr & vbCr & "Enter the ID of a record to delete:"  
    strTitleID = UCase(InputBox(strMsg))  
  
    If strTitleID = "" Then  
        Err.Raise 1, , "You didn't enter any value for the record ID."  
    End If  
  
    ' Move to the record and save data so it can be restored  
    rstRoySched.Filter = "title_id = '" & strTitleID & "'"  
  
    If rstRoySched.RecordCount < 1 Then  
        Err.Raise 1, , "There is no record for the record ID you entered."  
    End If  
  
    intLoRange = rstRoySched!lorange  
    intHiRange = rstRoySched!hirange  
    intRoyalty = rstRoySched!royalty  
  
    ' Delete the record  
    rstRoySched.Delete  
    rstRoySched.UpdateBatch  
  
    ' Show the results  
    rstRoySched.Filter = adFilterNone  
    rstRoySched.Requery  
    strMsg = ""  
    strMsg = "After delete there are " & rstRoySched.RecordCount & _  
       " titles with 20 percent royalty:" & vbCr & vbCr  
    Do While Not rstRoySched.EOF  
        strMsg = strMsg & rstRoySched!title_id & vbCr  
        rstRoySched.MoveNext  
    Loop  
    MsgBox strMsg  
  
    ' Restore the data because this is a demonstration  
    rstRoySched.AddNew  
    rstRoySched!title_id = strTitleID  
    rstRoySched!lorange = intLoRange  
    rstRoySched!hirange = intHiRange  
    rstRoySched!royalty = intRoyalty  
    rstRoySched.UpdateBatch  
  
    ' clean up  
    rstRoySched.Close  
    Set rstRoySched = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstRoySched Is Nothing Then  
        If rstRoySched.State = adStateOpen Then rstRoySched.Close  
    End If  
    Set rstRoySched = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndDeleteVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Delete, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)

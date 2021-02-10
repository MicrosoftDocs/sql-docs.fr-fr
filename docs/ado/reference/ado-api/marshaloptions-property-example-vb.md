---
description: MarshalOptions, exemple de propriété (VB)
title: MarshalOptions, exemple de propriété (VB) | Microsoft Docs
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
- MarshalOptions property [ADO], Visual Basic example
ms.assetid: ae576b2c-65aa-4838-872a-85e618806dc8
author: rothja
ms.author: jroth
ms.openlocfilehash: 501c7db2fada266e414f9c6cb4527793950c2df0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041789"
---
# <a name="marshaloptions-property-example-vb"></a>MarshalOptions, exemple de propriété (VB)
Cet exemple utilise la propriété [MarshalOptions](./marshaloptions-property-ado.md) pour spécifier les lignes qui sont renvoyées au serveur (toutes les lignes ou uniquement les lignes modifiées).  
  
```  
'BeginMarshalOptionsVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLEmployees As String  
  
    Dim strOldFirst As String  
    Dim strOldLast As String  
    Dim strMessage As String  
    Dim strMarshalAll As String  
    Dim strMarshalModified As String  
  
    ' Open connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open strCnxn  
  
    ' open recordset with names from Employees table  
    ' and set some properties through object refs  
    Set rstEmployees = New ADODB.Recordset  
    rstEmployees.CursorType = adOpenKeyset  
    rstEmployees.LockType = adLockOptimistic  
    rstEmployees.CursorLocation = adUseClient  
  
    strSQLEmployees = "SELECT fname, lname FROM Employee ORDER BY lname"  
  
    rstEmployees.Open strSQLEmployees, Cnxn, , , adCmdText  
     ' empty properties have been set above  
  
     ' Store original data  
    strOldFirst = rstEmployees!fname  
    strOldLast = rstEmployees!lname  
  
    ' Change data in edit buffer  
    rstEmployees!fname = "Linda"  
    rstEmployees!lname = "Kobara"  
  
    ' Show contents of buffer and get user input  
    strMessage = "Edit in progress:" & vbCr & _  
        " Original data = " & strOldFirst & " " & _  
        strOldLast & vbCr & " Data in buffer = " & _  
        rstEmployees!fname & " " & rstEmployees!lname & vbCr & vbCr & _  
        "Use Update to replace the original data with " & _  
        "the buffered data in the Recordset?"  
    strMarshalAll = "Would you like to send all the rows " & _  
        "in the recordset back to the server?"  
    strMarshalModified = "Would you like to send only " & _  
        "modified rows back to the server?"  
  
    If MsgBox(strMessage, vbYesNo) = vbYes Then  
        If MsgBox(strMarshalAll, vbYesNo) = vbYes Then  
            rstEmployees.MarshalOptions = adMarshalAll  
            rstEmployees.Update  
        ElseIf MsgBox(strMarshalModified, vbYesNo) = vbYes Then  
            rstEmployees.MarshalOptions = adMarshalModifiedOnly  
            rstEmployees.Update  
        End If  
    End If  
  
    ' sShow the resulting data  
    MsgBox "Data in recordset = " & rstEmployees!fname & " " & _  
        rstEmployees!lname  
  
    ' restore original data because this is a demonstration  
    If Not (strOldFirst = rstEmployees!fname And _  
            strOldLast = rstEmployees!lname) Then  
        rstEmployees!fname = strOldFirst  
        rstEmployees!lname = strOldLast  
        rstEmployees.Update  
    End If  
  
    ' clean up  
    rstEmployees.Close  
    Cnxn.Close  
    Set rstEmployees = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndMarshalOptionsVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [MarshalOptions, propriété (ADO)](./marshaloptions-property-ado.md)   
 [MarshalOptionsEnum](./marshaloptionsenum.md)
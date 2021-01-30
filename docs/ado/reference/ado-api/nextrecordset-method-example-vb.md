---
description: NextRecordset, exemple de méthode (VB)
title: NextRecordset, exemple de méthode (VB) | Microsoft Docs
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
- NextRecordset method [ADO], Visual Basic example
ms.assetid: b14806da-80d9-4da4-bb87-f558b36a6ac0
author: rothja
ms.author: jroth
ms.openlocfilehash: af703b24bc3c1f2e94ccc5979ac9b7977cb087e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167050"
---
# <a name="nextrecordset-method-example-vb"></a>NextRecordset, exemple de méthode (VB)
Cet exemple utilise la méthode [NextRecordset](./nextrecordset-method-ado.md) pour afficher les données d’un Recordset qui utilise une instruction de commande composée composée de trois instructions **Select** distinctes.  
  
```  
'BeginNextRecordsetVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    ' connection and recordset variables  
    Dim rstCompound As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim SQLCompound As String  
  
    Dim intCount As Integer  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open compound recordset  
    Set rstCompound = New ADODB.Recordset  
    SQLCompound = "SELECT * FROM Authors; " & _  
        "SELECT * FROM stores; " & _  
        "SELECT * FROM jobs"  
    rstCompound.Open SQLCompound, Cnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
    ' Display results from each SELECT statement  
    intCount = 1  
    Do Until rstCompound Is Nothing  
        Debug.Print "Contents of recordset #" & intCount  
  
        Do Until rstCompound.EOF  
            Debug.Print , rstCompound.Fields(0), rstCompound.Fields(1)  
            rstCompound.MoveNext  
        Loop  
  
        Set rstCompound = rstCompound.NextRecordset  
        intCount = intCount + 1  
    Loop  
  
    ' clean up  
    Cnxn.Close  
    Set rstCompound = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstCompound Is Nothing Then  
        If rstCompound.State = adStateOpen Then rstCompound.Close  
    End If  
    Set rstCompound = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndNextRecordsetVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [NextRecordset, méthode (ADO)](./nextrecordset-method-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)
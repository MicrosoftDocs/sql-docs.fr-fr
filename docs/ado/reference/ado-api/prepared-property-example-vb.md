---
description: Prepared, exemple de propriété (VB)
title: Prepared, exemple de propriété (VB) | Microsoft Docs
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
- Prepared property [ADO], Visual Basic example
ms.assetid: e3a3db2d-7f73-4288-ad08-5468f251d610
author: rothja
ms.author: jroth
ms.openlocfilehash: 8a9ed4d31ca839dfeeaa313921230c2847171b4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166835"
---
# <a name="prepared-property-example-vb"></a>Prepared, exemple de propriété (VB)
Cet exemple illustre la propriété [préparée](./prepared-property-ado.md) en ouvrant deux objets de [commande](./command-object-ado.md) : l’un préparé et l’autre non préparé.  
  
```  
'BeginPreparedVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim cmd1 As ADODB.Command  
    Dim cmd2 As ADODB.Command  
  
    Dim strCnxn As String  
    Dim strCmd As String  
    Dim sngStart As Single  
    Dim sngEnd As Single  
    Dim sngNotPrepared As Single  
    Dim sngPrepared As Single  
    Dim intLoop As Integer  
  
    ' Open a connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open strCnxn  
  
    ' Create two command objects for the same  
    ' command - one prepared and one not prepared  
    strCmd = "SELECT title, type FROM Titles ORDER BY type"  
  
    Set cmd1 = New ADODB.Command  
    Set cmd1.ActiveConnection = Cnxn  
    cmd1.CommandText = strCmd  
  
    Set cmd2 = New ADODB.Command  
    Set cmd2.ActiveConnection = Cnxn  
    cmd2.CommandText = strCmd  
    cmd2.Prepared = True  
  
    ' Set a timer, then execute the unprepared  
    ' command 20 times  
    sngStart = Timer  
    For intLoop = 1 To 20  
        cmd1.Execute  
    Next intLoop  
    sngEnd = Timer  
    sngNotPrepared = sngEnd - sngStart  
  
    ' Reset the timer, then execute the prepared  
    ' command 20 times  
    sngStart = Timer  
    For intLoop = 1 To 20  
        cmd2.Execute  
    Next intLoop  
    sngEnd = Timer  
    sngPrepared = sngEnd - sngStart  
  
    ' Display performance results  
    MsgBox "Performance Results:" & vbCr & _  
        "   Not Prepared: " & Format(sngNotPrepared, _  
        "##0.000") & " seconds" & vbCr & _  
        "   Prepared: " & Format(sngPrepared, _  
        "##0.000") & " seconds"  
  
    ' clean up  
    Cnxn.Close  
    Set Cnxn = Nothing  
    Set cmd1 = Nothing  
    Set cmd2 = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    Set cmd1 = Nothing  
    Set cmd2 = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndPreparedVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Command, objet (ADO)](./command-object-ado.md)   
 [Prepared, propriété (ADO)](./prepared-property-ado.md)
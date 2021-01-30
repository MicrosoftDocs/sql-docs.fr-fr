---
description: Append et CreateParameter, exemple de méthodes (VB)
title: Append et CreateParameter, exemple de méthodes (VB) | Microsoft Docs
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
- CreateParameter method [ADO], Visual Basic example
- Append method [ADO], Visual Basic example
ms.assetid: 46908cbd-434f-43e7-a794-ed0be0e0c0a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 80a1e86b9cd6beb54133ef796a560e5cdccbb177
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164840"
---
# <a name="append-and-createparameter-methods-example-vb"></a>Append et CreateParameter, exemple de méthodes (VB)
Cet exemple utilise les méthodes [Append](./append-method-ado.md) et [CreateParameter](./createparameter-method-ado.md) pour exécuter une procédure stockée avec un paramètre d’entrée.  
  
```  
'BeginAppendVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset, command and connection variables  
    Dim Cnxn As ADODB.Connection  
    Dim cmdByRoyalty As ADODB.Command  
    Dim prmByRoyalty As ADODB.Parameter  
    Dim rstByRoyalty As ADODB.Recordset  
    Dim rstAuthors As ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
    Dim strSQLByRoyalty As String  
     'record variables  
    Dim intRoyalty As Integer  
    Dim strAuthorID As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open command object with one parameter  
    Set cmdByRoyalty = New ADODB.Command  
    cmdByRoyalty.CommandText = "byroyalty"  
    cmdByRoyalty.CommandType = adCmdStoredProc  
  
    ' Get parameter value and append parameter  
    intRoyalty = Trim(InputBox("Enter royalty:"))  
    Set prmByRoyalty = cmdByRoyalty.CreateParameter("percentage", adInteger, adParamInput)  
    cmdByRoyalty.Parameters.Append prmByRoyalty  
    prmByRoyalty.Value = intRoyalty  
  
    ' Create recordset by executing the command  
    Set cmdByRoyalty.ActiveConnection = Cnxn  
    Set rstByRoyalty = cmdByRoyalty.Execute  
  
    ' Open the Authors Table to get author names for display  
    ' and set cursor client-side  
    Set rstAuthors = New ADODB.Recordset  
    strSQLAuthors = "Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adUseClient, adLockOptimistic, adCmdTable  
  
    ' Print recordset adding author names from Authors table  
    Debug.Print "Authors with " & intRoyalty & " percent royalty"  
  
    Do Until rstByRoyalty.EOF  
        strAuthorID = rstByRoyalty!au_id  
        Debug.Print "   " & rstByRoyalty!au_id & ", ";  
        rstAuthors.Filter = "au_id = '" & strAuthorID & "'"  
        Debug.Print rstAuthors!au_fname & " " & rstAuthors!au_lname  
        rstByRoyalty.MoveNext  
    Loop  
  
    ' clean up  
    rstByRoyalty.Close  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstByRoyalty = Nothing  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstByRoyalty Is Nothing Then  
        If rstByRoyalty.State = adStateOpen Then rstByRoyalty.Close  
    End If  
    Set rstByRoyalty = Nothing  
  
    If Not rstAuthors Is Nothing Then  
        If rstAuthors.State = adStateOpen Then rstAuthors.Close  
    End If  
    Set rstAuthors = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndAppendVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (ADO)](./append-method-ado.md)   
 [CreateParameter, méthode (ADO)](./createparameter-method-ado.md)   
 [Field, objet](./field-object.md)   
 [Fields, collection (ADO)](./fields-collection-ado.md)   
 [Objet Parameter](./parameter-object.md)
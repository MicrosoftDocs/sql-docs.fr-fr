---
description: Sort, exemple de propriétés (VB)
title: Sort, exemple de propriété (VB) | Microsoft Docs
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
- Sort property [ADO], Visual Basic example
ms.assetid: fc2fd40b-65d6-4023-90a3-90c9a88ef6cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 17ea6f0a278a9df688a27db338f566c4e5be6b1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170254"
---
# <a name="sort-property-example-vb"></a>Sort, exemple de propriétés (VB)
Cet exemple utilise la propriété [sort](./sort-property.md) de l’objet [Recordset](./recordset-object-ado.md) pour réorganiser les lignes d' **un jeu d’enregistrements** dérivé de la table **_Authors_* _ de la base de données _*_pubs_*_ . Une routine utilitaire secondaire imprime chaque ligne.  
  
```  
'BeginSortVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
     ' connection and recordset variables  
    Dim Cnxn As New ADODB.Connection  
    Dim rstAuthors As New ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
  
    Dim strTitle As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' open client-side recordset to enable sort method  
    Set rstAuthors = New ADODB.Recordset  
    rstAuthors.CursorLocation = adUseClient  
    strSQLAuthors = "SELECT _ FROM Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
     ' sort the recordset last name ascending  
    rstAuthors.Sort = "au_lname ASC, au_fname ASC"  
     ' show output  
    Debug.Print "Last Name Ascending:"  
    Debug.Print "First Name  Last Name" & vbCr  
  
    rstAuthors.MoveFirst  
    Do Until rstAuthors.EOF  
        Debug.Print rstAuthors!au_fname & " " & rstAuthors!au_lname  
        rstAuthors.MoveNext  
    Loop  
  
     ' sort the recordset last name descending  
    rstAuthors.Sort = "au_lname DESC, au_fname ASC"  
     ' show output  
    Debug.Print "Last Name Descending"  
    Debug.Print "First Name  Last Name" & vbCr  
  
    Do Until rstAuthors.EOF  
        Debug.Print rstAuthors!au_fname & " " & rstAuthors!au_lname  
        rstAuthors.MoveNext  
    Loop  
  
    ' clean up  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
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
'EndSortVB  
```  
  
 Il s’agit de la routine d’utilitaire secondaire qui imprime le titre donné et le contenu du **jeu d’enregistrements** spécifié.  
  
```  
Attribute VB_Name = "Sort"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recordset, objet (ADO)](./recordset-object-ado.md)   
 [Sort, propriété](./sort-property.md)
---
description: Type, exemple de propriété (objet Field) (VB)
title: Type, exemple de propriété (Field) (VB) | Microsoft Docs
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
- Type property [field] [ADO], Visual Basic example
ms.assetid: accb72f5-a3bd-4a7e-92b6-6da0783b4b75
author: rothja
ms.author: jroth
ms.openlocfilehash: a5cbe8963153887a4028474170f5039707c6794a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056424"
---
# <a name="type-property-example-field-vb"></a>Type, exemple de propriété (objet Field) (VB)
Cet exemple illustre la propriété [type](./type-property-ado.md) en affichant le nom de la constante qui correspond à la valeur de la propriété [type](./type-property-ado.md) de tous les objets [Field](./field-object.md) de la table ***Employees*** . La fonction FieldType est requise pour l’exécution de cette procédure.  
  
```  
'BeginTypeFieldVB  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
    Dim Cnxn As ADODB.Connection  
    Dim rstEmployees As ADODB.Recordset  
    Dim fld As ADODB.Field  
    Dim strCnxn As String  
    Dim strSQLEmployee As String  
    Dim FieldType As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open recordset with data from Employees table  
    Set rstEmployees = New ADODB.Recordset  
    strSQLEmployee = "employee"  
    rstEmployees.Open strSQLEmployee, Cnxn, , , adCmdTable  
    'rstEmployees.Open strSQLEmployee, Cnxn, adOpenStatic, adLockReadOnly, adCmdTable  
     ' the above two lines of code are identical  
  
    Debug.Print "Fields in Employees Table:" & vbCr  
  
    ' Enumerate Fields collection of Employees table  
    For Each fld In rstEmployees.Fields  
        ' translate field-type code to text  
        Select Case fld.Type  
            Case adChar  
               FieldType = "adChar"  
            Case adVarChar  
               FieldType = "adVarChar"  
            Case adSmallInt  
               FieldType = "adSmallInt"  
            Case adUnsignedTinyInt  
               FieldType = "adUnsignedTinyInt"  
            Case adDBTimeStamp  
               FieldType = "adDBTimeStamp"  
        End Select  
        ' show results  
        Debug.Print "  Name: " & fld.Name & vbCr & _  
          "  Type: " & FieldType & vbCr  
    Next fld  
  
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
  
    Set fld = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndTypeFieldVB  
  
Attribute VB_Name = "TypeField"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Field, objet](./field-object.md)   
 [Type, propriété (ADO)](./type-property-ado.md)
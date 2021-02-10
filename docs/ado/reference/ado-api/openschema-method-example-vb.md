---
description: OpenSchema, exemple de méthode (VB)
title: OpenSchema, exemple de méthode (VB) | Microsoft Docs
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
- OpenSchema method [ADO], Visual Basic example
ms.assetid: 455a02f0-8143-4562-8648-8fb45ffd334c
author: rothja
ms.author: jroth
ms.openlocfilehash: 185a0df6347748d71ec621940250e682e4c0eca6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041349"
---
# <a name="openschema-method-example-vb"></a>OpenSchema, exemple de méthode (VB)
Cet exemple utilise la méthode [OpenSchema](./openschema-method.md) pour afficher le nom et le type de chaque table dans la base de données ***pubs*** .  
  
```  
'BeginOpenSchemaVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rstSchema As ADODB.Recordset  
    Dim strCnxn As String  
  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    Set rstSchema = Cnxn.OpenSchema(adSchemaTables)  
  
    Do Until rstSchema.EOF  
        Debug.Print "Table name: " & _  
            rstSchema!TABLE_NAME & vbCr & _  
            "Table type: " & rstSchema!TABLE_TYPE & vbCr  
        rstSchema.MoveNext  
    Loop  
  
    ' clean up  
    rstSchema.Close  
    Cnxn.Close  
    Set rstSchema = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstSchema Is Nothing Then  
        If rstSchema.State = adStateOpen Then rstSchema.Close  
    End If  
    Set rstSchema = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndOpenSchemaVB  
```  
  
 Cet exemple spécifie une contrainte de requête TABLE_TYPE **dans** l' **argument _Criteria Criteria Criteria_*. Par conséquent, seules les informations de schéma pour les vues spécifiées dans la* base de données _ _pubs_** sont retournées. L’exemple affiche ensuite le (s) nom (s) et le (s) type (s) de chaque table (s).  
  
```  
Attribute VB_Name = "OpenSchema"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OpenSchema, méthode](./openschema-method.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)
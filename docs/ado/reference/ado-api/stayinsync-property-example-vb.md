---
description: StayInSync, exemple de propriété (VB)
title: StayInSync, exemple de propriété (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- StayInSync property [ADO], Visual Basic example
ms.assetid: b682bcc3-04b3-42b0-86f4-c17e0cd29baf
author: rothja
ms.author: jroth
ms.openlocfilehash: 13e1026b95386cb051aba0468d371937c4ed52d6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441911"
---
# <a name="stayinsync-property-example-vb"></a>StayInSync, exemple de propriété (VB)
Cet exemple montre comment la propriété [StayInSync](../../../ado/reference/ado-api/stayinsync-property.md) facilite l’accès aux lignes d’un [jeu d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md)hiérarchique.  
  
 La boucle externe affiche le prénom et le nom, l’État et l’identification de chaque auteur. Le **Recordset** ajouté pour chaque ligne est extrait de la collection de [champs](../../../ado/reference/ado-api/fields-collection-ado.md) et affecté automatiquement à **rstTitleAuthor** par la propriété **StayInSync** chaque fois que le **jeu d’enregistrements** parent passe à une nouvelle ligne. La boucle interne affiche quatre champs de chaque ligne dans le Recordset ajouté.  
  
```  
'BeginStayInSyncVB  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rst As ADODB.Recordset  
    Dim rstTitleAuthor As New ADODB.Recordset  
    Dim strCnxn As String  
  
    ' Open the connection with Data Shape attributes  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider=MSDataShape;Data Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Create a recordset  
    Set rst = New ADODB.Recordset  
    rst.StayInSync = True  
    rst.Open "SHAPE {select * from Authors} " & _  
                   "APPEND ( { select * from titleauthor} AS chapTitleAuthor " & _  
                   "RELATE au_id TO au_id) ", Cnxn, , , adCmdText  
  
    Set rstTitleAuthor = rst("chapTitleAuthor").Value  
    Do Until rst.EOF  
        Debug.Print rst!au_fname & " " & rst!au_lname & " " & _  
                   rst!State & " " & rst!au_id  
  
        Do Until rstTitleAuthor.EOF  
            Debug.Print rstTitleAuthor(0) & " " & rstTitleAuthor(1) & " " & _  
                   rstTitleAuthor(2) & " " & rstTitleAuthor(3)  
            rstTitleAuthor.MoveNext  
        Loop  
  
        rst.MoveNext  
    Loop  
  
    ' Clean up  
    rst.Close  
    Cnxn.Close  
    Set rst = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' Clean up  
    If Not rst Is Nothing Then  
        If rst.State = adStateOpen Then rst.Close  
    End If  
    Set rst = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndStayInSyncVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [StayInSync, propriété](../../../ado/reference/ado-api/stayinsync-property.md)

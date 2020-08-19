---
description: ParentCatalog, exemple de propriété (VB)
title: ParentCatalog, exemple de propriété (VB) | Microsoft Docs
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
- ParentCatalog property [ADOX], Visual Basic example
ms.assetid: 448bc850-7584-4c5f-89f3-5f4fee88b259
author: rothja
ms.author: jroth
ms.openlocfilehash: c4093cfbd1ca5372d86fd07b6fc7716d484c4f51
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88439711"
---
# <a name="parentcatalog-property-example-vb"></a>ParentCatalog, exemple de propriété (VB)
Le code suivant montre comment utiliser la propriété [ParentCatalog](../../../ado/reference/adox-api/parentcatalog-property-adox.md) pour accéder à une propriété spécifique au fournisseur avant d’ajouter une table à un catalogue. La propriété est **AutoIncrement**, ce qui crée un champ AUTOINCREMENT dans une base de données Microsoft Jet.  
  
```  
' BeginCreateAutoIncrColumnVB  
Sub Main()  
    On Error GoTo CreateAutoIncrColumnError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As New ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
  
    With tbl  
        .Name = "MyContacts"  
        Set .ParentCatalog = cat  
        ' Create fields and append them to the new Table object.  
        .Columns.Append "ContactId", adInteger  
        ' Make the ContactId column and auto incrementing column  
        .Columns("ContactId").Properties("AutoIncrement") = True  
        .Columns.Append "CustomerID", adVarWChar  
        .Columns.Append "FirstName", adVarWChar  
        .Columns.Append "LastName", adVarWChar  
        .Columns.Append "Phone", adVarWChar, 20  
        .Columns.Append "Notes", adLongVarWChar  
    End With  
  
    cat.Tables.Append tbl  
    Debug.Print "Table 'MyContacts' is added."  
  
    ' Delete the table as this is a demonstration.  
    cat.Tables.Delete tbl.Name  
    Debug.Print "Table 'MyContacts' is delete."  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set tbl = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CreateAutoIncrColumnError:  
  
    Set cat = Nothing  
    Set tbl = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateAutoIncrColumnVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (colonnes ADOX)](../../../ado/reference/adox-api/append-method-adox-columns.md)   
 [Append, méthode (Tables ADOX)](../../../ado/reference/adox-api/append-method-adox-tables.md)   
 [Catalog, objet (ADOX)](../../../ado/reference/adox-api/catalog-object-adox.md)   
 [Column, objet (ADOX)](../../../ado/reference/adox-api/column-object-adox.md)   
 [Columns, collection (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md)   
 [Name, propriété (ADOX)](../../../ado/reference/adox-api/name-property-adox.md)   
 [ParentCatalog, propriété (ADOX)](../../../ado/reference/adox-api/parentcatalog-property-adox.md)   
 [Table, objet (ADOX)](../../../ado/reference/adox-api/table-object-adox.md)   
 [Type, propriété (colonne) (ADOX)](../../../ado/reference/adox-api/type-property-column-adox.md)

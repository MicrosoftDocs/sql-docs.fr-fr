---
description: Append (méthode) sur la collection Keys, Type de clé ; RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)
title: Créer une relation de clé étrangère entre des tables, exemple (VB) | Microsoft Docs
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
- Key Type property [ADOX], Visual Basic example
- RelatedTable property [ADOX], Visual Basic example
- Keys Append method [ADOX], Visual Basic example
- UpdateRule property [ADOX], Visual Basic example
- RelatedColumn property [ADOX], Visual Basic example
ms.assetid: 13b5b1c3-6af6-439e-bb65-976578ba6bc2
author: rothja
ms.author: jroth
ms.openlocfilehash: a09e3eca2bec72ec61a9811133a65f18af1b7893
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054064"
---
# <a name="keys-append-method-key-type-relatedcolumn-relatedtable-and-updaterule-properties-example-vb"></a>Append (méthode) sur la collection Keys, Type de clé ; RelatedColumn, RelatedTable et UpdateRule, exemple de propriétés (VB)
Le code suivant montre comment créer une relation de clé étrangère entre deux tables existantes nommées **Customers** et **Orders**.  
  
```  
' BeginCreateKeyVB  
Sub Main()  
    On Error GoTo CreateKeyError  
  
    Dim kyForeign As New ADOX.Key  
    Dim cat As New ADOX.Catalog  
  
    ' Connect to the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Define the foreign key.  
    kyForeign.Name = "CustOrder"  
    kyForeign.Type = adKeyForeign  
    kyForeign.RelatedTable = "Customers"  
    kyForeign.Columns.Append "CustomerId"  
    kyForeign.Columns("CustomerId").RelatedColumn = "CustomerId"  
    kyForeign.UpdateRule = adRICascade  
  
    ' Append the foreign key to the keys collection.  
    cat.Tables("Orders").Keys.Append kyForeign  
  
    'Delete the key t demonstrate the Delete method.  
    cat.Tables("Orders").Keys.Delete kyForeign.Name  
  
    'Clean up.  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set kyForeign = Nothing  
    Exit Sub  
  
CreateKeyError:  
    Set cat = Nothing  
    Set kyForeign = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateKeyVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (clés ADOX)](./append-method-adox-keys.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [Column, objet (ADOX)](./column-object-adox.md)   
 [Columns, collection (ADOX)](./columns-collection-adox.md)   
 [Key, objet (ADOX)](./key-object-adox.md)   
 [Keys, collection (ADOX)](./keys-collection-adox.md)   
 [Name, propriété (ADOX)](./name-property-adox.md)   
 [RelatedColumn, propriété (ADOX)](./relatedcolumn-property-adox.md)   
 [RelatedTable, propriété (ADOX)](./relatedtable-property-adox.md)   
 [Table, objet (ADOX)](./table-object-adox.md)   
 [Tables, collection (ADOX)](./tables-collection-adox.md)   
 [Type, propriété (Key) (ADOX)](./type-property-key-adox.md)   
 [UpdateRule, propriété (ADOX)](./updaterule-property-adox.md)
---
description: Close, méthode de l’objet Connection, Type (exemple de propriété de l’objet Table) (VB)
title: Connection Close, méthode, table type, exemple de propriété (VB) | Microsoft Docs
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
- Close method [ADOX], Visual Basic example
- Type property [ADOX], Visual Basic example
ms.assetid: f88e7a3b-19ed-46e2-b2ce-3b611d9b8166
author: rothja
ms.author: jroth
ms.openlocfilehash: 00b3758db44e89d4c14b3a57bea8a7e7e3bdd158
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054334"
---
# <a name="connection-close-method-table-type-property-example-vb"></a>Close, méthode de l’objet Connection, Type (exemple de propriété de l’objet Table) (VB)
L’affectation de la valeur **Nothing** à la propriété [ActiveConnection](./activeconnection-property-adox.md) doit fermer la connexion au catalogue. Les regroupements associés seront vides. Tous les objets qui ont été créés à partir d’objets de schéma dans le catalogue sont orphelins. Toutes les propriétés de ces objets qui ont été mises en cache seront toujours disponibles, mais une tentative de lecture des propriétés nécessitant un appel au fournisseur échouera.  
  
```  
' BeginCloseConnectionVB  
Sub Main()  
    On Error GoTo CloseConnectionByNothingError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Set tbl = cat.Tables(0)  
    Debug.Print tbl.Type    ' Cache tbl.Type info  
    Set cat.ActiveConnection = Nothing  
    Debug.Print tbl.Type    ' tbl is orphaned  
    ' Previous line will succeed if this info was cached.  
    Debug.Print tbl.Columns(0).DefinedSize  
    ' Previous line will fail if this info has not been cached.  
  
    'Clean up.  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CloseConnectionByNothingError:  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndCloseConnectionVB  
```  
  
 La fermeture d’un objet de [connexion](../ado-api/connection-object-ado.md) qui a été utilisé pour ouvrir le catalogue doit avoir le même effet que l’affectation de la valeur **Nothing** à la propriété **ActiveConnection** .  
  
```  
Attribute VB_Name = "Connection"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, propriété (ADOX)](./activeconnection-property-adox.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [Column, objet (ADOX)](./column-object-adox.md)   
 [Columns, collection (ADOX)](./columns-collection-adox.md)   
 [Table, objet (ADOX)](./table-object-adox.md)   
 [Tables, collection (ADOX)](./tables-collection-adox.md)   
 [Type, propriété (table) (ADOX)](./type-property-table-adox.md)
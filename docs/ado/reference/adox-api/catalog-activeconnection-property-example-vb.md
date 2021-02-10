---
description: Exemple de propriété ActiveConnection de l’objet Catalog (VB)
title: Exemple de propriété ActiveConnection de Catalog (VB) | Microsoft Docs
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
- ActiveConnection property [ADOX], Visual Basic example
ms.assetid: bb3274b1-764d-43a7-a49f-ef55680ecd26
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ccc2a40bfde1ac4a44ed0c415cea0f90b92e31d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050350"
---
# <a name="catalog-activeconnection-property-example-vb"></a>Exemple de propriété ActiveConnection de l’objet Catalog (VB)
La définition de la propriété [ActiveConnection](./activeconnection-property-adox.md) sur une connexion ouverte valide « ouvre » le catalogue. À partir d’un catalogue ouvert, vous pouvez accéder aux objets de schéma contenus dans ce catalogue.  
  
```  
' BeginOpenConnectionVB  
Sub Main()  
    On Error GoTo OpenConnectionError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Debug.Print cat.Tables(0).Type  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
OpenConnectionError:  
  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndOpenConnectionVB  
```  
  
 Si vous définissez la propriété **ActiveConnection** sur une chaîne de connexion valide, le catalogue est également « ouvert ».  
  
```  
Attribute VB_Name = "Catalog"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, propriété (ADOX)](./activeconnection-property-adox.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [Table, objet (ADOX)](./table-object-adox.md)   
 [Tables, collection (ADOX)](./tables-collection-adox.md)   
 [Type, propriété (table) (ADOX)](./type-property-table-adox.md)
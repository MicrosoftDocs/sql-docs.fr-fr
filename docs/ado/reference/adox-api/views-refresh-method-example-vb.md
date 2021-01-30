---
description: Views, exemple de méthode Refresh (VB)
title: Views, exemple de méthode Refresh (VB) | Microsoft Docs
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
- Refresh method [ADOX]
ms.assetid: cdad2d66-6ade-40dc-9e74-e40cfa9bc127
author: rothja
ms.author: jroth
ms.openlocfilehash: 70795cf5c645ba3eaf9d6c869a067b02fad9d0c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169079"
---
# <a name="views-refresh-method-example-vb"></a>Views, exemple de méthode Refresh (VB)
Le code suivant montre comment actualiser la collection [views](./views-collection-adox.md) d’un [catalogue](./catalog-object-adox.md). Cela est nécessaire pour pouvoir accéder aux objets de [vue](./view-object-adox.md) du **catalogue** .  
  
```  
' BeginViewsRefreshVB  
Sub Main()  
    On Error GoTo ProcedureViewsRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection.  
    cat.Views.Refresh  
  
    'Clean up  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureViewsRefreshError:  
  
    If Not cat Is Nothing Then  
        Set cat = Nothing  
    End If  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndViewsRefreshVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Refresh, méthode (ADO)](../ado-api/refresh-method-ado.md)   
 [Views, collection (ADOX)](./views-collection-adox.md)
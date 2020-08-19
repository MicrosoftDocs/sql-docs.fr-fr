---
description: IndexNulls, exemple de propriété (VB)
title: IndexNulls, exemple de propriété (VB) | Microsoft Docs
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
- IndexNulls property [ADOX], Visual Basic example
ms.assetid: 45204669-32c0-4690-aab9-ddf0fd71ae48
author: rothja
ms.author: jroth
ms.openlocfilehash: 527e510979afd81a5e90ea207027479cde5c5f63
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88439881"
---
# <a name="indexnulls-property-example-vb"></a>IndexNulls, exemple de propriété (VB)
Cet exemple illustre la propriété [IndexNulls](../../../ado/reference/adox-api/indexnulls-property-adox.md) d’un [index](../../../ado/reference/adox-api/index-object-adox.md). Le code crée un nouvel index et définit la valeur de **IndexNulls** en fonction de l’entrée utilisateur (à partir d’une zone de liste nommée list1). Ensuite, l' **index** est ajouté à la table **Employees** [Table](../../../ado/reference/adox-api/table-object-adox.md) dans le *Northwind* [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md)Northwind. Le nouvel **index** est appliqué à un [jeu d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md) basé sur la table **Employees** et le **Recordset** est ouvert. Un nouvel enregistrement est ajouté à la table **Employees** , avec une valeur **null** dans le champ indexé. Le fait que ce nouvel enregistrement s’affiche dépend du paramètre de la propriété **IndexNulls** .  
  
```  
' BeginIndexNullsVB  
Private Sub cmdIndexNulls_Click()  
    IndexNullsX  
End Sub  
  
Sub IndexNullsX()  
  
    Dim cnn As New ADODB.Connection  
    Dim catNorthwind As New ADOX.Catalog  
    Dim idxNew As New ADOX.Index  
    Dim rstEmployees As New ADODB.Recordset  
    Dim varBookmark As Variant  
  
    ' Connect the catalog.  
    cnn.Open "Provider=Microsoft.Jet.OLEDB.4.0;" & _  
        "data source=c:\Program Files\" & _  
        "Microsoft Office\Office\Samples\Northwind.mdb;"  
  
    Set catNorthwind.ActiveConnection = cnn  
  
    ' Append Country column to new index  
    idxNew.Columns.Append "Country"  
    idxNew.Name = "NewIndex"  
  
    ' Set IndexNulls based on user selection in listbox List1  
    Select Case List1.List(List1.ListIndex)  
        Case "Allow"  
            idxNew.IndexNulls = adIndexNullsAllow  
        Case "Ignore"  
            idxNew.IndexNulls = adIndexNullsIgnore  
        Case Else  
            End  
    End Select  
  
    'Append new index to Employees table  
    catNorthwind.Tables("Employees").Indexes.Append idxNew  
  
    rstEmployees.Index = idxNew.Name  
    rstEmployees.Open "Employees", cnn, adOpenKeyset, _  
        adLockOptimistic, adCmdTableDirect  
  
    With rstEmployees  
        ' Add a new record to the Employees table.  
        .AddNew  
        !FirstName = "Gary"  
        !LastName = "Haarsager"  
        .Update  
  
        ' Bookmark the newly added record  
        varBookmark = .Bookmark  
  
        ' Use the new index to set the order of the records.  
        .MoveFirst  
  
        Debug.Print "Index = " & .Index & _  
            ", IndexNulls = " & idxNew.IndexNulls  
        Debug.Print "  Country - Name"  
  
        ' Enumerate the Recordset. The value of the  
        ' IndexNulls property will determine if the newly  
        ' added record appears in the output.  
        Do While Not .EOF  
            Debug.Print "    " & _  
                IIf(IsNull(!Country), "[Null]", !Country) & _  
                " - " & !FirstName & " " & !LastName  
            .MoveNext  
        Loop  
  
        ' Delete new record because this is a demonstration.  
        .Bookmark = varBookmark  
        .Delete  
  
        .Close  
    End With  
  
    ' Delete new Index because this is a demonstration.  
    catNorthwind.Tables("Employees").Indexes.Delete idxNew.Name  
    Set catNorthwind = Nothing  
  
End Sub  
' EndIndexNullsVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Index, objet (ADOX)](../../../ado/reference/adox-api/index-object-adox.md)   
 [IndexNulls, propriété (ADOX)](../../../ado/reference/adox-api/indexnulls-property-adox.md)

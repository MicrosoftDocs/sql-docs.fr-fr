---
description: Append (méthode) sur les collections Groups et Users, ChangePassword, exemple de méthodes (VB)
title: Groups and Users Append, ChangePassword, exemple de méthodes (VB) | Microsoft Docs
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
- ChangePassword method [ADOX], Visual Basic example
- Append method [ADOX], Visual Basic example
ms.assetid: c9426757-9cdd-4a95-b506-d3d011569109
author: rothja
ms.author: jroth
ms.openlocfilehash: 6864d99294ebd68b07c41b69bca6e18f479a4935
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164233"
---
# <a name="groups-and-users-append-changepassword-methods-example-vb"></a>Append (méthode) sur les collections Groups et Users, ChangePassword, exemple de méthodes (VB)
Cet exemple illustre la méthode [Append](./append-method-adox-groups.md) des [groupes](./groups-collection-adox.md), ainsi que la méthode [Append](./append-method-adox-users.md) des [utilisateurs](./users-collection-adox.md) en ajoutant un nouveau [groupe](./group-object-adox.md) et un nouvel [utilisateur](./user-object-adox.md) au système. Le nouveau **groupe** est ajouté à la collection de **groupes** du nouvel **utilisateur**. Par conséquent, le nouvel **utilisateur** est ajouté au **groupe**. En outre, la méthode [ChangePassword](./changepassword-method-adox.md) est utilisée pour spécifier le mot de passe de l' **utilisateur** .  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.  
  
```  
' BeginGroupVB  
Sub Main()  
    On Error GoTo GroupXError  
  
    Dim cat As ADOX.Catalog  
    Dim usrNew As ADOX.User  
    Dim usrLoop As ADOX.User  
    Dim grpLoop As ADOX.Group  
  
    Set cat = New ADOX.Catalog  
  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';" & _  
        "jet oledb:system database=" & _  
        "'system.mdw'"  
  
    With cat  
        'Create and append new group with a string.  
        .Groups.Append "Accounting"  
  
        ' Create and append new user with an object.  
        Set usrNew = New ADOX.User  
        usrNew.Name = "Pat Smith"  
        usrNew.ChangePassword "", "Password1"  
        .Users.Append usrNew  
  
        ' Make the user Pat Smith a member of the  
        ' Accounting group by creating and adding the  
        ' appropriate Group object to the user's Groups  
        ' collection. The same is accomplished if a User  
        ' object representing Pat Smith is created and  
        ' appended to the Accounting group Users collection  
        usrNew.Groups.Append "Accounting"  
  
        ' Enumerate all User objects in the  
        ' catalog's Users collection.  
        For Each usrLoop In .Users  
            Debug.Print "  " & usrLoop.Name  
            Debug.Print "    Belongs to these groups:"  
            ' Enumerate all Group objects in each User  
            ' object's Groups collection.  
            If usrLoop.Groups.Count <> 0 Then  
                For Each grpLoop In usrLoop.Groups  
                    Debug.Print "    " & grpLoop.Name  
                Next grpLoop  
            Else  
                Debug.Print "    [None]"  
            End If  
        Next usrLoop  
  
        ' Enumerate all Group objects in the default  
        ' workspace's Groups collection.  
        For Each grpLoop In .Groups  
            Debug.Print "  " & grpLoop.Name  
            Debug.Print "    Has as its members:"  
            ' Enumerate all User objects in each Group  
            ' object's Users collection.  
            If grpLoop.Users.Count <> 0 Then  
                For Each usrLoop In grpLoop.Users  
                    Debug.Print "    " & usrLoop.Name  
                Next usrLoop  
            Else  
                Debug.Print "    [None]"  
            End If  
        Next grpLoop  
  
        ' Delete new User and Group objects because this  
        ' is only a demonstration.  
        ' These two line are commented out because the sub "OwnersX" uses  
        ' the group "Accounting".  
'        .Users.Delete "Pat Smith"  
'        .Groups.Delete "Accounting"  
  
    End With  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set usrNew = Nothing  
    Exit Sub  
  
GroupXError:  
  
    Set cat = Nothing  
    Set usrNew = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndGroupVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (groupes ADOX)](./append-method-adox-groups.md)   
 [Append, méthode (utilisateurs ADOX)](./append-method-adox-users.md)   
 [Catalog, objet (ADOX)](./catalog-object-adox.md)   
 [ChangePassword, méthode (ADOX)](./changepassword-method-adox.md)   
 [Group, objet (ADOX)](./group-object-adox.md)   
 [Groups, collection (ADOX)](./groups-collection-adox.md)   
 [User, objet (ADOX)](./user-object-adox.md)   
 [Users, collection (ADOX)](./users-collection-adox.md)
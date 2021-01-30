---
description: Save et Open, exemple de méthodes (VB)
title: Save et Open, exemples de méthodes (VB) | Microsoft Docs
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
- Save method [ADO], Visual Basic example
- Open method [ADO]
ms.assetid: ddccdf58-9c57-4c9b-8b7f-0cf193f955fb
author: rothja
ms.author: jroth
ms.openlocfilehash: c76d26032d9986cba52442d62bb7ef8c10db5419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166603"
---
# <a name="save-and-open-methods-example-vb"></a>Save et Open, exemple de méthodes (VB)
Ces trois exemples montrent comment les méthodes [Save](./save-method.md) et [Open](./open-method-ado-recordset.md) peuvent être utilisées ensemble.  
  
 Supposons que vous vous apprêtez à effectuer un voyage d’affaires et que vous souhaitez prendre un tableau à partir d’une base de données. Avant d’aller plus tard, vous accédez aux données sous la forme [d’un jeu d’enregistrements](./recordset-object-ado.md) et vous les enregistrez sous une forme transportable. Lorsque vous arrivez à votre destination, vous accédez au **Recordset** sous la forme d’un **jeu d’enregistrements** local et déconnecté. Vous apportez des modifications au **jeu d’enregistrements**, puis vous l’enregistrez de nouveau. Enfin, lorsque vous revenez chez vous, vous vous reconnectez à la base de données et vous le mettez à jour avec les modifications que vous avez apportées sur la route.  
  
 Tout d’abord, accédez à la table **_Authors_* et enregistrez-la.  
  
```  
'BeginSaveVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim rstAuthors As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    Set rstAuthors = New ADODB.Recordset  
    strSQLAuthors = "SELECT au_id, au_lname, au_fname, city, phone FROM Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adOpenDynamic, adLockOptimistic, adCmdText  
  
    'For sake of illustration, save the Recordset to a diskette in XML format  
    rstAuthors.Save "c:\Pubs.xml", adPersistXML  
  
    ' clean up  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    'clean up  
    If Not rstAuthors Is Nothing Then  
        If rstAuthors.State = adStateOpen Then rstAuthors.Close  
    End If  
    Set rstAuthors = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSaveVB  
```  
  
 À ce stade, vous êtes arrivé à votre destination. Vous allez accéder à la table _*_Authors_*_ en tant que local, Disconnected _ _ Recordset * *. Vous devez disposer du fournisseur **MSPersist** sur l’ordinateur que vous utilisez pour accéder au fichier enregistré, a:\Pubs.xml.  
  
```  
Attribute VB_Name = "Save"  
```  
  
 Enfin, vous revenez à la page de démarrage. Maintenant, mettez à jour la base de données avec vos modifications.  
  
```  
Attribute VB_Name = "Save"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)   
 [En savoir plus sur la persistance des recordsets](../../guide/data/more-about-recordset-persistence.md)   
 [Save, méthode](./save-method.md)
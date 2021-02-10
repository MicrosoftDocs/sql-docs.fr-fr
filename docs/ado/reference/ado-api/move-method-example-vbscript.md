---
description: Move, exemple de méthode (VBScript)
title: Move, exemple de méthode (VBScript) | Microsoft Docs
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
- Move method, VBScript example
ms.assetid: 29ec4b95-8986-4970-943f-3da3ecb207a2
author: rothja
ms.author: jroth
ms.openlocfilehash: 631a0056dfea33b8bd1b0670e7c30c67a9332287
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041739"
---
# <a name="move-method-example-vbscript"></a>Move, exemple de méthode (VBScript)
Cet exemple utilise la méthode [Move](./move-method-ado.md) pour positionner le pointeur d’enregistrement, en fonction de l’entrée utilisateur.  
  
 Utilisez l’exemple suivant dans une page de Active Server (ASP). Pour afficher cet exemple entièrement fonctionnel, vous devez disposer de la source de données AdvWorks. mdb (installée avec le kit de développement logiciel (SDK)) située dans C:\Program Files\Microsoft Platform SDK\Samples\DataAccess\Rds\RDSTest\advworks.mdb ou modifier le chemin d’accès dans l’exemple de code pour refléter l’emplacement réel de ce fichier. Il s’agit d’un fichier de base de données Microsoft Access.  
  
 Utilisez **Find** pour localiser le fichier adovbs. Inc et placez-le dans le répertoire que vous prévoyez d’utiliser. Coupez et collez le code suivant dans le bloc-notes ou dans un autre éditeur de texte, puis enregistrez-le en tant que **MoveVBS. asp**. Vous pouvez afficher le résultat dans n’importe quel navigateur.  
  
 Essayez d’entrer une lettre ou un nombre entier pour voir le travail de gestion des erreurs.  
  
```  
<!-- BeginMoveVBS -->  
<%@ Language=VBScript %>  
<%' use this meta tag instead of adovbs.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
<HTML>  
<HEAD>  
<TITLE>ADO Move Methods</TITLE>  
<STYLE>  
<!--  
BODY {  
   font-family: "MS SANS SERIF",sans-serif;  
    }  
.thead1 {  
   background-color: #008080;   
   font-family: 'Arial Narrow','Arial',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Arial Narrow','Arial',sans-serif;   
   font-size: x-small;  
    }  
-->  
</STYLE>  
</HEAD>  
<BODY>   
<H3>ADO Move Methods</H3>  
<% ' to integrate/test this code replace the   
   ' Data Source value in the Connection string%>  
<%   
    ' connection and recordset variables  
    Dim Cnxn, strCnxn  
    Dim rsCustomers, strSQLCustomers  
  
    ' open connection  
    Set Cnxn = Server.CreateObject("ADODB.Connection")  
    strCnxn = "Provider='sqloledb';Data Source=" & _  
            Request.ServerVariables("SERVER_NAME") & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
  
    Cnxn.Open strCnxn  
  
     ' create and open Recordset using object refs  
    Set rsCustomers = Server.CreateObject("ADODB.Recordset")  
    strSQLCustomers = "Customers"  
  
    rsCustomers.ActiveConnection = Cnxn  
    rsCustomers.CursorLocation = adUseClient  
    rsCustomers.CursorType = adOpenKeyset  
    rsCustomers.LockType = adLockOptimistic  
    rsCustomers.Source = strSQLCustomers  
    rsCustomers.Open  
  
    'Check number of user moves this session and increment by entry  
    Session("Clicks") = Session("Clicks") + Request.Form("MoveAmount")  
    Clicks = Session("Clicks")  
    ' Move to last known recordset position plus amount passed   
    rsCustomers.Move CInt(Clicks)  
  
    'Error Handling  
    If rsCustomers.EOF Then  
        Session("Clicks") = rsCustomers.RecordCount  
        Response.Write "This is the Last Record"  
        rsCustomers.MoveLast  
    ElseIf rsCustomers.BOF Then  
        Session("Clicks") = 1  
        rsCustomers.MoveFirst  
        Response.Write "This is the First Record"  
    End If  
    %>  
  
    <H3>Current Record Number is <BR>  
    <%   
    If Session("Clicks") = 0 Then Session("Clicks") = 1  
    Response.Write(Session("Clicks") )%> of <%=rsCustomers.RecordCount%></H3>  
    <HR>  
  
    <TABLE COLSPAN=8 CELLPADDING=5 BORDER=0>  
  
    <!-- BEGIN column header row for Customer Table-->  
  
    <TR CLASS=thead1>  
       <TD>Company Name</TD>  
       <TD>Contact Name</TD>  
       <TD>City</TD>  
    </TR>  
        <% 'display%>  
        <TR CLASS=tbody>  
          <TD> <%= rsCustomers("CompanyName")%> </TD>  
          <TD> <%= rsCustomers("ContactName")%></TD>  
          <TD> <%= rsCustomers("City")%> </TD>  
        </TR>   
    </TABLE>  
  
    <HR>  
    <Input Type=Button Name=cmdDown  Value="<  ">  
    <Input Type=Button Name=cmdUp Value=" >">  
    <H5>Click Direction Arrows for Previous or Next Record  
    <BR> <BR>  
  
    <FORM Method = Post Action="MoveVbs.asp" Name=Form>  
    <TABLE>  
        <TR>  
           <TD><Input Type="Button" Name=Move Value="Move Amount "></TD>  
           <TD></TD>  
           <TD><Input Type="Text" Size="4" Name="MoveAmount" Value=0></TD>  
        <TR>  
    </TABLE>  
    Click Move Amount to use Move Method<br>  
    Enter Number of Records to Move + or - </H5>    </FORM>  
</BODY>  
  
<Script Language = "VBScript">  
  
Sub Move_OnClick  
   ' Make sure move value entered is an integer  
   If IsNumeric(Document.Form.MoveAmount.Value)Then  
      Document.Form.MoveAmount.Value = CInt(Document.Form.MoveAmount.Value)  
      Document.Form.Submit  
   Else  
      MsgBox "You Must Enter a Number", ,"ADO-ASP Example"  
      Document.Form.MoveAmount.Value = 0  
   End If  
End Sub  
  
Sub cmdDown_OnClick  
   Document.Form.MoveAmount.Value = -1  
   Document.Form.Submit  
End Sub  
  
Sub cmdUp_OnClick  
   Document.Form.MoveAmount.Value = 1  
   Document.Form.Submit  
End Sub  
</Script>  
  
<%  
    ' clean up  
    If rsCustomers.State = adStateOpen then  
        rsCustomers.Close  
    End If  
    If Cnxn.State = adStateOpen then  
        Cnxn.Close  
    End If  
%>  
</HTML>  
<!-- EndMoveVBS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Move, méthode (ADO)](./move-method-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)
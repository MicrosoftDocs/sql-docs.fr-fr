---
description: Execute, Requery et Clear, exemple de méthode (VBScript)
title: Execute, Requery et Clear, exemples de méthodes (VBScript) | Microsoft Docs
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
- Execute method [ADO], VBScript example
- Clear method [ADO], VBScript example
- Requery method [ADO], VBScript example
ms.assetid: 3a7bbf07-2fca-4892-95f4-eec93f2d5e91
author: rothja
ms.author: jroth
ms.openlocfilehash: c0708e50451d58dc6732ed9ffa2714716fb0c55e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034129"
---
# <a name="execute-requery-and-clear-methods-example-vbscript"></a>Execute, Requery et Clear, exemple de méthode (VBScript)
Cet exemple illustre la méthode **Execute** lorsqu’elle est exécutée à la fois à partir d’un objet de [commande](../../../ado/reference/ado-api/command-object-ado.md) et d’un objet de [connexion](../../../ado/reference/ado-api/connection-object-ado.md) . Elle utilise également la méthode [Requery](../../../ado/reference/ado-api/requery-method.md) pour récupérer les données actuelles dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)et la méthode [Clear](../../../ado/reference/ado-api/clear-method-ado.md) pour effacer le contenu de la collection [Errors](../../../ado/reference/ado-api/errors-collection-ado.md) . Les procédures ExecuteCommand et PrintOutput sont requises pour exécuter cette procédure.  
  
 Utilisez l’exemple suivant dans une page de Active Server (ASP). Pour afficher cet exemple entièrement fonctionnel, vous devez disposer de la source de données AdvWorks. mdb (installée avec les exemples du kit de développement logiciel (SDK)) située dans C:\Program Files\Microsoft Platform SDK\Samples\DataAccess\Rds\RDSTest\advworks.mdb ou modifier le chemin d’accès dans l’exemple de code pour refléter l’emplacement réel de ce fichier. Il s’agit d’un fichier de base de données Microsoft Access.  
  
 Utilisez **Find** pour localiser le fichier adovbs. Inc et placez-le dans le répertoire que vous prévoyez d’utiliser. Coupez et collez le code suivant dans le bloc-notes ou dans un autre éditeur de texte, puis enregistrez-le en tant que **ExecuteVBS. asp**. Vous pouvez afficher le résultat dans n’importe quel navigateur client.  
  
```  
<!-- BeginExecuteVBS -->  
<%@ Language=VBScript %>  
<% ' use this meta tag instead of ADOVBS.inc%>  
<!-- METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4"  -->  
<HTML>  
<HEAD>  
<META name="VI60_DefaultClientScript"  content=VBScript>  
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0">  
<title>ADO Execute Method</title>  
<STYLE>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead {  
   background-color: #008080;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</STYLE>  
</HEAD>  
  
<BODY>  
<H3>ADO Execute Method</H3>  
<HR>  
<H4>Recordset Retrieved Using Connection Object</H4>  
<!--- Recordsets retrieved using Execute method of Connection and Command Objects-->  
<%   
     ' connection, command and recordset variables  
    Dim Cnxn, strCnxn  
    Dim rsCustomers, strSQLCustomers  
    Dim Cmd   
    Dim rsProducts, strSQLProducts  
  
    ' create and open connection  
    Set Cnxn = Server.CreateObject("ADODB.Connection")   
    strCnxn="Provider='sqloledb';Data Source=" & _  
            Request.ServerVariables("SERVER_NAME") & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    Cnxn.Open  strCnxn  
    ' create and open recordset  
    Set rsCustomers = Server.CreateObject("ADODB.Recordset")  
    strSQLCustomers = "Customers"  
    rsCustomers.Open strSQLCustomers, Cnxn, adOpenKeyset, adLockOptimistic, adCmdTable  
  
    '1st Recordset using Connection - Execute  
    Set rsCustomers = Cnxn.Execute(strSQLCustomers)   
  
    Set Cmd = Server.CreateObject("ADODB.Command")  
    Cmd.ActiveConnection = Cnxn  
    strSQLProducts = "SELECT * From Products"  
    Cmd.CommandText = strSQLProducts  
  
    '2nd Recordset Cmd - execute   
    Set rsProducts = Cmd.Execute  
    %>  
    <TABLE COLSPAN=8 CELLPADDING=5 BORDER=0 ALIGN=CENTER>  
    <!-- BEGIN column header row for Customer Table-->  
    <TR CLASS=thead>  
      <TH>Company Name</TH>  
      <TH>Contact Name</TH>  
      <TH>City</TH>  
    </TR>  
  
    <!--Display ADO Data from Customer Table-->  
    <%   
    Do While Not rsCustomers.EOF %>  
      <TR CLASS=tbody>  
        <TD>   
        <%= rsCustomers("CompanyName")%>   
        </TD>  
        <TD>  
        <%= rsCustomers("ContactName") %>   
        </TD>  
        <TD>   
        <%= rsCustomers("City")%>   
        </TD>  
      </TR>   
      <%   
    rsCustomers.MoveNext   
    Loop   
    %>  
</TABLE>  
  
<HR>  
<H4>Recordset Retrieved Using Command Object</H4>  
  
<TABLE CELLPADDING=5 BORDER=0 ALIGN=CENTER WIDTH="80%">  
  
<!-- BEGIN column header row for Product List Table-->  
<TR CLASS=thead2>  
  <TH>Product Name</TH>  
  <TH>Unit Price</TH>  
</TR>  
  
<!-- Display ADO Data Product List-->  
<% Do Until rsProducts.EOF %>  
  <TR CLASS=tbody>  
    <TD>  
    <%= rsProducts("ProductName")%>    
    </TD>  
    <TD>   
    <%= rsProducts("UnitPrice")%>   
    </TD>  
<%   
    rsProducts.MoveNext   
    Loop  
  
    ' clean up  
    If rsCustomers.State = adStateOpen then  
        rsCustomers.Close  
    End If  
    If rsProducts.State = adStateOpen then  
        rsProducts.Close  
    End If  
    If Cnxn.State = adStateOpen then  
        Cnxn.Close  
    End If  
    Set Cmd = Nothing  
    Set rsCustomers = Nothing  
    Set rsProducts = Nothing  
    Set Cnxn = Nothing  
%>  
</TABLE>  
  
</BODY>  
</HTML>  
<!-- EndExecuteVBS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Clear, méthode (ADO)](../../../ado/reference/ado-api/clear-method-ado.md)   
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)   
 [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Objet Error](../../../ado/reference/ado-api/error-object.md)   
 [Errors, collection (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [Execute, méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)   
 [Execute, méthode (connexion ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Requery, méthode](../../../ado/reference/ado-api/requery-method.md)

---
description: Filter et RecordCount, exemple de propriétés (JScript)
title: Filter et RecordCount, exemple de propriétés (JScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- JScript
helpviewer_keywords:
- RecordCount property [ADO], JScript example
- Filter property [ADO], JScript example
ms.assetid: 677fa67e-9cb9-4d7d-a786-beeb5bee5236
author: rothja
ms.author: jroth
ms.openlocfilehash: 15043939f3e2f024895d5199117550022ad43117
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171062"
---
# <a name="filter-and-recordcount-properties-example-jscript"></a>Filter et RecordCount, exemple de propriétés (JScript)
Cet exemple ouvre un **jeu d’enregistrements** dans la table Companies de la base de données Northwind, puis utilise la propriété [Filter](./filter-property.md) pour limiter les enregistrements visibles à ceux dont le champ CompanyName commence par la lettre D. Coupez et collez le code suivant dans le bloc-notes ou un autre éditeur de texte, puis enregistrez-le en tant que **FilterJS. asp**.  
  
```  
<!-- BeginFilterJS -->  
<%@  Language=JavaScript %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
  
<head>  
<title>ADO Recordset.Filter Example</title>  
<style>  
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
</style>  
</head>  
  
<body bgcolor="White">  
  
<h1>ADO Recordset.Filter Example</h1>  
<!-- Page text goes here -->  
<%  
    // connection and recordset variables  
    var Cnxn = Server.CreateObject("ADODB.Connection")  
    var strCnxn = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var rsCustomers = Server.CreateObject("ADODB.Recordset");  
    var SQLCustomers = "select * from Customers;";  
    // record variables  
    var fld, filter  
    var showBlank = " ";  
    var showNull = "-NULL-";  
  
    try  
    {  
        //open connection   
        Cnxn.Open(strCnxn);  
  
        // create recordset client-side using object refs  
        rsCustomers.ActiveConnection = Cnxn;  
        rsCustomers.CursorLocation = adUseClient;  
        rsCustomers.CursorType = adOpenKeyset;  
        rsCustomers.LockType = adLockOptimistic;  
        rsCustomers.Source = SQLCustomers;  
        rsCustomers.Open();  
  
        rsCustomers.MoveFirst();  
  
        //set filter  
        filter = "CompanyName LIKE 'b*'";  
        rsCustomers.Filter = filter  
  
        if (rsCustomers.RecordCount == 0) {  
            Response.Write("No records matched ");  
            Response.Write (SQLCustomers + "So cannot make table...");  
            Cnxn.Close();  
            Response.End  
        }  
        else {  
        // show the data  
            Response.Write('<table width="100%" border="2">');      
            while(!rsCustomers.EOF) {  
                Response.Write('<tr class="tbody">');  
                for (var thisField = 0; thisField < rsCustomers.Fields.Count; thisField++) {  
                    fld = rsCustomers(thisField);  
                    fldValue = fld.Value;  
                    if (fldValue == null)  
                        fldValue = showNull;  
                    if (fldValue == "")  
                        thisField=showBlank;  
                    Response.Write("<td>" + fldValue + "</td>")  
                }  
                rsCustomers.MoveNext();  
                Response.Write("</tr>");  
            }  
            // close the table  
            Response.Write("</table>");  
        }  
    }      
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // clean up  
        if (rsCustomers.State == adStateOpen)  
            rsCustomers.Close;  
        if (Cnxn.State == adStateOpen)  
            Cnxn.Close;  
        rsCustomers = null;  
        Cnxn = null;  
    }  
%>  
  
</body>  
  
</html>  
<!-- EndFilterJS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Filter (propriété)](./filter-property.md)   
 [RecordCount, propriété (ADO)](./recordcount-property-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)
---
description: Flux et persistance
title: Flux et persistance | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b554afa949acea4f816ce66e92e897477c2fe00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032421"
---
# <a name="streams-and-persistence"></a>Flux et persistance
La méthode [Save](../../../ado/reference/ado-api/save-method.md) de l’objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) stocke ou *conserve* un **jeu d’enregistrements** dans un fichier, et la méthode [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) restaure le **Recordset** à partir de ce fichier.  
  
 Avec ADO 2,7 ou une version ultérieure, les méthodes **Save** et **Open** peuvent également conserver un **jeu d’enregistrements** dans un objet de [flux](../../../ado/reference/ado-api/stream-object-ado.md) . Cette fonctionnalité est particulièrement utile lorsque vous travaillez avec des pages RDS (Remote Data Service) et des pages d’Active Server (ASP).  
  
 Pour plus d’informations sur la façon dont la persistance peut être utilisée par elle-même sur des pages ASP, consultez la documentation ASP actuelle.  
  
 Voici quelques scénarios qui illustrent la façon dont les objets de **flux** et la persistance peuvent être utilisés.  
  
## <a name="scenario-1"></a>Scénario 1  
 Ce scénario enregistre simplement un **jeu d’enregistrements** dans un fichier, puis dans un **flux**. Il ouvre ensuite le flux persistant dans un autre **Recordset**.  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>Scénario 2  
 Ce scénario conserve un **jeu d’enregistrements** dans un **flux** au format XML. Il lit ensuite le **flux** dans une chaîne que vous pouvez examiner, manipuler ou afficher.  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>Scénario 3  
 Cet exemple de code montre le code ASP qui rend persistant un **jeu d’enregistrements** au format XML directement dans l’objet de **réponse** :  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>Scénario 4  
 Dans ce scénario, le code ASP écrit le contenu de l’ensemble d' **enregistrements** au format ADTG sur le client. Le [service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) peut utiliser ces données pour créer un **jeu d’enregistrements** déconnecté.  
  
 Une nouvelle propriété sur le [DATACONTROL](../../../ado/reference/rds-api/datacontrol-object-rds.md)RDS, [URL](../../../ado/reference/rds-api/url-property-rds.md), pointe vers la page. asp qui génère le **Recordset**. Cela signifie qu’un objet **Recordset** peut être obtenu sans RDS à l’aide de l’objet [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) côté serveur ou de l’utilisateur qui écrit un objet métier. Cela simplifie considérablement le modèle de programmation RDS.  
  
 Code côté serveur, nommé https://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 Code côté client :  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="https://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 Les développeurs ont également la possibilité d’utiliser un objet **Recordset** sur le client :  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "https://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Save, méthode](../../../ado/reference/ado-api/save-method.md)

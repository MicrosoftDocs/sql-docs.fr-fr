---
description: Récupération de jeux de résultats dans les flux
title: Récupération de jeux de résultats dans des flux | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO], retrieving query results
- query results into stream [ADO]
- retrieving results into stream [ADO]
ms.assetid: 996c1321-c926-4f57-8297-85c8c20de974
author: rothja
ms.author: jroth
ms.openlocfilehash: f625a4f96aa5ec083057af4118315d9df494530e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032541"
---
# <a name="retrieving-resultsets-into-streams"></a>Récupération de jeux de résultats dans les flux
Au lieu de recevoir les résultats dans l’objet **Recordset** traditionnel, ADO peut à la place récupérer les résultats de la requête dans un flux. L’objet de **flux** ADO (ou d’autres objets qui prennent en charge l’interface com **IStream** , tels que les objets de **requête** et de **réponse** ASP) peuvent être utilisés pour contenir ces résultats. Une utilisation de cette fonctionnalité consiste à récupérer les résultats au format XML. Avec SQL Server, par exemple, les résultats XML peuvent être retournés de plusieurs façons, par exemple à l’aide de la clause FOR XML avec une requête SQL SELECT ou à l’aide d’une requête XPath.  
  
 Pour recevoir les résultats de la requête dans un format de flux plutôt que dans un **jeu d’enregistrements**, vous devez spécifier la constante **adExecuteStream** de **ExecuteOptionEnum** en tant que paramètre de la méthode **Execute** d’un objet **Command** . Si votre fournisseur prend en charge cette fonctionnalité, les résultats sont retournés dans un flux lors de l’exécution. Vous devrez peut-être spécifier d’autres propriétés spécifiques au fournisseur avant l’exécution du code. Par exemple, avec le fournisseur Microsoft OLE DB pour SQL Server, vous devez spécifier des propriétés telles que le **flux de sortie** dans la collection **Properties** de l’objet **Command** . Pour plus d’informations sur les propriétés dynamiques spécifiques à SQL Server relatives à cette fonctionnalité, consultez XML-Related propriétés dans le Documentation en ligne de SQL Server.  
  
## <a name="for-xml-query-example"></a>Exemple de requête FOR XML  
 L’exemple suivant est écrit en VBScript dans la base de données Northwind :  
  
```html
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<%  Option Explicit      %>  
  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=" & _  
      Request.ServerVariables("SERVER_NAME") & ";" & _  
      Initial Catalog=Northwind;Integrated Security=SSPI;"  
  
   Response.write "Connect String = " & sConn & "<BR/>"  
  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
  
   adoConn.Open  
  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT * FROM PRODUCTS WHERE ProductName='Gumbr Gummibrchen' FOR XML AUTO</sql:query></ROOT>"  
  
   Response.write "Query String = " & sQuery & "<BR/>"  
  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
  
%>  
  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   xmlDoc.resolveExternals=false  
   xmlDoc.async=false  
  
   If xmlDoc.parseError.Reason <> "" then  
      Msgbox "parseError.Reason = " & xmlDoc.parseError.Reason  
   End If  
  
   Dim root, child  
   Set root = xmlDoc.documentElement  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI>" & child.getAttribute("ProductName") & "</LI>"  
   Next  
</SCRIPT>  
  
</HEAD>  
  
<BODY>  
  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
  
```  
  
 La clause FOR XML indique à SQL Server de retourner des données sous la forme d’un document XML.  
  
### <a name="for-xml-syntax"></a>Syntaxe FOR XML  
  
```syntax
FOR XML [RAW|AUTO|EXPLICIT]  
```  
  
 FOR XML RAW génère des éléments de ligne génériques qui ont des valeurs de colonne comme attributs. FOR XML AUTO utilise des heuristiques pour générer une arborescence hiérarchique avec des noms d’éléments basés sur des noms de tables. FOR XML EXPLICIT génère une table universelle avec des relations entièrement décrites par les métadonnées.  
  
 Voici un exemple d’instruction SQL SELECT FOR XML :  
  
```sql
SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO  
```  
  
 La commande peut être spécifiée dans une chaîne comme indiqué plus haut, assignée à **CommandText** ou sous la forme d’une requête de modèle XML affectée à **CommandStream**. Pour plus d’informations sur les requêtes de modèle XML, consultez [flux de commandes](../../../ado/guide/data/command-streams.md) dans ADO ou utilisation de flux pour l’entrée de commande dans l’documentation en ligne de SQL Server.  
  
 En tant que requête de modèle XML, la requête FOR XML se présente comme suit :  
  
```xml
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO </sql:query>  
```  
  
 Cet exemple spécifie l’objet de **réponse** ASP pour la propriété de **flux de sortie** :  
  
```vb
adoCmd.Properties("Output Stream") = Response  
```  
  
 Ensuite, spécifiez le paramètre **adExecuteStream** de l' **instruction EXECUTE**. Cet exemple enveloppe le flux de données dans des balises XML pour créer un îlot de données XML :  
  
```vb
Response.write "<XML ID=MyDataIsle>"  
adoCmd.Execute , , adExecuteStream  
Response.write "</XML>"  
```  
  
### <a name="remarks"></a>Notes  
 À ce stade, le code XML a été transmis au navigateur client et il est prêt à être affiché. Pour ce faire, vous utilisez le VBScript côté client pour lier le document XML à une instance du DOM et vous parcourez chaque nœud enfant pour générer une liste de produits au format HTML.

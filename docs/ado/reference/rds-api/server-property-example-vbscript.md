---
description: Server, exemple de propriété (VBScript)
title: Server, exemple de propriété (VBScript) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Server property [RDS], VBScript example
ms.assetid: 0fe57af9-a4d0-4986-a2e3-beaa4d26ed58
author: rothja
ms.author: jroth
ms.openlocfilehash: 85fbdc115a8b021020ca31f04e11a84fddbb827e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049179"
---
# <a name="server-property-example-vbscript"></a>Server, exemple de propriété (VBScript)
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Le code suivant montre comment définir le [RDS. Paramètre DataControl](./datacontrol-object-rds.md) au moment de la conception et le lier à un contrôle dépendant des données à l’aide du fournisseur SQLOLEDB. Coupez et collez ce code dans un document ASP normal et nommez-le **ServerDesignVBS. asp**. Le script ASP identifie votre serveur.  
  
```  
<!-- BeginServerDesignVBS -->  
<%@ Language=VBScript %>  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>Server Property Example (VBScript)</title>  
<style>  
<!--  
body {  
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
  
<body>  
<h1>Server Property Example (VBScript)</h1>  
  
<TABLE DATASRC=#RDS>  
   <TR>  
      <TD> <SPAN DATAFLD="FirstName"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="LastName"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Title"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Type"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Email"></SPAN> </TD>  
   </TR>  
</TABLE>  
<!-- Remote Data Service with Parameters set at Design Time -->  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=RDS HEIGHT=1 WIDTH=1>  
   <PARAM NAME="SQL" VALUE="Select * from Employees">  
   <PARAM NAME="SERVER" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind'">  
</OBJECT>  
  
</body>  
</html>  
<!-- EndServerDesignVBS -->  
```  
  
 L’exemple suivant montre comment définir les paramètres nécessaires de **RDS. DataControl** au moment de l’exécution. Pour tester cet exemple, coupez et collez ce code dans un document ASP normal et nommez-le **ServerRuntimeVBS. asp**. Le script ASP identifie votre serveur.  
  
```  
<!-- BeginServerRuntimeVBS -->  
<%@ Language=VBScript %>  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>Server Property Example (VBScript)</title>  
<style>  
<!--  
body {  
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
  
<body>  
<h1>Server Property Example (VBScript)</h1>  
  
<H2>RDS API Code Examples</H2>  
  
<H3>Remote Data Service Server Property Set at Run Time</H3>  
  
<!-- RDS.DataControl with no parameters set at design time -->  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=RDC HEIGHT=1 WIDTH=1>  
</OBJECT>  
  
<TABLE DATASRC=#RDC>  
   <TR>  
      <TD> <SPAN DATAFLD="FirstName"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="LastName"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Title"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Type"></SPAN> </TD>  
      <TD> <SPAN DATAFLD="Email"></SPAN> </TD>  
   </TR>  
</TABLE>  
  
<HR>  
<Input Size=70 Name="txtServer" Value="HTTP://<%= Request.ServerVariables("SERVER_NAME")%>">  
<BR>  
<Input Size=70 Name="txtConnect" Value="Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind'">  
<BR>  
<Input Size=70 Name="txtSQL" Value="Select * from Employees">  
<HR>  
<INPUT TYPE=BUTTON NAME="Run" VALUE="Run"><BR>  
  
<Script Language="VBScript">  
<!--  
' Set parameters of RDS.DataControl at Run Time  
Sub Run_OnClick  
   RDC.Server = txtServer.Value  
   RDC.SQL = txtSQL.Value  
   RDC.Connect = txtConnect.Value  
   RDC.Refresh  
End Sub  
-->  
</Script>  
  
</body>  
</html>  
<!-- EndServerRuntimeVBS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)   
 [Server, propriété (RDS)](./server-property-rds.md)
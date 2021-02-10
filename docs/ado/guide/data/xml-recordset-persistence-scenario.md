---
description: Scénario de persistance des recordsets XML
title: Scénario de persistance d’un jeu d’enregistrements XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence scenario
ms.assetid: 353d569a-043a-4397-9ee6-564c4af8d5f6
author: rothja
ms.author: jroth
ms.openlocfilehash: bbad3ddcfd8c34be6ea911f4cef580832f1c55b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032221"
---
# <a name="xml-recordset-persistence-scenario"></a>Scénario de persistance des recordsets XML
Dans ce scénario, vous allez créer une application Active Server pages (ASP) qui enregistre le contenu d’un objet Recordset directement dans l’objet de réponse ASP.  
  
> [!NOTE]
>  Ce scénario nécessite que le serveur Internet Information Server 5,0 (IIS) ou version ultérieure soit installé sur votre serveur.  
  
 Le jeu d’enregistrements retourné est affiché dans Internet Explorer à l’aide d’un [objet DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md).  
  
 Les étapes suivantes sont nécessaires pour créer ce scénario :  
  
-   Configuration de l’application  
  
-   Récupérer les données  
  
-   Envoyer les données  
  
-   Recevoir et afficher les données  
  
## <a name="step-1-set-up-the-application"></a>Étape 1 : configurer l’application  
 Créez un répertoire virtuel IIS nommé « XMLPersist » avec des autorisations de script. Créez deux nouveaux fichiers texte dans le dossier vers lequel pointe le répertoire virtuel, l’un nommé « XMLResponse. asp », l’autre nommé « Default.htm ».  
  
## <a name="step-2-get-the-data"></a>Étape 2 : obtenir les données  
 Dans cette étape, vous allez écrire le code pour ouvrir un jeu d’enregistrements ADO et vous préparer à l’envoyer au client. Ouvrez le fichier XMLResponse. asp à l’aide d’un éditeur de texte, tel que le bloc-notes, puis insérez le code suivant.  
  
```  
<%@ language="VBScript" %>  
  
<!-- #include file='adovbs.inc' -->  
  
<%  
  Dim strSQL, strCon  
  Dim adoRec   
  Dim adoCon   
  Dim xmlDoc   
  
  ' You will need to change "MySQLServer" below to the name of the SQL   
  ' server machine to which you want to connect.  
  strCon = "Provider=sqloledb;Data Source=MySQLServer;Initial Catalog=Pubs;Integrated Security=SSPI;"  
  Set adoCon = server.createObject("ADODB.Connection")  
  adoCon.Open strCon  
  
  strSQL = "SELECT Title, Price FROM Titles ORDER BY Price"  
  Set adoRec = Server.CreateObject("ADODB.Recordset")  
  adoRec.Open strSQL, adoCon, adOpenStatic, adLockOptimistic, adCmdText  
```  
  
 Veillez à remplacer la valeur du `Data Source` paramètre dans par `strCon` le nom de votre Microsoft SQL Server ordinateur.  
  
 Laissez le fichier ouvert et passez à l’étape suivante.  
  
## <a name="step-3-send-the-data"></a>Étape 3 : envoyer les données  
 Maintenant que vous disposez d’un jeu d’enregistrements, vous devez l’envoyer au client en l’enregistrant au format XML dans l’objet de réponse ASP. Ajoutez le code suivant en bas de XMLResponse. asp.  
  
```  
  Response.ContentType = "text/xml"  
  Response.Expires = 0  
  Response.Buffer = False  
  
  Response.Write "<?xml version='1.0'?>" & vbNewLine  
  adoRec.save Response, adPersistXML  
  adoRec.Close  
  Set adoRec=Nothing  
%>  
```  
  
 Notez que l’objet de réponse ASP est spécifié comme destination de la [méthode Save](../../reference/ado-api/save-method.md)de l’ensemble d’enregistrements. La destination de la méthode Save peut être n’importe quel objet prenant en charge l’interface IStream, tel qu’un [objet de flux ADO (ADO)](../../reference/ado-api/stream-object-ado.md), ou un nom de fichier qui comprend le chemin d’accès complet de l’enregistrement du Recordset.  
  
 Enregistrez et fermez XMLResponse. asp avant de passer à l’étape suivante. Copiez également le fichier adovbs. Inc à partir du dossier d’installation par défaut de la bibliothèque ADO dans le dossier où vous avez enregistré le fichier XMLResponse. asp.  
  
## <a name="step-4-receive-and-display-the-data"></a>Étape 4 : réception et affichage des données  
 Au cours de cette étape, vous allez créer un fichier HTML avec un objet [objet DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md) incorporé qui pointe vers le fichier XMLResponse. asp pour obtenir le jeu d’enregistrements. Ouvrez default.htm avec un éditeur de texte, tel que le bloc-notes, puis ajoutez le code suivant. Remplacez « SqlServer » dans l’URL par le nom de votre serveur.  
  
```  
<HTML>  
<HEAD><TITLE>ADO Recordset Persistence Sample</TITLE></HEAD>  
<BODY>  
  
<TABLE DATASRC="#RDC1" border="1">  
  <TR>  
<TD><SPAN DATAFLD="title"></SPAN></TD>  
<TD><SPAN DATAFLD="price"></SPAN></TD>  
  </TR>  
</TABLE>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="RDC1">  
   <PARAM NAME="URL" VALUE="XMLResponse.asp">  
</OBJECT>  
  
</BODY>  
</HTML>  
```  
  
 Fermez le fichier default.htm et enregistrez-le dans le dossier où vous avez enregistré XMLResponse. asp. À l’aide d’Internet Explorer 4,0 ou version ultérieure, ouvrez l’URL https://*SqlServer*/XMLPersist/default.htm et observez les résultats. Les données sont affichées dans une table DHTML liée. Ouvrez à présent l’URL https:// *SqlServer* /XMLPersist/XMLResponse.asp et observez les résultats. Le code XML s’affiche.  
  
## <a name="see-also"></a>Voir aussi  
 [Save, méthode](../../reference/ado-api/save-method.md)   
 [Persistance des enregistrements au format XML](./persisting-records-in-xml-format.md)
---
description: Utilisation d’un objet Connection
title: Utilisation d’un objet de connexion | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 4b34f971-5699-43e7-9b15-137d334fa66e
author: rothja
ms.author: jroth
ms.openlocfilehash: 36344c8b8c03677ac3b0ae25db097917d8d10d2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036799"
---
# <a name="using-a-connection-object"></a>Utilisation d’un objet Connection
Avant d’ouvrir un objet de **connexion** , vous devez définir certaines informations sur la source de données et le type de connexion. La plupart de ces informations sont détenues par le paramètre *ConnectionString* de la [méthode Open](../../../ado/reference/ado-api/open-method-ado-connection.md) sur l’objet **Connection** , ou par la [propriété ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) de l’objet **Connection** . Une chaîne de connexion se compose d’une liste de paires argument/valeur séparées par des points-virgules, les valeurs étant placées entre guillemets simples. Par exemple :  
  
```  
Dim sConn As String  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
```  
  
> [!NOTE]
>  Vous pouvez également spécifier un nom de source de données (DSN) ODBC ou un fichier UDL (Data Link) dans une chaîne de connexion. Pour plus d’informations sur les noms de source de données, consultez [gestion des sources de données](../../../odbc/admin/managing-data-sources.md) dans le Guide de référence du programmeur ODBC. Pour plus d’informations sur UDL, consultez [vue d’ensemble](/previous-versions/windows/desktop/ms718102(v=vs.85)) de l’API de liaison de données dans le Guide de référence du programmeur OLE DB.  
  
 En général, vous établissez une connexion en appelant la méthode **Connection. Open** avec une *chaîne de connexion* appropriée comme paramètre. Un exemple est illustré dans l’extrait de code Visual Basic suivant :  
  
```  
Dim oConn As ADODB.Connection  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
' Open a connection.  
Set oConn = New ADODB.Connection  
.Open   
  
' Make a query over the connection.  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
Set oRs = New ADODB.Recordset  
oRs.Open sSQL, , adOpenStatic, adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
' Close the connection.  
oConn.Close  
Set oConn = Nothing  
  
```  
  
 Ici **ors. Open** accepte une variable d’objet de **connexion** (*oConn*) comme valeur de son paramètre *ActiveConnection* . En outre, la propriété **Connection. CursorLocation** prend la valeur par défaut **adUseServer**. Comparez ceci à l’exemple [HelloData](../../../ado/guide/data/hellodata-a-simple-ado-application.md) dans la section précédente. L’instruction suivante entraînerait des erreurs d’exécution.  
  
```  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
```
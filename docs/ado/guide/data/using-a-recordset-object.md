---
description: Utilisation d’un objet Recordset
title: Utilisation d’un objet Recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 01c630d8-eb35-4bd0-a99f-7c0f85316cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 44fc7a242887419ba3013113632c8d1acd2a3337
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032301"
---
# <a name="using-a-recordset-object"></a>Utilisation d’un objet Recordset
Vous pouvez également utiliser **Recordset. Open** pour établir implicitement une connexion et émettre une commande sur cette connexion en une seule opération. Par exemple, dans Visual Basic :  
  
```  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
  
' Create and Open the Recordset object.  
Set oRs = New ADODB.Recordset  
oRs.CursorLocation = adUseClient  
oRs.Open sSQL, sConn, adOpenStatic, _  
               adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
oRs.Close          
Set oRs = Nothing  
```  
  
 Notez que **ors. Open** prend une chaîne de connexion (*sConn*), à la place d’un objet de **connexion** (*oConn*), en tant que valeur de son paramètre **ActiveConnection** . Le type de curseur côté client est également appliqué en définissant la propriété **CursorLocation** sur l’objet **Recordset** . Là encore, Comparez ceci avec l’exemple de **HelloData** .

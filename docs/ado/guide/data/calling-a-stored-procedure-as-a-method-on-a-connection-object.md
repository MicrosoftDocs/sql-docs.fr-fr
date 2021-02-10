---
description: Appel d’une procédure stockée en tant que méthode sur un objet Connection
title: Appel d’une procédure stockée en tant que méthode sur un objet de connexion | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 35ffdb79-a931-4271-a3bb-0cd804cf173e
author: rothja
ms.author: jroth
ms.openlocfilehash: ee564f90e5a1ff24155eaae5b4613df821138209
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037780"
---
# <a name="calling-a-stored-procedure-as-a-method-on-a-connection-object"></a>Appel d’une procédure stockée en tant que méthode sur un objet Connection
Vous pouvez appeler une procédure stockée comme s’il s’agissait d’une méthode native sur l’objet de **connexion** ouvert associé. Cela revient à appeler une commande nommée sur l’objet de **connexion** .  
  
 L’exemple de code Visual Basic suivant appelle une procédure stockée dans l’exemple de base de données Northwind, appelée CustOrdersOrders, qui est répertoriée ici à nouveau pour plus de commodité.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 L’exemple de code suivant montre comment appeler une procédure stockée comme s’il s’agissait d’une méthode native sur un objet de **connexion** ouvert associé.  
  
```  
Const DS = "MySQLServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
Dim objComm As New ADODB.Command  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Set a stored procedure  
  
Set objComm.ActiveConnection = objConn  
  
' Execute the stored procedure on  
' the active connection object...  
'    "ALFKI" is the required input parameter,  
'    objRs is the resultant output variable.  
objComm(1) = "ALFKI"
Set objRs = objComm.Execute

' Display the result.  
Debug.Print "Results returned from sp_CustOrdersOrders for ALFKI: "  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
 Loop  
  
'Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
Set objComm = Nothing  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](../../reference/ado-api/connection-object-ado.md)

---
description: Appel d’une procédure stockée avec une commande
title: Appel d’une procédure stockée à l’aide d’une commande | Microsoft Docs
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
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c66c4bdd347aaa8d01ab80d3a11ae67ce683b47
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075861"
---
# <a name="calling-a-stored-procedure-with-a-command"></a>Appel d’une procédure stockée avec une commande
Vous pouvez utiliser une commande pour appeler une procédure stockée. L’exemple de code à la fin de cette rubrique fait référence à une procédure stockée dans l’exemple de base de données Northwind, appelée CustOrdersOrders, qui est définie comme suit.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Pour plus d’informations sur la définition et l’appel des procédures stockées, consultez la documentation de votre SQL Server.  
  
 Cette procédure stockée est similaire à la commande utilisée dans les paramètres de l' [objet Command](./command-object-parameters.md). Il prend un paramètre d’ID de client et retourne des informations sur les commandes de ce client. L’exemple de code suivant utilise cette procédure stockée comme source pour un **jeu d’enregistrements** ADO.  
  
 L’utilisation de la procédure stockée vous permet d’accéder à une autre fonctionnalité ADO : la méthode **Refresh** de la collection **Parameters** . À l’aide de cette méthode, ADO peut automatiquement renseigner toutes les informations sur les paramètres requis par la commande au moment de l’exécution. Cette technique présente une baisse des performances, car ADO doit interroger la source de données pour obtenir des informations sur les paramètres.  
  
 D’autres différences importantes existent entre l’exemple de code suivant et le code dans les paramètres de l' [objet de commande](./command-object-parameters.md), où les paramètres ont été entrés manuellement. Tout d’abord, ce code n’affecte pas la **valeur true** à la propriété **Prepared** , car il s’agit d’une procédure stockée SQL Server qui est précompilée par définition. Deuxièmement, la propriété **CommandType** de l’objet **Command** est passée à **adCmdStoredProc** dans le deuxième exemple pour informer ADO que la commande était une procédure stockée.  
  
 Enfin, dans le deuxième exemple, le paramètre doit être référencé par index lors de la définition de la valeur, car vous ne connaissez peut-être pas le nom du paramètre au moment de la conception. Si vous connaissez le nom du paramètre, vous pouvez définir la nouvelle propriété [NamedParameters](../../reference/ado-api/namedparameters-property-ado.md) de l’objet de **commande** sur true et faire référence au nom de la propriété. Vous vous demandez peut-être pourquoi la position du premier paramètre mentionné dans la procédure stockée ( @CustomerID ) est 1 au lieu de 0 ( `objCmd(1) = "ALFKI"` ). Cela est dû au fait que le paramètre 0 contient une valeur de retour de la procédure stockée SQL Server.  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Article 117500 de la base de connaissances](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/185125)

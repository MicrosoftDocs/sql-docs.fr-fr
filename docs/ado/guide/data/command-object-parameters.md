---
description: Paramètres de l’objet Command
title: Paramètres de l’objet de commande | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], parameters
ms.assetid: 10e7ef4a-78bf-4e91-931e-cbc6c065dd4c
author: rothja
ms.author: jroth
ms.openlocfilehash: a49f7099e45e92cac31653f5c3f08442dda2459b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033319"
---
# <a name="command-object-parameters"></a>Paramètres de l’objet Command
La rubrique précédente a abordé la [création et l’exécution d’une commande simple](./creating-and-executing-a-simple-command.md). Une utilisation plus intéressante de l’objet de [commande](../../reference/ado-api/command-object-ado.md) est présentée dans l’exemple suivant, dans lequel la commande SQL a été paramétrée. Cette modification permet de réutiliser la commande, en passant chaque fois une valeur différente pour le paramètre. Étant donné que la propriété [Prepared Property](../../reference/ado-api/prepared-property-ado.md) de l’objet **Command** a la valeur **true**, ADO demande au fournisseur de compiler la commande spécifiée dans [CommandText](../../reference/ado-api/commandtext-property-ado.md) avant de l’exécuter pour la première fois. La commande compilée est également conservée en mémoire. Cela ralentit légèrement l’exécution de la commande la première fois qu’elle est exécutée en raison de la surcharge requise pour la préparer, mais entraîne un gain de performances chaque fois que la commande est appelée par la suite. Par conséquent, les commandes doivent être préparées uniquement si elles sont utilisées plus d’une fois.  
  
```  
'BeginManualParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set the CommandText as a parameterized SQL query.  
    objCmd.CommandText = "SELECT OrderID, OrderDate, " & _  
                         "RequiredDate, ShippedDate " & _  
                         "FROM Orders " & _  
                         "WHERE CustomerID = ? " & _  
                         "ORDER BY OrderID"  
    objCmd.CommandType = adCmdText  
  
    ' Prepare command because we will be executing it more than once.  
    objCmd.Prepared = True  
  
    ' Create new parameter for CustomerID. Initial value is ALFKI.  
    Set objParm1 = objCmd.CreateParameter("CustId", adChar, _  
                    adParamInput, 5, "ALFKI")  
    objCmd.Parameters.Append objParm1  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Execute once and display.  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' .Set new param value, re-execute command, and display.  
    objCmd("CustId") = "CACTU"  
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
'EndManualParamCmd  
  
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
  
 Tous les fournisseurs ne prennent pas en charge les commandes préparées. Si le fournisseur ne prend pas en charge la préparation de la commande, il peut retourner une erreur dès que cette propriété a la valeur **true**. Si elle ne retourne pas d’erreur, elle ignore la demande de préparation de la commande et affecte la **valeur false** à la propriété **Prepared** .
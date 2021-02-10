---
description: Passage de paramètres à une commande nommée
title: Passage de paramètres à une commande nommée | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- named commands [ADO]
- commands [ADO], passing parameters to a named command
ms.assetid: 36e0cdbe-7f50-40f5-af0d-700f5d8dc75a
author: rothja
ms.author: jroth
ms.openlocfilehash: 11fa07dd7af4512f8b7f41917d2778fecd6584ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032641"
---
# <a name="passing-parameters-to-a-named-command"></a>Passage de paramètres à une commande nommée
Tout comme le résultat de la commande est transmis en tant que variable *out* de la commande nommée, les paramètres d’une commande paramétrable peuvent être transmis comme *dans* les variables de la commande nommée.  
  
 L’exemple de code suivant tente de récupérer toutes les commandes passées par le client dont **CustomerID** a la valeur « ALKFI » dans la base de données Northwind. La valeur de **CustomerID** est fournie au moment où la commande nommée est appelée.  
  
```  
Const DS = "MySqlServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
Dim objComm As New ADODB.Command  
  
CommandText = "SELECT OrderID, OrderDate, " & _  
                     "RequiredDate, ShippedDate " & _  
                     "FROM Orders " & _  
                     "WHERE CustomerID = ? " & _  
                     "ORDER BY OrderID"  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Set a named command.  
objComm.CommandText = CommandText  
objComm.CommandType = adCmdText  
objComm.Name = "GetOrdersOf"  
Set objComm.ActiveConnection = objConn  
  
' Call the named command, passing a CustomerID value  
' as the input parameter.   
'    "ALFKI" is the required input parameter,  
'    objRs is the resultant output variable.  
objConn.GetOrdersOf "ALKFI", objRs  
  
' Display the result.  
Debug.Print "All orders by ALFKI:"  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
Loop  
  
' Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
Set objComm = Nothing  
```  
  
 Notez que tous les paramètres d’entrée doivent précéder toute variable de sortie et que les types de données des paramètres doivent correspondre ou peuvent être convertis en ceux des champs correspondants. L’instruction suivante :  
  
```  
objConn.GetOrdersOf 12345, objRs  
```  
  
 -génère une erreur de types de données incompatibles, car le paramètre d’entrée requis est un type **chaîne** , et non un type **entier** .  
  
 L’appel suivant :  
  
```  
objConn.GetOrdersOf "12345", objRs  
```  
  
 -est valide, mais génère un jeu de résultats vide, car il n’existe aucun enregistrement de ce type dans la base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)

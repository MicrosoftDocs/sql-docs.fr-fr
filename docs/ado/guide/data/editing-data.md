---
description: Modification des données
title: Modification des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, editing data
- AdUseClient [ADO]
- editing data [ADO]
ms.assetid: ef514f85-c446-4f05-824e-c9313b2ffae1
author: rothja
ms.author: jroth
ms.openlocfilehash: 2f8fa6b7a3fb898d425d439b11b42451d42ac17a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033229"
---
# <a name="editing-data"></a>Modification des données
Nous avons expliqué comment utiliser ADO pour se connecter à une source de données, exécuter une commande, obtenir les résultats dans un objet **Recordset** et naviguer dans le **Recordset**. Cette section se concentre sur l’opération ADO fondamentale suivante : modification des données.  
  
 Cette section continue à utiliser l’exemple **d’ensemble d’enregistrements** présenté lors de l' [examen des données](./examining-data.md), avec une modification importante. Le code suivant permet d’ouvrir le **jeu d’enregistrements**:  
  
```  
'BeginEditIntro  
    Dim strSQL As String  
    Dim objRs1 As ADODB.Recordset  
  
    strSQL = "SELECT * FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
  
    objRs1.Open strSQL, GetNewConnection, adOpenStatic, _  
                adLockBatchOptimistic, adCmdText  
  
    ' Disconnect the Recordset from the Connection object.  
    Set objRs1.ActiveConnection = Nothing  
'EndEditIntro  
```  
  
 La modification importante du code implique la définition de la propriété **CursorLocation** de l’objet de **connexion** égal à **adUseClient** dans la fonction *GetNewConnection* (illustrée dans l’exemple suivant), qui indique l’utilisation d’un curseur client. Pour plus d’informations sur les différences entre les curseurs côté client et côté serveur, consultez [Présentation des curseurs et des verrous](./understanding-cursors-and-locks.md).  
  
 Le paramètre **adUseClient** de la propriété **CursorLocation** déplace l’emplacement du curseur à partir de la source de données (dans ce cas, le SQL Server) vers l’emplacement du code client (la station de travail). Ce paramètre force ADO à appeler le moteur de curseur client pour OLE DB sur le client afin de créer et de gérer le curseur.  
  
 Vous avez peut-être également remarqué que le paramètre **LockType** de la méthode **Open** est passé à **adLockBatchOptimistic**. Cela ouvre le curseur en mode batch. (Le fournisseur met en cache plusieurs modifications et les écrit dans la source de données sous-jacente uniquement lorsque vous appelez la méthode **UpdateBatch** .) Les modifications apportées au **jeu d’enregistrements** ne seront pas mises à jour dans la base de données jusqu’à ce que la méthode **UpdateBatch** soit appelée.  
  
 Enfin, le code de cette section utilise une version modifiée de la fonction GetNewConnection. Cette version de la fonction retourne maintenant un curseur côté client. La fonction se présente comme suit :  
  
```  
'BeginNewConnection  
Public Function GetNewConnection() As ADODB.Connection  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim strConnStr As String  
  
    strConnStr = "Provider='SQLOLEDB';Initial Catalog='Northwind';" & _  
                 "Data Source='MySqlServer';Integrated Security='SSPI';"  
  
    objConn.ConnectionString = strConnStr  
    objConn.CursorLocation = adUseClient  
    objConn.Open  
  
    Set GetNewConnection = objConn  
  
    Exit Function  
  
ErrHandler:  
    Set objConn = Nothing  
    Set GetNewConnection = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Function  
'EndNewConnection  
```  
  
 Cette section contient les rubriques suivantes :  
  
-   [Modification d’enregistrements existants](./editing-existing-records.md)  
  
-   [Ajout d’enregistrements](./adding-records.md)  
  
-   [Détermination de ce qui est pris en charge](./determining-what-is-supported.md)  
  
-   [Suppression d’enregistrements avec la méthode Delete](./deleting-records-using-the-delete-method.md)  
  
-   [Alternatives : Utilisation d’instructions SQL](./alternatives-using-sql-statements.md)
---
description: Erreurs de Run-Time ADO
title: Erreurs ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
author: rothja
ms.author: jroth
ms.openlocfilehash: bed9958455af424c4f18cb0f6051eb30e2efaa3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033389"
---
# <a name="ado-run-time-errors"></a>Erreurs de Run-Time ADO
Les erreurs ADO sont signalées à votre programme comme des erreurs d’exécution. Vous pouvez utiliser le mécanisme de piégeage des erreurs de votre langage de programmation pour les intercepter et les gérer. Par exemple, dans Visual Basic, utilisez l’instruction **On Error** . Dans Visual C++, cela dépend de la méthode que vous utilisez pour accéder aux bibliothèques ADO. Avec #import, utilisez un bloc **try-catch** . Sinon, les programmeurs C++ doivent récupérer explicitement l’objet d’erreur en appelant **GetErrorInfo**. La procédure Visual Basic Sub suivante montre comment intercepter une erreur ADO :

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 Cette procédure d’événement **Form_Load** crée intentionnellement une erreur en tentant d’ouvrir le même objet de **connexion** à deux reprises. La deuxième fois que la méthode **Open** est appelée, le gestionnaire d’erreurs est activé. Dans ce cas, l’erreur est de type **adErrObjectOpen**. par conséquent, le gestionnaire d’erreurs affiche le message suivant avant de reprendre l’exécution du programme :

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 Le message d’erreur comprend chaque élément d’information fourni par l’Visual Basic objet **Err** , à l’exception de la valeur **LastDLLError** , qui ne s’applique pas ici. Le numéro d’erreur indique l’erreur qui s’est produite. La description est utile dans les cas où vous ne souhaitez pas gérer l’erreur vous-même. Vous pouvez simplement la transmettre à l’utilisateur. Bien que vous souhaitiez généralement utiliser des messages personnalisés pour votre application, vous ne pouvez pas anticiper chaque erreur ; la description donne un indice sur la cause du problème. Dans l’exemple de code, l’erreur a été signalée par l’objet de **connexion** . Vous verrez le type de l’objet ou l’ID de programmation ici, pas un nom de variable.

> [!NOTE]
>  L’objet Visual Basic **Err** contient uniquement des informations sur l’erreur la plus récente. La collection d' **Erreurs** ADO de l’objet de **connexion** contient un objet d' **erreur** pour chaque erreur déclenchée par l’opération ADO la plus récente. Utilisez la collection **Errors** plutôt que l’objet **Err** pour gérer plusieurs erreurs. Pour plus d’informations sur la collection d' **Erreurs** , consultez [Erreurs du fournisseur](./provider-errors.md). Toutefois, s’il n’existe aucun objet de **connexion** valide, l’objet **Err** est la seule source pour obtenir des informations sur les erreurs ADO.

 Quels types d’opérations sont susceptibles de provoquer des erreurs ADO ? Les erreurs ADO courantes peuvent impliquer l’ouverture d’un objet tel qu’une **connexion** ou un **jeu d’enregistrements**, la tentative de mise à jour des données ou l’appel d’une méthode ou d’une propriété qui n’est pas prise en charge par votre fournisseur.

 OLE DB erreurs peuvent également être transmises à votre application en tant qu’erreurs d’exécution dans la collection d' **Erreurs** .

 La rubrique suivante fournit plus d’informations sur les erreurs ADO.

-   [Informations de référence sur les erreurs ADO](./ado-error-reference.md)
---
description: Erreurs du fournisseur
title: Erreurs du fournisseur | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors collection [ADO]
- provider errors [ADO]
- event-related errors [ADO]
- errors [ADO], provider
- Error object [ADO], provider errors
ms.assetid: cc7d6ff9-2034-45c6-9d61-90b177010054
author: rothja
ms.author: jroth
ms.openlocfilehash: a13bcc83c360533b1137af69b688221157dc0dc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032605"
---
# <a name="provider-errors"></a>Erreurs du fournisseur
Lorsqu’une erreur de fournisseur se produit, une erreur d’exécution de-2147467259 est retournée. Lorsque vous recevez cette erreur, vérifiez la collection d' **Erreurs** de l’objet de **connexion** actif, qui contient une ou plusieurs erreurs décrivant ce qui s’est produit.  
  
## <a name="the-ado-errors-collection"></a>Collection d’erreurs ADO  
 Étant donné qu’une opération ADO particulière peut générer plusieurs erreurs de fournisseur, ADO expose une collection d’objets d’erreur via l’objet de **connexion** . Cette collection ne contient aucun objet si une opération se termine avec succès et contient un ou plusieurs objets d' **erreur** en cas de problème et si le fournisseur a déclenché une ou plusieurs erreurs. Examinez chaque objet d’erreur pour déterminer la cause exacte de l’erreur.  
  
 Dès que vous avez terminé de gérer les erreurs qui se sont produites, vous pouvez effacer la collection en appelant la méthode **Clear** . Il est particulièrement important de supprimer explicitement la collection **Errors** avant d’appeler la méthode **Resync**, **UpdateBatch** ou **CancelBatch** sur un **objet Recordset** , la méthode **Open** sur un objet **Connection** ou définir la propriété **Filter** sur un objet **Recordset** . En effaçant explicitement la collection, vous pouvez être certain que tous les objets d' **erreur** de la collection ne sont pas conservés à partir d’une opération précédente.  
  
 Certaines opérations peuvent générer des avertissements en plus des erreurs. Les avertissements sont également représentés par des objets d' **erreur** dans la collection **Errors** . Lorsqu’un fournisseur ajoute un avertissement à la collection, il ne génère pas d’erreur au moment de l’exécution. Vérifiez la propriété **Count** de la collection **Errors** pour déterminer si un avertissement a été généré par une opération particulière. Si le nombre est supérieur ou égal à un, un objet d' **erreur** a été ajouté à la collection. Dès que vous avez déterminé que la collection d' **Erreurs** contient des erreurs ou des avertissements, vous pouvez effectuer une itération au sein de la collection et récupérer des informations sur chaque objet d' **erreur** qu’elle contient. L’exemple de Visual Basic succinct suivant illustre ceci :  
  
```  
' BeginErrorHandlingVB02  
Private Function DeleteCustomer(ByVal CompanyName As String) As Long  
    On Error GoTo DeleteCustomerError  
  
    rst.Find "CompanyName='" & CompanyName & "'"  
DeleteCustomerError:  
Dim objError As ADODB.Error  
Dim strError As String  
  
    If cnn.Errors.Count > 0 Then  
        For Each objError In cnn.Errors  
            strError = strError & "Error #" & objError.Number & _  
                " " & objError.Description & vbCrLf & _  
                "NativeError: " & objError.NativeError & vbCrLf & _  
                "SQLState: " & objError.SQLState & vbCrLf & _  
                "Reported by: " & objError.Source & vbCrLf & _  
                "Help file: " & objError.HelpFile & vbCrLf & _  
                "Help Context ID: " & objError.HelpContext  
        Next  
        MsgBox strError  
    End If  
End Function  
' EndErrorHandlingVB02  
```  
  
 La routine de gestion des erreurs comprend une boucle **for each** qui examine chaque objet dans la collection d' **Erreurs** . Dans cet exemple, il accumule un message à afficher. Dans un programme opérationnel, vous écrirez du code pour effectuer une tâche appropriée pour chaque erreur, par exemple fermer tous les fichiers ouverts et fermer le programme de manière ordonnée.  
  
## <a name="the-error-object"></a>Objet d’erreur  
 En examinant un objet d' **erreur** , vous pouvez déterminer l’erreur qui s’est produite et, plus important, l’application ou l’objet à l’origine de l’erreur. L’objet d' **erreur** a les propriétés suivantes :  
  
|Nom de la propriété|Description|  
|-------------------|-----------------|  
|**Description**|Description textuelle de l’erreur qui s’est produite.|  
|**HelpContext, HelpFile**|Fait référence à la rubrique d’aide et au fichier d’aide qui contiennent une description de l’erreur qui s’est produite.|  
|**NativeError**|Numéro d’erreur spécifique au fournisseur.|  
|**Nombre**|Entier long qui représente le nombre (listé dans **ErrorValueEnum**) de l’erreur qui s’est produite.|  
|**Source**|Indique le nom de l’objet ou de l’application qui a généré une erreur.|  
|**SQLState**|Code d’erreur à cinq caractères renvoyé par le fournisseur pendant le processus d’une instruction SQL.|  
  
 L’objet d' **erreur** ADO est très similaire à l’objet standard Visual Basic **Err** . Ses propriétés décrivent l’erreur qui s’est produite. Outre le numéro de l’erreur, vous recevez également deux informations associées. La propriété **NativeError** contient un numéro d’erreur spécifique au fournisseur que vous utilisez. Dans l’exemple précédent, le fournisseur est le fournisseur Microsoft OLE DB pour SQL Server. par conséquent, le **NativeError** contient des erreurs spécifiques à SQL Server. La propriété **SQLSTATE** comporte un code à cinq lettres qui décrit une erreur dans une instruction SQL.  
  
## <a name="event-related-errors"></a>Erreurs de Event-Related  
 L’objet d' **erreur** est également utilisé lorsque des erreurs liées aux événements se produisent. Vous pouvez déterminer si une erreur s’est produite dans le processus qui a déclenché un événement ADO en vérifiant l’objet d' **erreur** passé comme paramètre d’événement.  
  
 Si l’opération qui provoque un événement se termine correctement, le paramètre *adStatus* du gestionnaire d’événements est défini sur *adStatusOK*. En revanche, si l’opération qui a déclenché l’événement a échoué, le paramètre *adStatus* est défini sur *adStatusErrorsOccurred*. Dans ce cas, le paramètre *perror* contient un objet d' **erreur** qui décrit l’erreur.

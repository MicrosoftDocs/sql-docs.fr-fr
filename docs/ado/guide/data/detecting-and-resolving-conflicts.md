---
description: Détection et résolution des conflits
title: Détection et résolution des conflits | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- conflicts [ADO], detecting and resolving
- ADO, detecting and resolving conflicts
ms.assetid: b28fdd26-c1a4-40ce-a700-2b0c9d201514
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c92b847b95aacd5cf642c9646f4618dc5986abf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037629"
---
# <a name="detecting-and-resolving-conflicts"></a>Détection et résolution des conflits
Si vous traitez votre Recordset en mode immédiat, il y a beaucoup moins de risques de problèmes d’accès concurrentiel. En revanche, si votre application utilise la mise à jour en mode batch, il peut y avoir de bonnes chances qu’un utilisateur modifie un enregistrement avant que les modifications apportées par un autre utilisateur modifiant le même enregistrement soient enregistrées. Dans ce cas, vous souhaiterez que votre application gère correctement le conflit. Il se peut que vous souhaitiez que la dernière personne envoie une mise à jour au serveur « WINS ». Vous pouvez également permettre à l’utilisateur le plus récent de décider quelle mise à jour doit être prioritaire en lui fournissant un choix entre les deux valeurs conflictuelles.  
  
 Quel que soit le cas, ADO fournit les propriétés UnderlyingValue et OriginalValue de l’objet Field pour gérer ces types de conflits. Utilisez ces propriétés en association avec la méthode Resync et la propriété Filter de l’objet Recordset.  
  
## <a name="remarks"></a>Notes  
 Quand ADO rencontre un conflit pendant une mise à jour par lot, un avertissement est ajouté à la collection Errors. Par conséquent, vous devez toujours vérifier les erreurs immédiatement après avoir appelé BatchUpdate. Si vous les trouvez, commencez à tester l’hypothèse que vous avez rencontré un conflit. La première étape consiste à définir la propriété de filtre sur le jeu d’enregistrements comme étant égale à adFilterConflictingRecords. Cela limite la vue de votre Recordset uniquement aux enregistrements qui sont en conflit. Si la propriété RecordCount est égale à zéro après cette étape, vous savez que l’erreur a été déclenchée par autre chose qu’un conflit.  
  
 Quand vous appelez BatchUpdate, ADO et le fournisseur génèrent des instructions SQL pour effectuer des mises à jour sur la source de données. N’oubliez pas que certaines sources de données ont des limitations sur les types de colonnes qui peuvent être utilisés dans une clause WHERE.  
  
 Ensuite, appelez la méthode Resync sur le Recordset avec l’argument AffectRecords défini sur adAffectGroup et le jeu d’arguments ResyncValues égal à adResyncUnderlyingValues. La méthode Resync met à jour les données de l’objet Recordset actuel à partir de la base de données sous-jacente. En utilisant adAffectGroup, vous vous assurez que seuls les enregistrements visibles avec le paramètre de filtre actuel, c’est-à-dire uniquement les enregistrements en conflit, sont resynchronisés avec la base de données. Cela peut avoir une incidence significative sur les performances si vous traitez un jeu d’enregistrements volumineux. En définissant l’argument ResyncValues sur adResyncUnderlyingValues lors de l’appel de Resync, vous vous assurez que la propriété UnderlyingValue contiendra la valeur (en conflit) de la base de données, que la propriété Value conserve la valeur entrée par l’utilisateur, et que la propriété OriginalValue contiendra la valeur d’origine du champ (valeur qu’il avait avant la dernière opération UpdateBatch réussie). Vous pouvez ensuite utiliser ces valeurs pour résoudre le conflit par programme ou demander à l’utilisateur de sélectionner la valeur qui sera utilisée.  
  
 Cette technique est illustrée dans l’exemple de code suivant. L’exemple crée artificiellement un conflit à l’aide d’un jeu d’enregistrements distinct pour modifier une valeur dans la table sous-jacente avant l’appel de UpdateBatch.  
  
```  
'BeginConflicts  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT * FROM Shippers WHERE ShipperID = 2"  
  
    'Open Rs and change a value  
    Set objRs1 = New ADODB.Recordset  
    Set objRs2 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Introduce a conflict at the db...  
    objRs2.Open strSQL, strConn, adOpenKeyset, adLockOptimistic, adCmdText  
    objRs2("Phone") = "(999) 555-9999"  
    objRs2.Update  
    objRs2.Close  
    Set objRs2 = Nothing  
  
    On Error Resume Next  
    objRs1.UpdateBatch  
  
    If objRs1.ActiveConnection.Errors.Count <> 0 Then  
        Dim intConflicts As Integer  
  
        intConflicts = 0  
  
        objRs1.Filter = adFilterConflictingRecords  
  
        intConflicts = objRs1.RecordCount  
  
        'Resync so we can see the UnderlyingValue and offer user a choice.  
        'This sample only displays all three values and resets to original.  
        objRs1.Resync adAffectGroup, adResyncUnderlyingValues  
  
        If intConflicts > 0 Then  
            strMsg = "A conflict occurred with updates for " & intConflicts & _  
                     " record(s)." & vbCrLf & "The values will be restored" & _  
                     " to their original values." & vbCrLf & vbCrLf  
  
            objRs1.MoveFirst  
            While Not objRs1.EOF  
              strMsg = strMsg & "SHIPPER = " & objRs1("CompanyName") & vbCrLf  
              strMsg = strMsg & "Value = " & objRs1("Phone").Value & vbCrLf  
              strMsg = strMsg & "UnderlyingValue = " & _  
                                 objRs1("Phone").UnderlyingValue & vbCrLf  
              strMsg = strMsg & "OriginalValue = " & _  
                                 objRs1("Phone").OriginalValue & vbCrLf  
              strMsg = strMsg & vbCrLf & "Original value has been restored."  
  
              MsgBox strMsg, vbOKOnly, _  
                    "Conflict " & objRs1.AbsolutePosition & _  
                    " of " & intConflicts  
  
              objRs1("Phone").Value = objRs1("Phone").OriginalValue  
              objRs1.MoveNext  
            Wend  
  
            objRs1.UpdateBatch adAffectGroup  
        Else  
            'Other error occurred. Minimal handling in this example.  
             strMsg = "Errors occurred during the update. " & _  
                        objRs1.ActiveConnection.Errors(0).Number & " " & _  
                        objRs1.ActiveConnection.Errors(0).Description  
        End If  
  
        On Error GoTo 0  
    End If  
  
    objRs1.MoveFirst  
    objRs1.Close  
    Set objRs1 = Nothing  
'EndConflicts  
```  
  
 Vous pouvez utiliser la propriété Status de l’enregistrement en cours ou d’un champ spécifique pour déterminer le type de conflit qui s’est produit.  
  
 Pour plus d’informations sur la gestion des erreurs, consultez [gestion des erreurs](./error-handling.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Mode Batch](./batch-mode.md)
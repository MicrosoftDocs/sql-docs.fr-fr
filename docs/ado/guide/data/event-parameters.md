---
description: Paramètres des événements
title: Paramètres d’événement | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Error parameter [ADO]
- Object parameter [ADO]
- Status parameter [ADO]
- events [ADO], parameters
- Reason parameter [ADO]
- event parameters [ADO]
ms.assetid: bd5c5afa-d301-4899-acda-40f98a6afa4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f60d32ccd968c39da4fcea3978a46389e49ae49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033209"
---
# <a name="event-parameters"></a>Paramètres des événements
Chaque gestionnaire d’événements a un paramètre d’État qui contrôle le gestionnaire d’événements. Pour les événements complets, ce paramètre est également utilisé pour indiquer la réussite ou l’échec de l’opération qui a généré l’événement. La plupart des événements complets ont également un paramètre d’erreur qui fournit des informations sur les erreurs qui ont pu se produire, ainsi qu’un ou plusieurs paramètres d’objet qui font référence aux objets ADO utilisés pour effectuer l’opération. Par exemple, l’événement [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md) comprend les paramètres d’objet de la **commande**, du **Recordset** et des objets de **connexion** associés à l’événement. Dans l’exemple Microsoft® Visual Basic®, vous pouvez voir les objets pCommand, prerecordset et pConnection qui représentent les objets **Command**, **Recordset** et **Connection** utilisés par la méthode **Execute** .  
  
```  
Private Sub connEvent_ExecuteComplete(ByVal RecordsAffected As Long, _  
     ByVal pError As ADODB.Error, _  
     adStatus As ADODB.EventStatusEnum, _  
     ByVal pCommand As ADODB.Command, _  
     ByVal pRecordset As ADODB.Recordset, _  
     ByVal pConnection As ADODB.Connection)  
```  
  
 À l’exception de l’objet **Error** , les mêmes paramètres sont passés aux événements. Cela vous permet d’examiner chacun des objets qui seront utilisés dans l’opération en attente et de déterminer si l’opération doit être autorisée à se terminer.  
  
 Certains gestionnaires d’événements ont un paramètre *reason* , qui fournit des informations supplémentaires sur la raison pour laquelle l’événement s’est produit. Par exemple, les événements **WillMove** et **MoveComplete** peuvent se produire en raison de l’appel d’une des méthodes de navigation (**MoveNext**, **MovePrevious**, etc.) ou du résultat d’une rerequête.  
  
## <a name="status-parameter"></a>Paramètre d’État  
 Lorsque la routine du gestionnaire d’événements est appelée, le paramètre *Status* est défini sur l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**adStatusOK**|Passé aux événements de réussite et d’achèvement. Cette valeur signifie que l’opération qui a provoqué l’événement s’est terminée avec succès.|  
|**adStatusErrorsOccurred**|Transmis aux événements complets uniquement. Cette valeur signifie que l’opération qui a provoqué l’événement a échoué, ou qu’un événement d’annulation de l’opération a été annulé. Pour plus d’informations, vérifiez le paramètre d' *erreur* .|  
|**adStatusCantDeny**|Passé aux événements uniquement. Cette valeur signifie que l’opération ne peut pas être annulée par l’événement. Elle doit être effectuée.|  
  
 Si vous déterminez dans votre événement que l’opération doit se poursuivre, laissez le paramètre d' *État* inchangé. Toutefois, tant que le paramètre d’État entrant n’a pas la valeur **adStatusCantDeny**, vous pouvez annuler l’opération en attente en remplaçant l' *État* par **adStatusCancel**. Dans ce cas, le paramètre *Status* de l’événement complet associé à l’opération est défini sur **adStatusErrorsOccurred**. L’objet d' **erreur** transmis à l’événement Complete contient la valeur **adErrOperationCancelled**.  
  
 Si vous ne souhaitez plus traiter un événement, vous pouvez définir l' *État* sur **adStatusUnwantedEvent** et votre application ne recevra plus de notification de cet événement. Toutefois, n’oubliez pas que certains événements peuvent être déclenchés pour plusieurs raisons. Dans ce cas, vous devez spécifier **adStatusUnwantedEvent** pour chaque raison possible. Par exemple, pour cesser de recevoir des notifications d’événements **RecordChange** en attente, vous devez définir le paramètre *Status* sur **adStatusUnwantedEvent** pour **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete** et **adRsnFirstChange** lorsqu’ils se produisent.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**adStatusUnwantedEvent**|Demandez que ce gestionnaire d’événements ne reçoive plus de notifications.|  
|**adStatusCancel**|Demande d’annulation de l’opération qui est sur le lieu de se produire.|  
  
## <a name="error-parameter"></a>Paramètre d’erreur  
 Le paramètre d' *erreur* est une référence à un objet d' [erreur](../../reference/ado-api/error-object.md) ADO. Lorsque le paramètre *Status* est défini sur **adStatusErrorsOccurred**, l’objet **Error** contient des détails sur la raison de l’échec de l’opération. Si l’événement qui est associé à un événement Complete a annulé l’opération en définissant le paramètre *Status* sur **adStatusCancel**, l’objet Error est toujours défini sur **adErrOperationCancelled**.  
  
## <a name="object-parameter"></a>Paramètre de l’objet  
 Chaque événement reçoit un ou plusieurs objets représentant les objets impliqués dans l’opération. Par exemple, l’événement **ExecuteComplete** reçoit un objet **Command** , un objet **Recordset** et un objet **Connection** .  
  
## <a name="reason-parameter"></a>Paramètre Reason  
 Le *paramètre Reason* , *adReason*, fournit des informations supplémentaires sur la raison pour laquelle l’événement s’est produit. Les événements avec un paramètre *adReason* peuvent être appelés plusieurs fois, même pour une même opération, pour une raison différente à chaque fois. Par exemple, le gestionnaire d’événements **WillChangeRecord** est appelé pour les opérations qui sont sur le point d’effectuer ou d’annuler l’insertion, la suppression ou la modification d’un enregistrement. Si vous souhaitez traiter un événement uniquement lorsqu’il se produit pour une raison particulière, vous pouvez utiliser le paramètre *adReason* pour filtrer les occurrences qui ne vous intéressent pas. Par exemple, si vous souhaitez traiter les événements de modification d’enregistrement uniquement lorsqu’un enregistrement a été ajouté, vous pouvez utiliser une commande semblable à la suivante.  
  
```  
' BeginEventExampleVB01  
Private Sub rsTest_WillChangeRecord(ByVal adReason As ADODB.EventReasonEnum, ByVal cRecords As Long, adStatus As ADODB.EventStatusEnum, ByVal pRecordset As ADODB.Recordset)  
   If adReason = adRsnAddNew Then  
       ' Process event  
       '...  
   Else  
       ' Cancel event notification for all  
       ' other possible adReason values.  
       adStatus = adStatusUnwantedEvent  
   End If  
End Sub  
' EndEventExampleVB01  
```  
  
 Dans ce cas, la notification peut éventuellement se produire pour chacune des autres raisons. Toutefois, elle n’aura lieu qu’une seule fois pour chaque raison. Une fois la notification effectuée une fois pour chaque raison, vous recevrez une notification uniquement pour l’ajout d’un nouvel enregistrement.  
  
 En revanche, vous devez définir *adStatus* sur **adStatusUnwantedEvent** une seule fois pour demander qu’un gestionnaire d’événements sans paramètre **adReason** cesse de recevoir des notifications d’événements.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du gestionnaire d’événements ADO](./ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](./ado-event-instantiation-by-language.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](./how-event-handlers-work-together.md)   
 [Types d’événements](./types-of-events.md)
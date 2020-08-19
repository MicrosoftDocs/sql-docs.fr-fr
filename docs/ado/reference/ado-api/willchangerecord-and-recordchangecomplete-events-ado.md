---
description: WillChangeRecord et RecordChangeComplete, événements (ADO)
title: WillChangeRecord et RecordChangeComplete, événements (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- RecordChangeComplete
- Recordset::WillChangeRecord
- WillChangeRecord
- Recordset::RecordChangeComplete
helpviewer_keywords:
- WillChangeRecord event [ADO]
- recordchangecomplete event [ADO]
ms.assetid: cbc369fd-63af-4a7d-96ae-efa91b78ca69
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a6cb124e51c232b0a3a26e9eb84316e3bde7ecd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441521"
---
# <a name="willchangerecord-and-recordchangecomplete-events-ado"></a>WillChangeRecord et RecordChangeComplete, événements (ADO)
L’événement **WillChangeRecord** est appelé avant qu’un ou plusieurs enregistrements (lignes) du [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) soient modifiés. L’événement **RecordChangeComplete** est appelé après la modification d’un ou de plusieurs enregistrements.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillChangeRecord adReason, cRecords, adStatus, pRecordset  
RecordChangeCompleteadReason, cRecords, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Reason*  
 Valeur [EventReasonEnum](../../../ado/reference/ado-api/eventreasonenum.md) qui spécifie la raison de cet événement. Sa valeur peut être **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete**ou **adRsnFirstChange**.  
  
 *cRecords*  
 Valeur de **type long** qui indique le nombre d’enregistrements en modification (affectés).  
  
 *pError*  
 Objet d' [erreur](../../../ado/reference/ado-api/error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de *adStatus* est **adStatusErrorsOccurred**; dans le cas contraire, il n’est pas défini.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 Quand **WillChangeRecord** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi. Elle a la valeur **adStatusCantDeny** si cet événement ne peut pas demander l’annulation de l’opération en attente.  
  
 Quand **RecordChangeComplete** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement s’est déroulée correctement ou **adStatusErrorsOccurred** si l’opération a échoué.  
  
 Avant le retour de **WillChangeRecord** , affectez à ce paramètre la valeur **adStatusCancel** pour demander l’annulation de l’opération qui a provoqué cet événement ou affectez la valeur **adStatusUnwantedEvent** à ce paramètre pour empêcher les notifications suivantes.  
  
 Avant le retour de **RecordChangeComplete** , définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pRecordset*  
 Objet **Recordset** . **Jeu d’enregistrements** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Un événement **WillChangeRecord** ou **RecordChangeComplete** peut se produire pour le premier champ modifié dans une ligne en raison des opérations suivantes du **Recordset** : [Update](../../../ado/reference/ado-api/update-method.md), [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md), [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md), [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)et [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md). La valeur de la valeur de la [CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md) de l’ensemble d' **enregistrements** détermine les opérations qui déclenchent les événements.  
  
 Pendant l’événement **WillChangeRecord** , la propriété de [filtre](../../../ado/reference/ado-api/filter-property.md) **Recordset** est définie sur **adFilterAffectedRecords**. Vous ne pouvez pas modifier cette propriété lors du traitement de l’événement.  
  
 Vous devez définir le paramètre **adStatus** sur **adStatusUnwantedEvent** pour chaque valeur de **adReason** possible afin d’arrêter complètement la notification d’événement pour tout événement qui comprend un paramètre **adReason** .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)

---
description: WillChangeRecordset et RecordsetChangeComplete, événements (ADO)
title: WillChangeRecordset et RecordsetChangeComplete, événements (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset::RecordsetChangeComplete
- RecordsetChangeComplete
- Recordset::WillChangeRecordset
- WillChangeRecordset
helpviewer_keywords:
- RecordsetChangeComplete event [ADO]
- WillChangeRecordset event [ADO]
ms.assetid: d5d44659-e0d9-46d9-a297-99c43555082f
author: rothja
ms.author: jroth
ms.openlocfilehash: 6246eaa8dbdd92236a891fa56189e104b594b64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172353"
---
# <a name="willchangerecordset-and-recordsetchangecomplete-events-ado"></a>WillChangeRecordset et RecordsetChangeComplete, événements (ADO)
L’événement **WillChangeRecordset** est appelé avant qu’une opération en attente modifie le [Recordset](./recordset-object-ado.md). L’événement **RecordsetChangeComplete** est appelé après la modification du **Recordset** .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillChangeRecordset adReason, adStatus, pRecordset  
RecordsetChangeComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Reason*  
 Valeur [EventReasonEnum](./eventreasonenum.md) qui spécifie la raison de cet événement. Sa valeur peut être **adRsnRequery**, **adRsnResynch**, **adRsnClose**, **adRsnOpen**.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) .  
  
 Lorsque **WillChangeRecordset** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi. Elle a la valeur **adStatusCantDeny** si cet événement ne peut pas demander l’annulation de l’opération en attente.  
  
 Quand **RecordsetChangeComplete** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération qui a provoqué l’événement a réussi, **adStatusErrorsOccurred** si l’opération a échoué, ou **adStatusCancel** si l’opération associée à l’événement **WillChangeRecordset** précédemment accepté a été annulée.  
  
 Avant le retour de **WillChangeRecordset** , définissez ce paramètre sur **adStatusCancel** pour demander l’annulation de l’opération en attente, ou définissez ce paramètre sur adStatusUnwantedEvent pour empêcher les notifications suivantes.  
  
 Avant que **WillChangeRecordset** ou **RecordsetChangeComplete** ne retourne, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pError*  
 Objet d' [erreur](./error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de *adStatus* est **adStatusErrorsOccurred**; dans le cas contraire, il n’est pas défini.  
  
 *pRecordset*  
 Objet **Recordset** . **Jeu d’enregistrements** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Un événement **WillChangeRecordset** ou **RecordsetChangeComplete** peut se produire en raison de la [rerequête](./requery-method.md) de l’ensemble **d’enregistrements** ou des méthodes [ouvertes](./open-method-ado-recordset.md) .  
  
 Si le fournisseur ne prend pas en charge les signets, une notification d’événement **RecordsetChange** se produit chaque fois que de nouvelles lignes sont extraites du fournisseur. La fréquence de cet événement dépend de la propriété **RecordsetCacheSize** .  
  
 Vous devez définir le paramètre **adStatus** sur **adStatusUnwantedEvent** pour chaque valeur de **adReason** possible afin d’arrêter complètement la notification d’événement pour tout événement qui comprend un paramètre **adReason** .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)
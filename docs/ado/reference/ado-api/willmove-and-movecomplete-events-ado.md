---
description: WillMove et MoveComplete, événements (ADO)
title: WillMove et MoveComplete, événements (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset::MoveComplete
- WillMove
- MoveComplete
- Recordset::WillMove
helpviewer_keywords:
- MoveComplete event [ADO]
- WillMove event [ADO]
ms.assetid: 1a3d1042-4f30-4526-a0c7-853c242496db
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c992f95ae9caf96708f5fcde0c255ff8c7c6f11
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441501"
---
# <a name="willmove-and-movecomplete-events-ado"></a>WillMove et MoveComplete, événements (ADO)
L’événement **WillMove** est appelé avant qu’une opération en attente modifie la position actuelle dans l’ensemble [d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md). L’événement **MoveComplete** est appelé après la modification de la position actuelle dans le **jeu d’enregistrements** .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillMove adReason, adStatus, pRecordset  
MoveComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Reason*  
 Valeur [EventReasonEnum](../../../ado/reference/ado-api/eventreasonenum.md) qui spécifie la raison de cet événement. Sa valeur peut être **adRsnMoveFirst**, **adRsnMoveLast**, **adRsnMoveNext**, **adRsnMovePrevious**, **adRsnMove**ou **adRsnRequery**.  
  
 *pError*  
 Objet d' [erreur](../../../ado/reference/ado-api/error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de *adStatus* est **adStatusErrorsOccurred**; dans le cas contraire, le paramètre n’est pas défini.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 Lorsque **WillMove** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi. Elle a la valeur **adStatusCantDeny** si cet événement ne peut pas demander l’annulation de l’opération en attente.  
  
 Quand **MoveComplete** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement s’est déroulée correctement ou **adStatusErrorsOccurred** si l’opération a échoué.  
  
 Avant que **WillMove** retourne, affectez à ce paramètre la valeur **adStatusCancel** pour demander l’annulation de l’opération en attente, ou affectez la valeur **adStatusUnwantedEvent** à ce paramètre pour empêcher les notifications suivantes.  
  
 Avant que **MoveComplete** retourne, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pRecordset*  
 Objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) . **Jeu d’enregistrements** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Un événement **WillMove** ou **MoveComplete** peut se produire en raison des opérations suivantes sur les **jeux d’enregistrements** : [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md), [Move](../../../ado/reference/ado-api/move-method-ado.md), [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveLast](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md)et [Requery](../../../ado/reference/ado-api/requery-method.md). Ces événements peuvent se produire en raison des propriétés suivantes : [filtre](../../../ado/reference/ado-api/filter-property.md), [index](../../../ado/reference/ado-api/index-property.md), [signet](../../../ado/reference/ado-api/bookmark-property-ado.md), [AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md)et [AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md). Ces événements se produisent également si un **jeu d'** enregistrements enfant contient des événements **Recordset** connectés et que le **jeu d’enregistrements** parent est déplacé.  
  
 Vous devez définir le paramètre *adStatus* sur **adStatusUnwantedEvent** pour chaque valeur *adReason* possible afin d’arrêter complètement la notification d’événement pour tout événement qui comprend un paramètre *adReason* .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Résumé du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)

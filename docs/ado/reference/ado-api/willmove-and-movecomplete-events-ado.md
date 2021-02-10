---
description: WillMove et MoveComplete, événements (ADO)
title: WillMove et MoveComplete, événements (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
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
ms.openlocfilehash: a2476447e5da8baa89a2df326eba3d41225c698e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056054"
---
# <a name="willmove-and-movecomplete-events-ado"></a>WillMove et MoveComplete, événements (ADO)
L’événement **WillMove** est appelé avant qu’une opération en attente modifie la position actuelle dans l’ensemble [d’enregistrements](./recordset-object-ado.md). L’événement **MoveComplete** est appelé après la modification de la position actuelle dans le **jeu d’enregistrements** .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillMove adReason, adStatus, pRecordset  
MoveComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Reason*  
 Valeur [EventReasonEnum](./eventreasonenum.md) qui spécifie la raison de cet événement. Sa valeur peut être **adRsnMoveFirst**, **adRsnMoveLast**, **adRsnMoveNext**, **adRsnMovePrevious**, **adRsnMove** ou **adRsnRequery**.  
  
 *pError*  
 Objet d' [erreur](./error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de *adStatus* est **adStatusErrorsOccurred**; dans le cas contraire, le paramètre n’est pas défini.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) .  
  
 Lorsque **WillMove** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi. Elle a la valeur **adStatusCantDeny** si cet événement ne peut pas demander l’annulation de l’opération en attente.  
  
 Quand **MoveComplete** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement s’est déroulée correctement ou **adStatusErrorsOccurred** si l’opération a échoué.  
  
 Avant que **WillMove** retourne, affectez à ce paramètre la valeur **adStatusCancel** pour demander l’annulation de l’opération en attente, ou affectez la valeur **adStatusUnwantedEvent** à ce paramètre pour empêcher les notifications suivantes.  
  
 Avant que **MoveComplete** retourne, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pRecordset*  
 Objet [Recordset](./recordset-object-ado.md) . **Jeu d’enregistrements** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Un événement **WillMove** ou **MoveComplete** peut se produire en raison des opérations suivantes sur les **jeux d’enregistrements** : [Open](./open-method-ado-recordset.md), [Move](./move-method-ado.md), [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveLast](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveNext](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [AddNew](./addnew-method-ado.md)et [Requery](./requery-method.md). Ces événements peuvent se produire en raison des propriétés suivantes : [filtre](./filter-property.md), [index](./index-property.md), [signet](./bookmark-property-ado.md), [AbsolutePage](./absolutepage-property-ado.md)et [AbsolutePosition](./absoluteposition-property-ado.md). Ces événements se produisent également si un **jeu d'** enregistrements enfant contient des événements **Recordset** connectés et que le **jeu d’enregistrements** parent est déplacé.  
  
 Vous devez définir le paramètre *adStatus* sur **adStatusUnwantedEvent** pour chaque valeur *adReason* possible afin d’arrêter complètement la notification d’événement pour tout événement qui comprend un paramètre *adReason* .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Résumé du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)
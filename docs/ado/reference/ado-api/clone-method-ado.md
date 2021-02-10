---
description: Clone, méthode (ADO)
title: Clone, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Clone
- Recordset20::raw_Clone
helpviewer_keywords:
- Clone method [ADO]
ms.assetid: ad49265f-1c05-4271-9bbf-7c00010ac18c
author: rothja
ms.author: jroth
ms.openlocfilehash: 57bf5f2a4e9134ffdd497133c44741af8a84f2ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027107"
---
# <a name="clone-method-ado"></a>Clone, méthode (ADO)
Crée un objet [Recordset](./recordset-object-ado.md) dupliqué à partir d’un objet **Recordset** existant. Spécifie éventuellement que le clone doit être en lecture seule.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set rstDuplicate = rstOriginal.Clone (LockType)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une référence **d’objet Recordset** .  
  
#### <a name="parameters"></a>Paramètres  
 *rstDuplicate*  
 Variable objet qui identifie l’objet **Recordset** dupliqué à créer.  
  
 *rstOriginal*  
 Variable objet qui identifie l’objet **Recordset** à dupliquer.  
  
 *Verrou*  
 facultatif. Valeur [LockTypeEnum](./locktypeenum.md) qui spécifie le type de verrou du **Recordset** d’origine ou un **jeu d’enregistrements** en lecture seule. Les valeurs valides sont **adLockUnspecified** ou **adLockReadOnly**.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **clone** pour créer plusieurs objets **Recordset** dupliqués, en particulier si vous souhaitez conserver plusieurs enregistrements actifs dans un ensemble donné d’enregistrements. L’utilisation de la méthode **clone** est plus efficace que la création et l’ouverture d’un nouvel objet **Recordset** qui utilise la même définition que l’original.  
  
 La propriété [Filter](./filter-property.md) du **Recordset** d’origine, le cas échéant, ne sera pas appliquée au clone. Définissez la propriété **Filter** du nouvel ensemble d' **enregistrements** pour filtrer les résultats. La façon la plus simple de copier une valeur de **filtre** existante consiste à l’assigner directement, comme suit.  
  
```  
rsNew.Filter = rsOriginal.Filter  
```  
  
 L’enregistrement actuel d’un clone nouvellement créé est défini sur le premier enregistrement.  
  
 Les modifications que vous apportez à un objet **Recordset** sont visibles dans tous ses clones, quel que soit le type de curseur. Toutefois, après l’exécution de [Requery](./requery-method.md) sur le **jeu d’enregistrements** d’origine, les clones ne sont plus synchronisés avec l’original.  
  
 La fermeture de l' **objet Recordset** d’origine ne ferme pas ses copies et ne ferme pas non plus la copie d’origine ou de l’une des autres copies.  
  
 Vous pouvez cloner un objet **Recordset** qui prend en charge les signets uniquement. Les valeurs de signet sont interchangeables ; autrement dit, une référence de signet d’un objet **Recordset** fait référence au même enregistrement dans l’un de ses clones.  
  
 Certains événements du **Recordset** qui sont déclenchés se produisent également dans tous les clones de l’ensemble **d’enregistrements** . Toutefois, étant donné que l’enregistrement en cours peut différer d’un **jeu d’enregistrements** cloné à un autre, les événements peuvent ne pas être valides pour le clone. Par exemple, si vous modifiez la valeur d’un champ, un événement [WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md) se produit dans le **jeu d’enregistrements** modifié et dans tous les clones. Le paramètre *Fields* de l’événement **WillChangeField** d’un **jeu d’enregistrements** cloné (où la modification n’a pas été apportée) fait référence aux champs de l’enregistrement actuel du clone, qui peut être un enregistrement différent de l’enregistrement actif du **Recordset** d’origine où la modification s’est produite.  
  
 Le tableau suivant fournit une liste complète de tous les événements du **Recordset** . Elle indique si elles sont valides et déclenchées pour tous les clones d’ensemble d’enregistrements générés à l’aide de la méthode **clone** .  
  
|Événement|Déclenché dans les clones ?|  
|-----------|--------------------------|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|Non|  
|[FetchComplete](./fetchcomplete-event-ado.md)|Non|  
|[FetchProgress](./fetchprogress-event-ado.md)|Non|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|Oui|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|Non|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|Oui|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Non|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|Oui|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|Oui|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Non|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|Non|  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Clone, exemple de méthode (VB)](./clone-method-example-vb.md)   
 [Clone, exemple de méthode (VBScript)](./clone-method-example-vbscript.md)   
 [Clone, exemple de méthode (VC++)](./clone-method-example-vc.md)
---
description: EndOfRecordset, événement (ADO)
title: EndOfRecordset, événement (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EndOfRecordset
- Recordset::EndOfRecordset
helpviewer_keywords:
- EndOfRecordset event [ADO]
ms.assetid: 475de5e2-f634-4954-9edf-0027a6ba38d6
author: rothja
ms.author: jroth
ms.openlocfilehash: 9369b22c012773627c17da55a336aac116dc4e06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171226"
---
# <a name="endofrecordset-event-ado"></a>EndOfRecordset, événement (ADO)
L’événement **EndOfRecordset** est appelé en cas de tentative de déplacement vers une ligne située au-delà de la fin du [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EndOfRecordset fMoreData, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *fMoreData*  
 Valeur **VARIANT_BOOL** qui, si elle est définie sur VARIANT_TRUE, indique que des lignes supplémentaires ont été ajoutées au **Recordset**.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 Lorsque **EndOfRecordset** est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi. Elle a la valeur **adStatusCantDeny** si cet événement ne peut pas demander l’annulation de l’opération qui a provoqué cet événement.  
  
 Avant le retour de **EndOfRecordset** , définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pRecordset*  
 Objet **Recordset** . **Jeu d’enregistrements** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Un événement **EndOfRecordset** peut se produire si l’opération [MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) échoue.  
  
 Ce gestionnaire d’événements est appelé en cas de tentative de déplacement au-delà de la fin de l’objet **Recordset** , peut-être en raison de l’appel de **MoveNext**. Toutefois, dans ce cas, vous pouvez récupérer plus d’enregistrements à partir d’une base de données et les ajouter à la fin de l’ensemble d' **enregistrements**. Dans ce cas, affectez à *fMoreData* la valeur VARIANT_TRUE et retournez à partir de **EndOfRecordset**. Appelez ensuite **MoveNext** à nouveau pour accéder aux enregistrements récupérés récemment.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)

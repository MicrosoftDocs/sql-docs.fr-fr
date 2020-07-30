---
title: EventReasonEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- EventReasonEnum
helpviewer_keywords:
- EventReasonEnum enumeration [ADO]
ms.assetid: 7d4a5496-ec2d-4936-b36a-7049a82be4b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 94b36cbab5ffe7c22f4d1941e61af8fabc8b9973
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87242729"
---
# <a name="eventreasonenum"></a>EventReasonEnum
Spécifie la raison pour laquelle un événement a eu lieu.  
  
|Constant|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adRsnAddNew**|1|Une opération a ajouté un nouvel enregistrement.|  
|**adRsnClose**|9|Une opération a fermé le **Recordset**.|  
|**adRsnDelete**|2|Une opération a supprimé un enregistrement.|  
|**adRsnFirstChange**|11|Une opération a apporté la première modification à un enregistrement.|  
|**adRsnMove**|10|Une opération a déplacé le pointeur d’enregistrement dans le **jeu d’enregistrements**.|  
|**adRsnMoveFirst**|12|Une opération a déplacé le pointeur d’enregistrement vers le premier enregistrement dans le **jeu d’enregistrements**.|  
|**adRsnMoveLast**|15|Une opération a déplacé le pointeur d’enregistrement vers le dernier enregistrement du **Recordset**.|  
|**adRsnMoveNext**|13|Une opération a déplacé le pointeur d’enregistrement vers l’enregistrement suivant dans le **Recordset**.|  
|**adRsnMovePrevious**|14|Une opération a déplacé le pointeur d’enregistrement vers l’enregistrement précédent dans le **Recordset**.|  
|**adRsnRequery**|7|Une opération a réinterrogé le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).|  
|**adRsnResynch**|8|Une opération a resynchronisé le **Recordset** avec la base de données.|  
|**adRsnUndoAddNew**|5|Une opération a inversé l’ajout d’un nouvel enregistrement.|  
|**adRsnUndoDelete**|6|Une opération a inversé la suppression d’un enregistrement.|  
|**adRsnUndoUpdate**|4|Une opération a inversé la mise à jour d’un enregistrement.|  
|**adRsnUpdate**|3|Une opération a mis à jour un enregistrement existant.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constant|  
|--------------|  
|AdoEnums. EventReason. ADDNEW|  
|AdoEnums.EventReason.CLOSE|  
|AdoEnums. EventReason. DELETE|  
|AdoEnums. EventReason. FIRSTCHANGE|  
|AdoEnums. EventReason. MOVE|  
|AdoEnums.EventReason.MOVEFIRST|  
|AdoEnums. EventReason. MOVELAST|  
|AdoEnums. EventReason. MOVENEXT|  
|AdoEnums. EventReason. MOVEPREVIOUS|  
|AdoEnums. EventReason. Requery|  
|AdoEnums. EventReason. Resynch|  
|AdoEnums.EventReason.UNDOADDNEW|  
|AdoEnums.EventReason.UNDODELETE|  
|AdoEnums.EventReason.UNDOUPDATE|  
|AdoEnums.EventReason.UPDATE|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [WillChangeRecord et RecordChangeComplete, événements (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)  

        [WillChangeRecordset et RecordsetChangeComplete, événements (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)  
    :::column-end:::
    :::column:::
        [WillMove et MoveComplete, événements (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)  
    :::column-end:::
:::row-end:::

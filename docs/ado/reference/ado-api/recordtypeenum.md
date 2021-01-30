---
description: RecordTypeEnum
title: RecordTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordTypeEnum
helpviewer_keywords:
- RecordTypeEnum enumeration [ADO]
ms.assetid: f557e537-015d-4ba7-8a41-a6f00b366a91
author: rothja
ms.author: jroth
ms.openlocfilehash: 8ef671384a552f240930e24a073f979f6e84da8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166679"
---
# <a name="recordtypeenum"></a>RecordTypeEnum
Spécifie le type d’objet d' [enregistrement](./record-object-ado.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adSimpleRecord**|0|Indique un enregistrement *simple* (ne contient pas de nœuds enfants).|  
|**adCollectionRecord**|1|Indique un enregistrement de *collection* (contient des nœuds enfants).|  
|**adRecordUnknown**|-1|Indique que le type de cet **enregistrement** est inconnu.|  
|**adStructDoc**|2|Indique un type spécial d’enregistrement de *collection* qui représente des documents structurés com.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [RecordType, propriété (ADO)](./recordtype-property-ado.md)
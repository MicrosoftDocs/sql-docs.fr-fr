---
description: StreamReadEnum
title: StreamReadEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamReadEnum
helpviewer_keywords:
- StreamReadEnum enumeration [ADO]
ms.assetid: cfa1b416-003a-436f-a21b-bd2397e54db3
author: rothja
ms.author: jroth
ms.openlocfilehash: 5200d39c0dfe9d79fa0adaab4c2a3aeb9060c9d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170118"
---
# <a name="streamreadenum"></a>StreamReadEnum
Spécifie si l’intégralité du flux ou la ligne suivante doit être lue à partir d’un objet de [flux](./stream-object-ado.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adReadAll**|-1|Par défaut. Lit tous les octets du flux, à partir de la position actuelle jusqu’au marqueur [EOS](./eos-property.md) . Il s’agit de la seule valeur **StreamReadEnum** valide avec des flux binaires ([type](./type-property-ado-stream.md) **adTypeBinary**).|  
|**adReadLine**|-2|Lit la ligne suivante dans le flux (désigné par la propriété [LineSeparator](./lineseparator-property-ado.md) ).|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Read, méthode](./read-method.md)  
    :::column-end:::
    :::column:::
        [ReadText, méthode](./readtext-method.md)  
    :::column-end:::
:::row-end:::
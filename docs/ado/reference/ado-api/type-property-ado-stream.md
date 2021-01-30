---
description: Type, propriété (objet Stream ADO)
title: Type, propriété (ADO Stream) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Type
- _Stream::get_Type
- _Stream::put_Type
helpviewer_keywords:
- Type property [ADO Stream]
ms.assetid: f6a17e8c-7a28-48d0-bded-76b9e0cf7639
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e170cd368771fc75c2c6e552d4465dfe51866c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166374"
---
# <a name="type-property-ado-stream"></a>Type, propriété (objet Stream ADO)
Indique le type de données contenues dans le [flux](./stream-object-ado.md) (binaire ou texte).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [StreamTypeEnum](./streamtypeenum.md) qui spécifie le type de données contenu dans l’objet de **flux** . La valeur par défaut est **adTypeText**. Toutefois, si des données binaires sont écrites initialement dans un nouveau **flux** vide, le **type** sera remplacé par **adTypeBinary**.  
  
## <a name="remarks"></a>Notes  
 La propriété de **type** est en lecture/écriture uniquement lorsque la position actuelle est au début du **flux** ([position](./position-property-ado.md) est 0) et en lecture seule à n’importe quelle autre position.  
  
 La propriété de **type** détermine les méthodes qui doivent être utilisées pour lire et écrire dans le **flux**. Pour les **flux** de texte, utilisez [READTEXT](./readtext-method.md) et [WRITETEXT](./writetext-method.md). Pour les **flux** binaires, utilisez [Read](./read-method.md) et [Write](./write-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [RecordType, propriété (ADO)](./recordtype-property-ado.md)   
 [Type, propriété (ADO)](./type-property-ado.md)
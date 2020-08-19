---
description: Type, propriété (objet Stream ADO)
title: Type, propriété (ADO Stream) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
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
ms.openlocfilehash: 8fd1519092d72f8a562bb266d2aa6d547d8e37df
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441731"
---
# <a name="type-property-ado-stream"></a>Type, propriété (objet Stream ADO)
Indique le type de données contenues dans le [flux](../../../ado/reference/ado-api/stream-object-ado.md) (binaire ou texte).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [StreamTypeEnum](../../../ado/reference/ado-api/streamtypeenum.md) qui spécifie le type de données contenu dans l’objet de **flux** . La valeur par défaut est **adTypeText**. Toutefois, si des données binaires sont écrites initialement dans un nouveau **flux**vide, le **type** sera remplacé par **adTypeBinary**.  
  
## <a name="remarks"></a>Notes  
 La propriété de **type** est en lecture/écriture uniquement lorsque la position actuelle est au début du **flux** ([position](../../../ado/reference/ado-api/position-property-ado.md) est 0) et en lecture seule à n’importe quelle autre position.  
  
 La propriété de**type** détermine les méthodes qui doivent être utilisées pour lire et écrire dans le **flux**. Pour les **flux**de texte, utilisez [READTEXT](../../../ado/reference/ado-api/readtext-method.md) et [WRITETEXT](../../../ado/reference/ado-api/writetext-method.md). Pour les **flux**binaires, utilisez [Read](../../../ado/reference/ado-api/read-method.md) et [Write](../../../ado/reference/ado-api/write-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [RecordType, propriété (ADO)](../../../ado/reference/ado-api/recordtype-property-ado.md)   
 [Type, propriété (ADO)](../../../ado/reference/ado-api/type-property-ado.md)

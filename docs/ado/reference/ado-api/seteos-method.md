---
description: SetEOS, méthode
title: SetEos, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SetEOS
- _Stream::SetEOS
helpviewer_keywords:
- SetEOS method [ADO]
ms.assetid: 707c18ca-6a56-4970-bbd6-ae1fb86a0b8a
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b9685f0ca0b5e9bdd1b484c7d424308619307d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170281"
---
# <a name="seteos-method"></a>SetEOS, méthode
Définit la position qui correspond à la fin du flux.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.SetEOS  
```  
  
## <a name="remarks"></a>Notes  
 **SetEOS** met à jour la valeur de la propriété [EOS](./eos-property.md) , en mettant la [position](./position-property-ado.md) actuelle à la fin du flux. Tous les octets ou caractères qui suivent la position actuelle sont tronqués.  
  
 Comme [Write](./write-method.md), [WRITETEXT](./writetext-method.md)et [CopyTo](./copyto-method-ado.md) ne tronquent pas de valeurs supplémentaires dans les objets de **flux** existants, vous pouvez tronquer ces octets ou caractères en définissant la nouvelle position de fin de flux avec **SetEOS**.  
  
> [!CAUTION]
>  Si vous affectez à **EOS** une position avant la fin réelle du flux, vous perdrez toutes les données après la nouvelle position **EOS** .  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)
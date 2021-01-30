---
description: Write, méthode
title: Write, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_Write
- _Stream::Write
helpviewer_keywords:
- Write method [ADO]
ms.assetid: 02982e6a-ac5f-4af2-b82e-ce12534b84b2
author: rothja
ms.author: jroth
ms.openlocfilehash: 0d37c1ec3650a4d10d98021bea3b27e996d43e45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172347"
---
# <a name="write-method"></a>Write, méthode
Écrit des données binaires dans un objet de [flux](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.Write Buffer  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Buffer*  
 **Variant** qui contient un tableau d’octets à écrire.  
  
## <a name="remarks"></a>Notes  
 Les octets spécifiés sont écrits dans l’objet de **flux** sans aucun espace intermédiaire entre chaque octet.  
  
 La [position](./position-property-ado.md) actuelle est définie sur l’octet qui suit les données écrites. La méthode **Write** ne tronque pas le reste des données dans un flux. Si vous souhaitez tronquer ces octets, appelez [SetEOS](./seteos-method.md).  
  
 Si vous écrivez après la position [EOS](./eos-property.md) actuelle, la [taille](./size-property-ado-stream.md) du **flux** sera augmentée pour contenir de nouveaux octets, et **EOS** passera au nouvel octet dernier dans le **flux**.  
  
> [!NOTE]
>  La méthode **Write** est utilisée avec les flux binaires (le [type](./type-property-ado-stream.md) est **adTypeBinary**). Pour les flux de texte (**type** **adTypeText**), utilisez [WRITETEXT](./writetext-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [WriteText, méthode](./writetext-method.md)
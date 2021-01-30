---
description: Read, méthode
title: Read, méthode | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream::raw_Read
- _Stream::Read
helpviewer_keywords:
- Read method [ADO]
ms.assetid: 838502de-80f1-4eeb-8838-dd3d9403e567
author: rothja
ms.author: jroth
ms.openlocfilehash: 0adf12bc63745f739aaf8b71a92c660c1c1ad2fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170492"
---
# <a name="read-method"></a>Read, méthode
Lit un nombre spécifié d’octets à partir d’un objet de [flux](./stream-object-ado.md) binaire.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Variant = Stream.Read ( NumBytes)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *NumBytes*  
 Facultatif. Valeur de **type long** qui spécifie le nombre d’octets à lire dans le fichier ou la valeur [StreamReadEnum](./streamreadenum.md) **adReadAll**, qui est la valeur par défaut.  
  
## <a name="return-value"></a>Valeur renvoyée  
 La méthode **Read** lit un nombre spécifié d’octets ou le flux entier à partir d’un objet de **flux** et retourne les données résultantes sous la forme d’un **Variant**.  
  
## <a name="remarks"></a>Notes  
 Si *NumBytes* est supérieur au nombre d’octets restants dans le **flux**, seuls les octets restants sont retournés. Les données lues ne sont pas complétées pour correspondre à la longueur spécifiée par *NumBytes*. S’il n’y a pas d’octets à lire, un variant avec une valeur null est retourné. La **lecture** ne peut pas être utilisée pour lire vers l’arrière.  
  
> [!NOTE]
>  *NumBytes* mesure toujours les octets. Pour les objets de **flux** de texte ([type](./type-property-ado-stream.md) **adTypeText**), utilisez [READTEXT](./readtext-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ReadText, méthode](./readtext-method.md)
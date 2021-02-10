---
description: WriteText, méthode
title: WriteText, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d310e2beaf69968721e2edc5c65c57cb88cf492
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056034"
---
# <a name="writetext-method"></a>WriteText, méthode
Écrit une chaîne de texte spécifiée dans un objet de [flux](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Données*  
 Valeur de **chaîne** qui contient le texte en caractères à écrire.  
  
 *Options*  
 facultatif. Valeur de [StreamWriteEnum](./streamwriteenum.md) qui spécifie si un caractère de séparation de ligne doit être écrit à la fin de la chaîne spécifiée.  
  
## <a name="remarks"></a>Notes  
 Les chaînes spécifiées sont écrites dans l’objet de **flux** sans aucun espace ou caractère intermédiaire entre chaque chaîne.  
  
 La [position](./position-property-ado.md) actuelle est définie sur le caractère qui suit les données écrites. La méthode **WRITETEXT** ne tronque pas le reste des données dans un flux. Si vous souhaitez tronquer ces caractères, appelez [SetEOS](./seteos-method.md).  
  
 Si vous écrivez après la position [EOS](./eos-property.md) actuelle, la [taille](./size-property-ado-stream.md) du **flux** est augmentée pour contenir de nouveaux caractères, et **EOS** se déplace vers le nouvel octet dernier dans le **flux**.  
  
> [!NOTE]
>  La méthode **WRITETEXT** est utilisée avec les flux de texte (le [type](./type-property-ado-stream.md) est **adTypeText**). Pour les flux binaires (**type** **adTypeBinary**), utilisez [Write](./write-method.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Write, méthode](./write-method.md)
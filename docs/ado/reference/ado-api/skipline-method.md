---
description: SkipLine, méthode
title: SkipLine, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SkipLine
- _Stream::SkipLine
helpviewer_keywords:
- Skipline method [ADO]
ms.assetid: 0abc00fe-ee09-4c8e-b1f2-48ee9c5f3329
author: rothja
ms.author: jroth
ms.openlocfilehash: 3c1b23ef249e2dd543772e1e414d347cc2377860
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051430"
---
# <a name="skipline-method"></a>SkipLine, méthode
Ignore une ligne entière lors de la lecture d’un [flux](./stream-object-ado.md)de texte.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.SkipLine  
```  
  
## <a name="remarks"></a>Notes  
 Tous les caractères jusqu’à et y compris le séparateur de ligne suivant sont ignorés. Par défaut, [LineSeparator](./lineseparator-property-ado.md) est **adCRLF**. Si vous tentez d’ignorer la dernière [EOS](./eos-property.md), la position actuelle restera à **EOS**.  
  
 La méthode **SkipLine** est utilisée avec les flux de texte (le [type](./type-property-ado-stream.md) est **adTypeText**).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)
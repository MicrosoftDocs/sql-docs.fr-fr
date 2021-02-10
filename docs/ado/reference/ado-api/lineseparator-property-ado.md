---
description: LineSeparator, propriété (ADO)
title: LineSeparator, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::LineSeparator
helpviewer_keywords:
- LineSeparator property [ADO]
ms.assetid: 0b20fbb8-6b83-48ec-b442-f96c8a4bafbb
author: rothja
ms.author: jroth
ms.openlocfilehash: fa50003834cf83017ec9eaf4581ed1cff7bd26bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044279"
---
# <a name="lineseparator-property-ado"></a>LineSeparator, propriété (ADO)
Indique le caractère binaire à utiliser comme séparateur de lignes dans les objets de [flux](./stream-object-ado.md) de texte.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur [LineSeparatorsEnum](./lineseparatorsenum.md) qui indique le caractère de séparation de ligne utilisé dans le **flux**. La valeur par défaut est **adCRLF**.  
  
## <a name="remarks"></a>Notes  
 **LineSeparator** est utilisé pour interpréter les lignes lors de la lecture du contenu d’un **flux** de texte. Les lignes peuvent être ignorées à l’aide de la méthode [SkipLine](./skipline-method.md) .  
  
 **LineSeparator** est utilisé uniquement avec des objets de **flux** de texte (le [type](./type-property-ado-stream.md) est **adTypeText**). Cette propriété est ignorée si le **type** est **adTypeBinary**.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Stream, objet (ADO)](./stream-object-ado.md)
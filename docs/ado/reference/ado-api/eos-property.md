---
description: EOS, propriété
title: EOS, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EOS
- Stream::EOS
helpviewer_keywords:
- EOS property
ms.assetid: 57e08c5f-f3ed-4ecd-8c66-50b83b1031d1
author: rothja
ms.author: jroth
ms.openlocfilehash: 45268bfba7669f3c202cec9bc652424981e1234e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025123"
---
# <a name="eos-property"></a>EOS, propriété
Indique si la position actuelle est à la fin du [flux](../../../ado/reference/ado-api/stream-object-ado.md).  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une valeur **booléenne** qui indique si la position actuelle est à la fin du flux. **EOS** retourne la **valeur true** s’il n’y a plus d’octets dans le flux ; elle retourne la **valeur false** s’il y a plus d’octets après la position actuelle.  
  
 Pour définir la fin de la position du flux, utilisez la méthode [SetEOS](../../../ado/reference/ado-api/seteos-method.md) . Pour déterminer la position actuelle, utilisez la propriété [position](../../../ado/reference/ado-api/position-property-ado.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés EOS et LineSeparator et SkipLine, exemple de méthode (VB)](../../../ado/reference/ado-api/eos-and-lineseparator-properties-and-skipline-method-example-vb.md)   
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)

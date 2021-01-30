---
description: Position, propriété (ADO)
title: Position, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Position
helpviewer_keywords:
- Position property [ADO]
ms.assetid: daa8319a-49aa-4c1c-9af6-0b01e9ab2f9d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9b2da711df951fc79db5034eeb447a71ade21fee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166853"
---
# <a name="position-property-ado"></a>Position, propriété (ADO)
Indique la position actuelle dans un objet de [flux](./stream-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** qui spécifie le décalage, en nombre d’octets, de la position actuelle à partir du début du flux. La valeur par défaut est 0, qui représente le premier octet dans le flux.  
  
## <a name="remarks"></a>Notes  
 La position actuelle peut être déplacée vers un point situé après la fin du flux. Si vous spécifiez la position actuelle au-delà de la fin du flux, la [taille](./size-property-ado-stream.md) de l’objet de **flux** sera augmentée en conséquence. Tout nouvel octet ajouté de cette façon sera null.  
  
> [!NOTE]
>  La **position** mesure toujours les octets. Pour les flux de texte utilisant des jeux de caractères multioctets, multipliez la position par la taille de caractère pour déterminer le nombre de caractères. Par exemple, pour un jeu de caractères de deux octets, le premier caractère se trouve à la position 0, le deuxième caractère à la position 2, le troisième caractère à la position 4, et ainsi de suite.  
  
> [!NOTE]
>  Les valeurs négatives ne peuvent pas être utilisées pour modifier la position actuelle dans un **flux**. Seuls les nombres positifs peuvent être utilisés pour la **position**.  
  
> [!NOTE]
>  Pour les objets de **flux** en lecture seule, ADO ne retourne pas d’erreur si **position** est définie sur une valeur supérieure à la **taille** du **flux**. Cela ne modifie pas la taille du **flux**, ni ne modifie le contenu du **flux** de quelque manière que ce soit. Toutefois, cette opération doit être évitée, car elle entraîne une valeur de **position** insignifiante.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Charset, propriété (ADO)](./charset-property-ado.md)
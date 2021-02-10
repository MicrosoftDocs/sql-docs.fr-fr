---
description: Ordinal, propriété (objet Position d’ADO MD)
title: Propriété ordinale (position ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Position::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: 6efe8b5d-a2d5-43a9-a5ea-f9244f8d4ec9
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ff25700d58b2f1101a7819d6c8faa914c2b0ae6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051010"
---
# <a name="ordinal-property-ado-md-position"></a>Ordinal, propriété (objet Position d’ADO MD)
Identifie de façon unique une [position](./position-object-ado-md.md) le long d’un axe.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un entier **long** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 La propriété **ordinale** d’un objet [position](./position-object-ado-md.md) correspond à l’index de la **position** dans la collection [positions](./positions-collection-ado-md.md) .  
  
 Une cellule peut rapidement être récupérée à l’aide de l' **ordinal** de la **position** le long de chaque axe avec la propriété [Item](./item-property-ado-md-cellset.md) de l’objet [Cellset](./cellset-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Position, objet (ADO MD)](./position-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CellSet, objet (ADO MD)](./cellset-object-ado-md.md)   
 [Item, propriété (ADO MD CellSet)](./item-property-ado-md-cellset.md)   
 [Ordinal, propriété (objet Cell d’ADO MD)](./ordinal-property-ado-md-cell.md)
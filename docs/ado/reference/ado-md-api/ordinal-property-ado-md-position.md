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
ms.openlocfilehash: a560b34e6ec55892a6f779c4808e5a7c69d99bca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169741"
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
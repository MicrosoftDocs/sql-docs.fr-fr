---
description: FormattedValue, propriété (ADO MD)
title: FormattedValue, propriété (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::FormattedValue
- FormattedValue
helpviewer_keywords:
- FormattedValue property [ADO MD]
ms.assetid: 5c06451e-06ec-4da6-9a87-2d043469248a
author: rothja
ms.author: jroth
ms.openlocfilehash: c2b1f8ebc7341a85cbeb887133336653979b5d9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169843"
---
# <a name="formattedvalue-property-ado-md"></a>FormattedValue, propriété (ADO MD)
Indique l’affichage mis en forme d’une valeur de [cellule](./cell-object-ado-md.md) .  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une **chaîne** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **FormattedValue** pour obtenir la valeur d’affichage mise en forme de la propriété [value](./value-property-ado-md.md) d’un objet [Cell](./cell-object-ado-md.md) . Par exemple, si la valeur d’une cellule était 1056,87 et que cette valeur représente un montant en dollars, **FormattedValue** serait $1 056,87.  
  
## <a name="applies-to"></a>S'applique à  
 [Cell, objet (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CellSet, exemple (VB)](./cellset-example-vb.md)   
 [Value, propriété (ADO MD)](./value-property-ado-md.md)
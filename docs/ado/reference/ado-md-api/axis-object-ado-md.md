---
description: Axis, objet (ADO MD)
title: Objet Axis (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Axis
helpviewer_keywords:
- Axis object [ADO MD]
ms.assetid: 5f498c9a-b1e7-4e6e-9ae6-71eadaf9aada
author: rothja
ms.author: jroth
ms.openlocfilehash: 591e3ea0a1fd9ebc3e6d8321da1af1638102749c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055774"
---
# <a name="axis-object-ado-md"></a>Axis, objet (ADO MD)
Représente un axe positionnel ou de filtre d’un CellSet, contenant les membres sélectionnés d’une ou plusieurs dimensions.  
  
## <a name="remarks"></a>Notes  
 Un objet **Axis** peut être contenu dans une collection [axes](./axes-collection-ado-md.md) ou être retourné par la propriété [FilterAxis](./filteraxis-property-ado-md.md) d’un ensemble de [cellules](./cellset-object-ado-md.md).  
  
 Avec les collections et les propriétés d’un objet **Axis** , vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez l' **axe** à l’aide de la propriété [Name](./name-property-ado-md.md) .  
  
-   Itérer au sein de chaque position le long d’un **axe** à l’aide de la collection [positions](./positions-collection-ado-md.md) .  
  
-   Obtenez le nombre de dimensions sur l' **axe** avec la propriété [DimensionCount](./dimensioncount-property-ado-md.md) .  
  
-   Obtenez des attributs spécifiques au fournisseur de l' **axe** à l’aide de la collection de [Propriétés](../ado-api/properties-collection-ado.md) ADO standard.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](./axis-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AXIS, exemple (VBScript)](./axis-example-vbscript.md)   
 [Axes, collection (ADO MD)](./axes-collection-ado-md.md)   
 [Collection positions (ADO MD)](./positions-collection-ado-md.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)
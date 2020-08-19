---
description: Axis, objet (ADO MD)
title: Objet Axis (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Axis
helpviewer_keywords:
- Axis object [ADO MD]
ms.assetid: 5f498c9a-b1e7-4e6e-9ae6-71eadaf9aada
author: rothja
ms.author: jroth
ms.openlocfilehash: 733a2962e381a7b8918fccfd076f44dbbc23da1e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441281"
---
# <a name="axis-object-ado-md"></a>Axis, objet (ADO MD)
Représente un axe positionnel ou de filtre d’un CellSet, contenant les membres sélectionnés d’une ou plusieurs dimensions.  
  
## <a name="remarks"></a>Notes  
 Un objet **Axis** peut être contenu dans une collection [axes](../../../ado/reference/ado-md-api/axes-collection-ado-md.md) ou être retourné par la propriété [FilterAxis](../../../ado/reference/ado-md-api/filteraxis-property-ado-md.md) d’un ensemble de [cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md).  
  
 Avec les collections et les propriétés d’un objet **Axis** , vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez l' **axe** à l’aide de la propriété [Name](../../../ado/reference/ado-md-api/name-property-ado-md.md) .  
  
-   Itérer au sein de chaque position le long d’un **axe** à l’aide de la collection [positions](../../../ado/reference/ado-md-api/positions-collection-ado-md.md) .  
  
-   Obtenez le nombre de dimensions sur l' **axe** avec la propriété [DimensionCount](../../../ado/reference/ado-md-api/dimensioncount-property-ado-md.md) .  
  
-   Obtenez des attributs spécifiques au fournisseur de l' **axe** à l’aide de la collection de [Propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) ADO standard.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/axis-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AXIS, exemple (VBScript)](../../../ado/reference/ado-md-api/axis-example-vbscript.md)   
 [Axes, collection (ADO MD)](../../../ado/reference/ado-md-api/axes-collection-ado-md.md)   
 [Collection positions (ADO MD)](../../../ado/reference/ado-md-api/positions-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)

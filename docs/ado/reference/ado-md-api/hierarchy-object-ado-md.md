---
description: Hierarchy, objet (ADO MD)
title: Objet Hierarchy (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Hierarchy
helpviewer_keywords:
- Hierarchy object [ADO MD]
ms.assetid: 034af340-ac79-494e-ba5e-2b57da1cb9de
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f84fe2157e9da38ad55a56334436a5d54128823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051150"
---
# <a name="hierarchy-object-ado-md"></a>Hierarchy, objet (ADO MD)
Représente une façon dont les membres d’une [dimension](./dimension-object-ado-md.md) peuvent être agrégés ou « cumulés ». Une dimension peut être agrégée avec une ou plusieurs hiérarchies.  
  
## <a name="remarks"></a>Notes  
 Avec les collections et les propriétés d’un objet **Hierarchy** , vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez la **hiérarchie** avec les propriétés [Name](./name-property-ado-md.md) et [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Retourne une chaîne significative qui décrit la **hiérarchie** avec la propriété [Description](./description-property-ado-md.md) .  
  
-   Retourne les objets de [niveau](./level-object-ado-md.md) qui composent la **hiérarchie** avec la collection de [niveaux](./levels-collection-ado-md.md) .  
  
-   Utilisez la collection de [Propriétés](../ado-api/properties-collection-ado.md) ADO standard pour obtenir des informations supplémentaires sur l’objet **Hierarchy** .  
  
 La collection **Properties** contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste de propriétés réelle peut varier en fonction de l’implémentation du fournisseur. Pour obtenir une liste plus complète des propriétés disponibles, consultez la documentation de votre fournisseur.  
  
|Nom|Description|  
|----------|-----------------|  
|AllMember|Membre au niveau le plus élevé de ROLLUP dans la hiérarchie.|  
|CatalogName|Nom du catalogue auquel appartient ce cube.|  
|CubeName|Nom du cube.|  
|DefaultMember|Nom unique du membre par défaut de cette hiérarchie.|  
|Description|Description explicite de la hiérarchie.|  
|DimensionType|Type de dimension auquel appartient cette hiérarchie.|  
|DimensionUniqueName|Nom non ambigu de la dimension.|  
|HierarchyCaption|Étiquette ou légende associée à la hiérarchie.|  
|HierarchyCardinality|Nombre de membres de la hiérarchie.|  
|HierarchyGUID|GUID de la hiérarchie.|  
|HierarchyName|Nom de la hiérarchie.|  
|HierarchyUniqueName|Nom non ambigu de la hiérarchie.|  
|SchemaName|Nom du schéma auquel appartient ce cube.|  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](./hierarchy-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, exemple (VBScript)](./cubedef-example-vbscript.md)   
 [Objet dimension (ADO MD)](./dimension-object-ado-md.md)   
 [Collections Hierarchies (ADO MD)](./hierarchies-collection-ado-md.md)   
 [Collection Levels (ADO MD)](./levels-collection-ado-md.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)
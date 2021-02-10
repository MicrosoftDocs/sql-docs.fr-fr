---
description: Level, objet (ADO MD)
title: Objet Level (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Level
helpviewer_keywords:
- Level object [ADO MD]
ms.assetid: 37815869-ed30-45fd-9aea-0a986c1b305c
author: rothja
ms.author: jroth
ms.openlocfilehash: 501a7e3eda723579a63b39cf7fd09c9a39216604
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051140"
---
# <a name="level-object-ado-md"></a>Level, objet (ADO MD)
Contient un ensemble de membres, chacun ayant le même rang dans une hiérarchie.  
  
## <a name="remarks"></a>Notes  
 Avec les collections et les propriétés d’un objet **Level** , vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez le **niveau** avec les propriétés [Name](./name-property-ado-md.md) et [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Retourne une chaîne à utiliser lors de l’affichage du **niveau** avec la propriété [Caption](./caption-property-ado-md.md) .  
  
-   Retourne une chaîne significative qui décrit le **niveau** avec la propriété [Description](./description-property-ado-md.md) .  
  
-   Retourne les objets [membres](./member-object-ado-md.md) qui composent le **niveau** avec la collection de [membres](./members-collection-ado-md.md) .  
  
-   Retourne le nombre de niveaux à partir de la racine du **niveau** avec la propriété [Depth](./depth-property-ado-md.md) .  
  
-   Utilisez la collection de [Propriétés](../ado-api/properties-collection-ado.md) ADO standard pour obtenir des informations supplémentaires sur l’objet **Level** .  
  
 La collection **Properties** contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste de propriétés réelle peut varier en fonction de l’implémentation du fournisseur. Pour obtenir une liste plus complète des propriétés disponibles, consultez la documentation de votre fournisseur.  
  
|Nom|Description|  
|----------|-----------------|  
|CatalogName|Nom du catalogue auquel appartient ce cube.|  
|CubeName|Nom du cube.|  
|Description|Description explicite du niveau.|  
|DimensionUniqueName|Nom non ambigu de la [dimension](./dimension-object-ado-md.md).|  
|HierarchyUniqueName|Nom non ambigu de la hiérarchie.|  
|LevelCaption|Étiquette ou légende associée au niveau.|  
|LevelCardinality|Nombre de membres du niveau.|  
|LevelGUID|GUID du niveau.|  
|LevelName|Nom du niveau.|  
|LevelNumber|Distance entre le niveau et la racine de la hiérarchie.|  
|LevelType|Type de niveau.|  
|LevelUniqueName|Nom non ambigu du niveau.|  
|SchemaName|Nom du schéma auquel appartient ce cube.|  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](./level-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, exemple (VBScript)](./cubedef-example-vbscript.md)   
 [Objet Hierarchy (ADO MD)](./hierarchy-object-ado-md.md)   
 [Collection Levels (ADO MD)](./levels-collection-ado-md.md)   
 [Collection Members (ADO MD)](./members-collection-ado-md.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)
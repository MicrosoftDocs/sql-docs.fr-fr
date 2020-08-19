---
description: Member, objet (ADO MD)
title: Member, objet (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Member
helpviewer_keywords:
- Member object [ADO MD], members
ms.assetid: 3dedf755-0741-4c3f-8b4e-bff8ff8809c8
author: rothja
ms.author: jroth
ms.openlocfilehash: 6e0797a4d273c51b950e3973d1864480755a20d1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88440921"
---
# <a name="member-object-ado-md"></a>Member, objet (ADO MD)
Représente un membre d’un niveau dans un cube, les enfants d’un membre d’un niveau ou un membre d’une position le long d’un axe d’un ensemble de cellules.  
  
## <a name="remarks"></a>Notes  
 Les propriétés d’un **membre** varient en fonction du contexte dans lequel il est utilisé. Un **membre** d’un [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) dans un [CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md) a une propriété [Children](../../../ado/reference/ado-md-api/children-property-ado-md.md) qui retourne les **membres** au niveau inférieur suivant dans la hiérarchie à partir du **membre**actuel. Pour un **membre** d’une [position](../../../ado/reference/ado-md-api/position-object-ado-md.md), la collection **Children** est toujours vide. En outre, la propriété [type](../../../ado/reference/ado-md-api/type-property-ado-md.md) s’applique uniquement aux **membres** d’un **niveau**.  
  
 Un **membre** de la **position** possède deux propriétés qui sont utiles lors de l’affichage de l' [Cellset](../../../ado/reference/ado-md-api/cellset-object-ado-md.md): [DrilledDown](../../../ado/reference/ado-md-api/drilleddown-property-ado-md.md) et [ParentSameAsPrev](../../../ado/reference/ado-md-api/parentsameasprev-property-ado-md.md). Une erreur se produit si ces propriétés sont accessibles sur un **membre** d’un **niveau**.  
  
 Avec les collections et les propriétés d’un objet **membre** d’un **niveau**, vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez le **membre** avec les propriétés [Name](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) .  
  
-   Retourne une chaîne à utiliser lors de l’affichage du **membre** avec la propriété [Caption](../../../ado/reference/ado-md-api/caption-property-ado-md.md) .  
  
-   Retourne une chaîne significative qui décrit une mesure ou un **membre** de formule avec la propriété [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) .  
  
-   Déterminez la nature du **membre** avec la propriété [type](../../../ado/reference/ado-md-api/type-property-ado-md.md) .  
  
-   Obtenez des informations sur le **niveau** du **membre** avec les propriétés [LevelDepth](../../../ado/reference/ado-md-api/leveldepth-property-ado-md.md) et [LevelName](../../../ado/reference/ado-md-api/levelname-property-ado-md.md) .  
  
-   Obtenir les **membres** associés dans une [hiérarchie](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md) avec les propriétés [parent](../../../ado/reference/ado-md-api/parent-property-ado-md.md) et [Children](../../../ado/reference/ado-md-api/children-property-ado-md.md) .  
  
-   Comptez les enfants d’un **membre** avec la propriété [ChildCount](../../../ado/reference/ado-md-api/childcount-property-ado-md.md) .  
  
-   Utilisez la collection de [Propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) ADO standard pour obtenir des informations supplémentaires sur l’objet **Level** .  
  
 Avec les collections et les propriétés d’un **membre** d’une **position** le long d’un [axe](../../../ado/reference/ado-md-api/axis-object-ado-md.md), vous pouvez effectuer les opérations suivantes :  
  
-   Identifiez le **membre** avec les propriétés [Name](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) .  
  
-   Retourne une chaîne à utiliser lors de l’affichage du **membre** avec la propriété [Caption](../../../ado/reference/ado-md-api/caption-property-ado-md.md) .  
  
-   Retourne une chaîne significative qui décrit une mesure ou un **membre** de formule avec la propriété [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) .  
  
-   Obtenez des informations sur le **niveau** du **membre** avec les propriétés [LevelDepth](../../../ado/reference/ado-md-api/leveldepth-property-ado-md.md) et [LevelName](../../../ado/reference/ado-md-api/levelname-property-ado-md.md) .  
  
-   Comptez les enfants d’un **membre** avec la propriété [ChildCount](../../../ado/reference/ado-md-api/childcount-property-ado-md.md) .  
  
-   Utilisez la propriété [DrilledDown](../../../ado/reference/ado-md-api/drilleddown-property-ado-md.md) pour déterminer s’il existe au moins un enfant sur l' **axe** qui suit immédiatement ce **membre**.  
  
-   Utilisez la propriété [ParentSameAsPrev](../../../ado/reference/ado-md-api/parentsameasprev-property-ado-md.md) pour déterminer si le parent de ce **membre** est le même que le parent du **membre**qui le précède immédiatement.  
  
-   Utilisez la collection de [Propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) ADO standard pour obtenir des informations supplémentaires sur l’objet **Level** .  
  
 La collection **Properties** contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste de propriétés réelle peut varier en fonction de l’implémentation du fournisseur. Pour obtenir une liste plus complète des propriétés disponibles, consultez la documentation de votre fournisseur.  
  
|Nom|Description|  
|----------|-----------------|  
|CatalogName|Nom du catalogue auquel appartient ce cube.|  
|ChildrenCardinality|Nombre d'enfants de ce membre.|  
|CubeName|Nom du cube.|  
|Description|Description explicite du membre.|  
|DimensionUniqueName|Nom non ambigu de la [dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md).|  
|HierarchyUniqueName|Nom non ambigu de la hiérarchie.|  
|LevelNumber|Distance entre le niveau et la racine de la hiérarchie.|  
|LevelUniqueName|Nom non ambigu du niveau.|  
|MemberCaption|Étiquette ou légende associée au membre.|  
|MemberGUID|GUID du membre.|  
|MemberName|Nom du membre.|  
|MemberOrdinal|Numéro ordinal du membre.|  
|MemberType|Type du membre.|  
|MemberUniqueName|Nom non ambigu du membre.|  
|ParentCount|Nombre de parents de ce membre.|  
|ParentLevel|Numéro de niveau du parent du membre.|  
|ParentUniqueName|Nom non ambigu du parent du membre.|  
|SchemaName|Nom du schéma auquel appartient ce cube.|  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/member-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de catalogue (VB)](../../../ado/reference/ado-md-api/catalog-example-vb.md)   
 [Collection Members (ADO MD)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)

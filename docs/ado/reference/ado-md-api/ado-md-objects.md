---
description: Objets ADO MD
title: Objets ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO MD, objects
- objects [ADO MD]
ms.assetid: 2a32e873-3282-4520-a7ed-89493f1da80e
author: rothja
ms.author: jroth
ms.openlocfilehash: ae9723fb593a6f2599a3fe13cbfc359fa280febe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172306"
---
# <a name="ado-md-objects"></a>Objets ADO MD

|Object|Description|  
|-|-|  
|[Axe](./axis-object-ado-md.md)|Représente un axe positionnel ou de filtre d’un CellSet, contenant les membres sélectionnés d’une ou plusieurs dimensions.|  
|[Catalogue](./catalog-object-ado-md.md)|Contient des informations sur les schémas multidimensionnels (c’est-à-dire les cubes et les dimensions sous-jacentes, les hiérarchies, les niveaux et les membres) spécifiques à un fournisseur de données multidimensionnelles (MDP).|  
|[Cellule](./cell-object-ado-md.md)|Représente les données à l’intersection des coordonnées d’axe, contenues dans un CellSet.|  
|[CellSet](./cellset-object-ado-md.md)|Représente les résultats d’une requête multidimensionnelle. Il s’agit d’une collection de cellules sélectionnées à partir de cubes ou d’autres cellules.|  
|[CubeDef](./cubedef-object-ado-md.md)|Représente un cube à partir d’un schéma multidimensionnel, contenant un ensemble de dimensions associées.|  
|[Dimension](./dimension-object-ado-md.md)|Représente l’une des dimensions d’un cube multidimensionnel, contenant une ou plusieurs hiérarchies de membres.|  
|[Hierarchy](./hierarchy-object-ado-md.md)|Représente une façon dont les membres d’une dimension peuvent être agrégés ou « cumulés ». Une dimension peut être agrégée avec une ou plusieurs hiérarchies.|  
|[Niveau](./level-object-ado-md.md)|Contient un ensemble de membres, chacun ayant le même rang dans une hiérarchie.|  
|[Membre](./member-object-ado-md.md)|Représente un membre d’un niveau dans un cube, les enfants d’un membre d’un niveau ou un membre d’une position le long d’un axe d’un ensemble de cellules.|  
|[Position](./position-object-ado-md.md)|Représente un ensemble d’un ou plusieurs membres de dimensions différentes qui définissent un point le long d’un axe.|  
  
 En outre, l’objet **catalogue** est connecté à un objet de **connexion** ADO, qui est inclus dans la bibliothèque ADO standard :  
  
|Object|Description|  
|------------|-----------------|  
|[Connection](../ado-api/connection-object-ado.md)|Représente une connexion ouverte à une source de données.|  
  
 Les relations entre ces objets sont illustrées dans le [modèle objet ADO MD](./ado-md-object-model.md).  
  
 De nombreux objets ADO MD peuvent être contenus dans une collection correspondante. Par exemple, un objet [CubeDef](./cubedef-object-ado-md.md) peut être contenu dans une collection [CubeDefs](./cubedefs-collection-ado-md.md) d’un **catalogue**. Pour plus d’informations, consultez [ADO MD collections](./ado-md-collections.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO MD](./ado-md-object-model.md)   
 [Exemples de code ADO MD](./ado-md-code-examples.md)   
 [Collections de ADO MD](./ado-md-collections.md)   
 [ADO MD les constantes énumérées](./ado-md-enumerated-constants.md)   
 [Méthodes ADO MD](./ado-md-methods.md)   
 [Modèle objet ADO MD](./ado-md-object-model.md)   
 [Propriétés ADO MD](./ado-md-properties.md)
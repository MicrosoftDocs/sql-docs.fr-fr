---
description: Name (MDX)
title: Nom (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1f7598d8ee25b3ffa50d8b4ab7638640d0c5e4ac
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88483762"
---
# <a name="name-mdx"></a>Name (MDX)


  Retourne le nom d'une dimension, d'une hiérarchie, d'un niveau ou d'un membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Dimension expression syntax  
Dimension_Expression.Name  
  
Hierarchy expression syntax  
Hierarchy_Expression.Name  
  
Level_expression syntax  
Level_Expression.Name  
  
Member expression syntax  
Member_Expression.Name  
```  
  
## <a name="arguments"></a>Arguments  
 *Dimension_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une dimension.  
  
 *Hierarchy_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *Member_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
## <a name="remarks"></a>Notes  
 La fonction **Name** retourne le nom de l’objet, et non le nom unique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="dimension-hierarchy-and-level-expression-example"></a>Exemple d'expression de dimension, de hiérarchie et de niveau  
 L'exemple ci-dessous retourne le nom de la dimension Date ainsi que les noms de hiérarchie et de niveau du membre July 2001.  
  
```  
WITH MEMBER Measures.DimensionName AS [Date].Name  
MEMBER Measures.HierarchyName AS [Date].[Calendar].[July 2001].Hierarchy.Name  
MEMBER Measures.LevelName as [Date].[Calendar].[July 2001].Level.Name  
  
SELECT {Measures.DimensionName, Measures.HierarchyName, Measures.LevelName} ON 0  
from [Adventure Works]  
```  
  
### <a name="member-expression-example"></a>Exemple d'expression de membre  
 L'exemple ci-dessous retourne le nom du membre, ainsi que sa valeur, sa clé et sa légende.  
  
```  
WITH MEMBER MemberName AS [Date].[Calendar].[July 1, 2001].Name  
MEMBER Measures.ValueColumn as [Date].[Calendar].[July 1, 2001].MemberValue  
MEMBER Measures.KeyColumn as [Date].[Calendar].[July 1, 2001].Member_Key  
MEMBER Measures.NameColumn as [Date].[Calendar].[July 1, 2001].Member_Name  
  
SELECT {Measures.MemberName, Measures.ValueColumn, Measures.KeyColumn, Measures.NameColumn} ON 0  
from [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

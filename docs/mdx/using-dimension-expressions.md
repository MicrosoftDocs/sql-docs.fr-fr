---
description: Utilisation d'expressions de dimension
title: Utilisation d’expressions de dimension | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: e8fb367f0e4ab1cc99a1a84ed51e3cca3cab75ee
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88341115"
---
# <a name="using-dimension-expressions"></a>Utilisation d'expressions de dimension


  Vous utilisez généralement des expressions de dimension et de hiérarchie lorsque vous passez des paramètres à des fonctions dans MDX (Multidimensional Expressions) pour retourner des membres, des jeux ou des tuples à partir d'une hiérarchie.  
  
 Les expressions de dimension peuvent être uniquement des expressions simples car elles sont des identificateurs d'objet. Pour une explication des expressions simples et complexes, consultez [expressions &#40;&#41;MDX ](../mdx/expressions-mdx.md) .  
  
## <a name="dimension-expressions"></a>Expressions de dimension  
 Une expression de dimension contient un identificateur de dimension ou une fonction de dimension.  
  
 Les expressions de dimension sont rarement utilisées seules. À la place, il est souhaitable généralement de spécifier une hiérarchie sur une dimension. La seule exception est lorsque vous travaillez avec la dimension de mesures qui n'ont pas de hiérarchies.  
  
 L'exemple ci-dessous présente un membre calculé qui utilise conjointement l'expression [Measures] avec les fonctions Members et Count() pour retourner le nombre de membres dans la dimension Measures :  
  
 `WITH MEMBER [Measures].[MeasureCount] AS`  
  
 `COUNT([Measures].MEMBERS)`  
  
 `SELECT [Measures].[MeasureCount] ON 0`  
  
 `FROM [Adventure Works]`  
  
 Un identificateur de dimension apparaît comme *Dimension_Name* dans la notation BNF utilisée pour décrire des instructions MDX.  
  
## <a name="hierarchy-expressions"></a>Expressions de hiérarchie  
 De même, une expression de hiérarchie contient un identificateur de hiérarchie ou une fonction de hiérarchie. L'exemple ci-dessous illustre l'utilisation de l'expression de hiérarchie [Date].[Calendar] conjointement avec les fonctions .Levels et .Count pour retourner le nombre de niveaux dans la hiérarchie Calendar de la dimension Date :  
  
 `WITH MEMBER [Measures].[CalendarLevelCount] AS`  
  
 `[Date].[Calendar].Levels.Count`  
  
 `SELECT [Measures].[CalendarLevelCount] ON 0`  
  
 `FROM [Adventure Works]`  
  
 Le scénario le plus courant où les expressions de hiérarchie sont utilisées a lieu conjointement avec la fonction .Members, pour retourner tous les membres sur une hiérarchie. L'exemple suivant retourne toutes les membres de [Date].[Calendar] sur l'axe des lignes :  
  
 `SELECT [Measures].[Internet Sales Amount] ON 0,`  
  
 `[Date].[Calendar].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
 Un identificateur de hiérarchie apparaît comme *Dimension_Name. hierarchy_name* dans la notation BNF utilisée pour décrire des instructions MDX.  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;MDX&#41;](../mdx/expressions-mdx.md)  
  
  

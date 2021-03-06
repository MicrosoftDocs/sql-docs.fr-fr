---
description: This (MDX)
title: This (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: b55a416a14d0b837d134e7f9d8520d77cc13e375
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192329"
---
# <a name="this-mdx"></a>This (MDX)


  Retourne le sous-cube actuel pour l'utiliser avec des assignations dans le script de calcul MDX (Multidimensional Expressions).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
This   
```  
  
## <a name="remarks"></a>Notes  
 **Cette** fonction peut être utilisée à la place d’une expression de sous-cube pour fournir le sous-cube actuel au sein de l’étendue actuelle au sein du script de calcul MDX. **Cette** fonction doit être utilisée sur le côté gauche d’une assignation.  
  
## <a name="examples"></a>Exemples  
 Le fragment de script MDX suivant illustre l'utilisation du mot clé THIS avec les instructions SCOPE pour effectuer des affectations aux sous-cubes :  
  
 `Scope`  
  
 `(`  
  
 `[Date].[Fiscal Year].&[2005],`  
  
 `[Date].[Fiscal].[Fiscal Quarter].Members,`  
  
 `[Measures].[Sales Amount Quota]`  
  
 `) ;`  
  
 `This = ParallelPeriod`  
  
 `(`  
  
 `[Date].[Fiscal].[Fiscal Year], 1,`  
  
 `[Date].[Fiscal].CurrentMember`  
  
 `) * 1.35 ;`  
  
 `/*-- Allocate equally to months in FY 2002 -----------------------------*/`  
  
 `Scope`  
  
 `(`  
  
 `[Date].[Fiscal Year].&[2002],`  
  
 `[Date].[Fiscal].[Month].Members`  
  
 `) ;`  
  
 `This = [Date].[Fiscal].CurrentMember.Parent / 3 ;`  
  
 `End Scope ;`  
  
 `End Scope;`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;&#41;MDX ](../mdx/mdx-function-reference-mdx.md)   
 [Calculs](/analysis-services/multidimensional-models-olap-logical-cube-objects/calculations)  
  

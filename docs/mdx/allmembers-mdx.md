---
description: AllMembers (MDX)
title: AllMembers (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: ba70d4ad8b301cbd8af2fd76ce058f2dab2e4a4c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461671"
---
# <a name="allmembers-mdx"></a>AllMembers (MDX)


  Évalue une hiérarchie ou une expression de niveau et retourne un jeu qui contient tous les membres de la hiérarchie ou du niveau spécifié, y compris tous les membres calculés figurant dans cette hiérarchie ou ce niveau.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Hierarchy syntax  
Hierarchy_Expression.AllMembers  
  
Level syntax  
Level_Expression.AllMembers  
```  
  
## <a name="arguments"></a>Arguments  
 *Hierarchy_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
## <a name="remarks"></a>Notes  
 La fonction **AllMembers** retourne un jeu qui contient tous les membres, y compris les membres calculés, dans la hiérarchie ou le niveau spécifié. La fonction **AllMembers** retourne les membres calculés même si la hiérarchie ou le niveau spécifié ne contient aucun membre visible.  
  
> [!IMPORTANT]  
>  Lorsqu'une dimension contient uniquement une hiérarchie visible unique, cette hiérarchie peut être désignée soit par le nom de dimension, soit par le nom de la hiérarchie, puisque le nom de dimension dans ce cas est résolu à son unique hiérarchie visible. Par exemple, `Measures.AllMembers` est une expression MDX valide parce qu'elle est résolue à la seule hiérarchie de la dimension de mesures.  
  
> [!NOTE]  
>  La fonction **AllMembers** est sémantiquement similaire à la fonction [AddCalculatedMembers (MDX)](../mdx/addcalculatedmembers-mdx.md) .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne tous les membres de la `Date].[Calendar Year]` hiérarchie d’attribut [sur l’axe des colonnes, y compris les membres calculés et l’ensemble de tous les enfants de la `[Product].[Model Name]` hiérarchie d’attribut sur l’axe des lignes du cube **Adventure Works** .  
  
```  
SELECT  
   [Date].[Calendar Year].AllMembers ON COLUMNS,  
   [Product].[Model Name].Children ON ROWS  
FROM  
   [Adventure Works]  
```  
  
 L’exemple suivant retourne tous les membres de la dimension **Measures** sur l’axe des colonnes, y compris tous les membres calculés et l’ensemble de tous les enfants de la `[Product].[Model Name]` hiérarchie d’attribut sur l’axe des lignes du cube **Adventure Works** .  
  
```  
SELECT  
    Measures.AllMembers ON COLUMNS,  
    [Product].[Model Name].Children ON ROWS  
FROM  
    [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [AddCalculatedMembers&#41;MDX &#40;](../mdx/addcalculatedmembers-mdx.md)   
 [&#41;MDX &#40;](../mdx/children-mdx.md)   
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

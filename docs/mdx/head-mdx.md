---
description: Head (MDX)
title: Head (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 51a832bf38d3834c44f9b31f5bbfd27833d6d691
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88429921"
---
# <a name="head-mdx"></a>Head (MDX)


  Retourne le premier nombre spécifié d'éléments dans un jeu, en conservant les doublons.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Head(Set_Expression [ ,Count ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Count*  
 Expression numérique valide qui précise le nombre de tuples à retourner.  
  
## <a name="remarks"></a>Notes  
 La fonction **Head** retourne le nombre spécifié de tuples à partir du début de l’ensemble spécifié. L'ordre des éléments est conservé. La valeur par défaut de Count est 1. Si le nombre de tuples spécifié est inférieur à 1, la fonction **Head** retourne un ensemble vide. Si le nombre de tuples spécifié dépasse le nombre de tuples dans le jeu, la fonction retourne le jeu d'origine.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne les cinq premières sous-catégories de vente de produits, quelle que soit la hiérarchie et conformément à la mesure Reseller Gross Profit (marge brute du revendeur). La fonction **Head** est utilisée pour retourner uniquement les 5 premiers jeux dans le résultat une fois que le résultat est ordonné à l’aide de la fonction **Order** .  
  
```  
SELECT   
[Measures].[Reseller Gross Profit] ON 0,  
Head  
   (Order   
      ([Product].[Product Categories].[SubCategory].members  
         ,[Measures].[Reseller Gross Profit]  
         ,BDESC  
      )  
   ,5  
   ) ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [&#41;MDX &#40;](../mdx/tail-mdx.md)   
 [Élément &#40;Tuple&#41; &#40;MDX&#41;](../mdx/item-tuple-mdx.md)   
 [Élément &#40;&#41; &#40;MDX&#41;](../mdx/item-member-mdx.md)   
 [Classement &#40;&#41;MDX ](../mdx/rank-mdx.md)   
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

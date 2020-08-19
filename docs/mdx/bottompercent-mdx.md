---
description: BottomPercent (MDX)
title: BottomPercent (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1b3ce341526270335564e9f67ccb89612854e098
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494976"
---
# <a name="bottompercent-mdx"></a>BottomPercent (MDX)


  Trie un jeu en ordre croissant, et retourne un jeu de tuples avec les valeurs les plus basses dont le total cumulé est supérieur ou égal à un pourcentage spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BottomPercent(Set_Expression, Percentage, Numeric_Expression)   
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Percentage*  
 Expression numérique valide qui précise le pourcentage de tuples à retourner.  
  
 *Numeric_Expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
## <a name="remarks"></a>Notes  
 La fonction **BottomPercent** calcule la somme de l’expression numérique spécifiée évaluée sur un jeu spécifié, en triant le jeu dans l’ordre croissant. Elle retourne ensuite les éléments dotés des valeurs les plus faibles et dont le pourcentage cumulé du total de la valeur additionnée correspond au moins au pourcentage indiqué. Enfin, elle retourne le plus petit sous-ensemble d'un jeu dont le total cumulé est égal au moins au pourcentage précisé. Les éléments retournés sont triés du plus grand au plus petit.  
  
> [!IMPORTANT]  
>  La fonction **BottomPercent** , comme la fonction de [pourcentage](../mdx/toppercent-mdx.md) , interrompt toujours la hiérarchie. Pour plus d'informations, consultez la fonction Order.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne pour la catégorie Bikes (bicyclettes) le plus petit jeu de membres au niveau City (ville) de la hiérarchie Geography (zone géographique) dans la dimension Geography de l'année fiscale 2003 dont le total cumulé et obtenu à l'aide de la mesure Reseller Sales Amount (volume de vente du revendeur) correspond au moins à 15 % du total cumulé (en commençant par les membres de jeu en question avec le nombre le plus faible de ventes).  
  
```  
SELECT  
[Product].[Product Categories].Bikes ON 0,  
BottomPercent  
   ({[Geography].[Geography].[City].Members}  
   , 15  
   , ([Measures].[Reseller Sales Amount],[Product].[Product Categories].Bikes)  
   ) ON 1  
FROM [Adventure Works]  
WHERE ([Measures].[Reseller Sales Amount],[Date].[Fiscal].[Fiscal Year].[FY 2003])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

---
description: Sum (MDX)
title: Sum (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: f65525836157f5ac106cfa7ba0d5458689583def
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449621"
---
# <a name="sum-mdx"></a>Sum (MDX)


  Retourne la somme d'une expression numérique évaluée sur un jeu spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Sum( Set_Expression [ , Numeric_Expression ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression d'ensemble MDX (Multidimensional Expressions) valide.  
  
 *Numeric_Expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
## <a name="remarks"></a>Notes  
 Si une expression numérique est spécifiée, l'expression numérique spécifiée est évaluée sur le jeu, puis totalisée. Si aucune expression numérique n'est précisée, le jeu spécifié est évalué dans le contexte actuel des membres du jeu avant d'être totalisé. Si la fonction SUM est appliquée à une expression non numérique, le résultat n'est pas défini.  
  
> [!NOTE]  
>  Analysis Services ignore les valeurs NULL lors du calcul de la somme d'un jeu de nombres.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne la somme de la mesure Reseller Sales Amount (volume de vente du revendeur) de tous les membres de la hiérarchie d'attribut Product.Category pour les années civiles 2001 et 2002.  
  
```  
WITH MEMBER Measures.x AS SUM  
   ( { [Date].[Calendar Year].&[2001]  
         , [Date].[Calendar Year].&[2002] }  
      , [Measures].[Reseller Sales Amount]  
    )  
SELECT Measures.x ON 0  
,[Product].[Category].Members ON 1  
FROM [Adventure Works]  
```  
  
 L'exemple ci-dessous retourne la somme des coûts de fret concernant les ventes Internet du mois de juillet 2002 jusqu'à la date du 20 juillet.  
  
```  
WITH MEMBER Measures.x AS SUM   
   (  
      MTD([Date].[Calendar].[Date].[July 20, 2002])  
     , [Measures].[Internet Freight Cost]  
     )  
SELECT Measures.x ON 0  
FROM [Adventure Works]  
```  
  
 L’exemple suivant utilise le mot clé WITH MEMBER et la fonction **Sum** pour définir un membre calculé dans la dimension Measures qui contient la somme de la mesure Reseller Sales Amount pour le Canada et États-Unis membres de la hiérarchie d’attribut Country dans la dimension Geography.  
  
```  
WITH MEMBER Measures.NorthAmerica AS SUM   
      (  
         {[Geography].[Country].&[Canada]  
            , [Geography].[Country].&[United States]}  
       ,[Measures].[Reseller Sales Amount]  
      )  
SELECT {[Measures].[NorthAmerica]} ON 0,  
[Product].[Category].members ON 1  
FROM [Adventure Works]  
```  
  
 Souvent, la fonction **Sum** est utilisée avec la fonction **CurrentMember** ou des fonctions telles que **AAD** qui renvoient un jeu qui varie en fonction du CurrentMember d’une hiérarchie. Par exemple, la requête suivante retourne la somme de la mesure de Montant des ventes sur Internet pour toutes les dates du début de l'année civile à la date affichée sur l'axe des lignes :  
  
 `WITH MEMBER MEASURES.YTDSUM AS`  
  
 `SUM(YTD(), [Measures].[Internet Sales Amount])`  
  
 `SELECT {[Measures].[Internet Sales Amount], MEASURES.YTDSUM} ON 0,`  
  
 `[Date].[Calendar].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

---
description: VisualTotals (MDX)
title: VisualTotals (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1c1e85f4384c322938f5e7937c32bcd116acdda2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88500422"
---
# <a name="visualtotals-mdx"></a>VisualTotals (MDX)


  Retourne un jeu généré en totalisant de manière dynamique les membres enfants d'un jeu spécifié, et avec l'aide éventuelle d'un modèle pour le nom du membre parent dans le jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
VisualTotals(Set_Expression[,Pattern])  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Modèle*  
 Expression de chaîne valide du membre parent du jeu contenant un astérisque (*) en tant que caractère de substitution utilisé pour le nom du parent.  
  
## <a name="remarks"></a>Notes  
 L'expression de jeu définie peut spécifier un jeu qui contient des membres à n'importe quel niveau au sein d'une seule dimension. Il s'agit généralement de membres pour lesquels une relation ancêtre-descendant existe. La fonction **VisualTotals** totalise les valeurs des membres enfants dans le jeu spécifié et ignore les membres enfants qui ne sont pas dans le jeu dans le calcul des totaux de résultat. Les totaux sont effectués de manière visible pour les jeux classés dans l'ordre hiérarchique. Si l'ordre des membres dans les jeux ne respecte pas la hiérarchie, les résultats obtenus ne sont pas des valeurs visibles. Par exemple, la fonction VisualTotals (USA, WA, CA, Seattle) ne retourne pas WA sous la forme Seattle, mais plutôt les valeurs de WA, CA et Seattle, puis totalise ces valeurs en tant que valeur visible pour la valeur USA, comptabilisant ainsi les ventes pour Seattle deux fois.  
  
> [!NOTE]  
>  L’application de la fonction **VisualTotals** à des membres de dimension qui ne sont pas associés à une mesure ou qui se trouvent sous la granularité du groupe de mesures entraîne le remplacement des valeurs par la valeur null.  
  
 *Pattern*, qui est facultatif, spécifie le format de l’étiquette des totaux. Le *modèle* requiert un astérisque (*) comme caractère de substitution pour le membre parent et le reste du texte dans la chaîne apparaît dans le résultat concaténé avec le nom du parent. Pour afficher un astérisque littéral, utilisez deux astérisques ( \* \* ).  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne la valeur visible du troisième trimestre de l'année civile 2001 sur la base de l'unique descendant spécifié, soit le mois de juillet.  
  
```  
SELECT VisualTotals  
   ({[Date].[Calendar].[Calendar Quarter].&[2001]&[3]  
      ,[Date].[Calendar].[Month].&[2001]&[7]}) ON 0  
FROM [Adventure Works]  
```  
  
 L'exemple suivant retourne le membre [All] de la hiérarchie d'attribut Category dans la dimension Product avec deux de ses quatre enfants. Le total retourné pour le membre [All] de la mesure Internet Sales Amount (volume de vente Internet) correspond au total des membres Accessories (accessoires) et Clothing (vêtements) uniquement. L'argument Pattern est également utilisé pour préciser l'étiquette de la colonne [All Products].  
  
```  
SELECT  
   VisualTotals  
   ({[Product].[Category].[All Products]  
      ,[Product].[Category].[Accessories]  
      ,[Product].[Category].[Clothing]}  
      , '* - Visual Total'  
   ) ON Columns  
, [Measures].[Internet Sales Amount] ON Rows  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

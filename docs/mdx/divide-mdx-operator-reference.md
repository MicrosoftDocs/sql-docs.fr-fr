---
description: Référence de l’opérateur Divide-MDX
title: Diviser (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: a0d6947e1e0b4dc45d56980b734c2de98d8ad24c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484022"
---
# <a name="divide---mdx-operator-reference"></a>Référence de l’opérateur Divide-MDX


  Exécute une opération arithmétique qui divise un nombre par un autre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Dividend / Divisor  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Détaché*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une valeur numérique.  
  
 *Division*  
 Expression MDX valide qui retourne une valeur numérique.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur dont le type de données du paramètre possède la priorité la plus élevée.  
  
## <a name="remarks"></a>Notes  
 La valeur réelle retournée par l’opérateur **/(Division)** représente le quotient de la première expression divisée par la deuxième expression.  
  
 Les deux expressions doivent être de même type de données, ou l'une des expressions doit pouvoir être implicitement convertie dans le type de données de l'autre expression. Si *Divisor* correspond à une valeur null, l’opérateur génère une erreur. Si *diviseur* et *Dividend* correspondent à une valeur null, l’opérateur retourne une valeur null.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous illustre l'utilisation de cet opérateur.  
  
```  
-- This query returns the freight cost per user,  
-- for products, averaged by month.   
With Member [Measures].[Freight Per Customer] as  
    [Measures].[Internet Freight Cost]  
    /   
    [Measures].[Customer Count]  
  
SELECT   
    [Ship Date].[Calendar].[Calendar Year] Members ON 0,  
    [Product].[Category].[Category].Members ON 1  
FROM  
    [Adventure Works]  
WHERE  
    ([Measures].[Freight Per Customer])  
```  
  
 Diviser une valeur non nulle ou non NULL par zéro ou NULL retourne la valeur Infinity qui est affichée dans les résultats de la requête comme valeur "1.#INF". Dans la plupart des cas, vous devez vérifier la présence d'une division par zéro pour éviter cette situation. L'exemple suivant illustre les opérations suivantes :  
  
 `//Returns 1.#INF when Internet Sales Amount is zero or null`  
  
 `Member [Measures].[Reseller to Internet Ratio] AS`  
  
 `[Measures].[Reseller Sales Amount]`  
  
 `/`  
  
 `[Measures].[Internet Sales Amount]`  
  
 `//Traps the division by zero scenario and returns null instead of 1.#INF`  
  
 `Member [Measures].[Reseller to Internet Ratio With Error Handling] AS`  
  
 `IIF([Measures].[Internet Sales Amount]=0, NULL,`  
  
 `[Measures].[Reseller Sales Amount]`  
  
 `/`  
  
 `[Measures].[Internet Sales Amount])`  
  
 `SELECT`  
  
 `{[Measures].[Reseller to Internet Ratio],[Measures].[Reseller to Internet Ratio With Error Handling]} ON 0,`  
  
 `[Product].[Category].[Category].Members ON 1`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
 `WHERE([Date].[Calendar].[Calendar Year].&[2001])`  
  
## <a name="see-also"></a>Voir aussi  
 [&#41;de &#40;IIf ](../mdx/iif-mdx.md)   
 [Référence des opérateurs MDX &#40;&#41;MDX ](../mdx/mdx-operator-reference-mdx.md)  
  
  

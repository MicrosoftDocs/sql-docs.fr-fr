---
description: IS (MDX)
title: IS (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: eab5fc86d89fccbe6ae56c4dba78ccde60e26d50
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88387345"
---
# <a name="is-mdx"></a>IS (MDX)


  Effectue une comparaison logique sur deux expressions d'objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Expression1 IS ( Expression2 | NULL )  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Expression1*  
 Expression MDX (Multidimensional Expressions) valide retournant une référence à un objet MDX.  
  
 *Expression2*  
 Expression MDX valide retournant une référence à un objet MDX.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur booléenne qui retourne **true** si les deux arguments font référence au même objet ; Sinon, **false**. Si le mot clé **null** est spécifié, l’opérateur retourne la **valeur true** si *expression1* est **null**; Sinon, **false**.  
  
## <a name="remarks"></a>Notes  
 L’opérateur **is** est souvent utilisé pour déterminer si les tuples et les membres sont idempotent, ce qui signifie qu’ils sont exactement équivalents.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment utiliser l’opérateur **is** pour vérifier si le membre actuel sur un axe est un membre spécifique :  
  
 `With`  
  
 `//Returns TRUE if the currentmember is Bikes`  
  
 `Member [Measures].[IsBikes?] AS`  
  
 `[Product].[Category].CurrentMember IS [Product].[Category].&[1]`  
  
 `SELECT`  
  
 `{[Measures].[IsBikes?]} ON 0,`  
  
 `[Product].[Category].[Category].Members ON 1`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des opérateurs MDX &#40;&#41;MDX ](../mdx/mdx-operator-reference-mdx.md)  
  
  

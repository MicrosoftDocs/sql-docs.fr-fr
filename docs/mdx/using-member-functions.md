---
description: Utilisation de fonctions de membre
title: Utilisation des fonctions membres | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 4cec35f8d09d671b3b426ff5ac9e18797df716ed
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88491406"
---
# <a name="using-member-functions"></a>Utilisation de fonctions de membre


  Une fonction de membre est une fonction MDX (Multidimensional Expressions) qui retourne un membre. Les fonctions de membre, comme les fonctions de tuple et les fonctions de définition, sont essentielles à la négociation des structures multidimensionnelles de la syntaxe MDX (Multidimensional Expressions) présentes dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
 Parmi les nombreuses fonctions membres de MDX, le plus important est la fonction **CurrentMember** , qui est utilisée pour déterminer le membre actuel sur une hiérarchie. La requête suivante montre comment l’utiliser, ainsi que les fonctions **parent**, **Ancestor**et **PrevMember** :  
  
 `WITH`  
  
 `//Returns the name of the currentmember on the Calendar hierarchy`  
  
 `MEMBER MEASURES.[CurrentMemberDemo] AS [Date].[Calendar].CurrentMember.Name`  
  
 `//Returns the name of the parent of the currentmember on the Calendar hierarchy`  
  
 `MEMBER MEASURES.[ParentDemo] AS [Date].[Calendar].CurrentMember.Parent.Name`  
  
 `//Returns the name of the ancestor of the currentmember on the Calendar hierarchy at the Year level`  
  
 `MEMBER MEASURES.[AncestorDemo] AS ANCESTOR([Date].[Calendar].CurrentMember, [Date].[Calendar].[Calendar Year]).Name`  
  
 `//Returns the name of the member before the currentmember on the Calendar hierarchy`  
  
 `MEMBER MEASURES.[PrevMemberDemo] AS [Date].[Calendar].CurrentMember.Prevmember.Name`  
  
 `SELECT{MEASURES.[CurrentMemberDemo],MEASURES.[ParentDemo],MEASURES.[AncestorDemo],MEASURES.[PrevMemberDemo] } ON 0,`  
  
 `[Date].[Calendar].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;syntaxe MDX&#41;](../mdx/functions-mdx-syntax.md)   
 [Utilisation des fonctions tuple](../mdx/using-tuple-functions.md)   
 [Utilisation de fonctions de jeu](../mdx/using-set-functions.md)  
  
  

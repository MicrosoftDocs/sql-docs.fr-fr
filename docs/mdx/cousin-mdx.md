---
description: Cousin (MDX)
title: Cousin (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: eff78bb06f935311f178d2e39be11bec3e959776
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88500542"
---
# <a name="cousin-mdx"></a>Cousin (MDX)


  Retourne le membre enfant avec la même position relative sous un membre parent que le membre enfant spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cousin( Member_Expression , Ancestor_Member_Expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *Member_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Ancestor_Member_Expression*  
 Expression de membre MDX (Multidimensional Expressions) valide qui retourne un membre ancêtre.  
  
## <a name="remarks"></a>Notes  
 Cette fonction agit sur l'ordre et la position des membres au sein des niveaux. En présence de deux hiérarchies, lorsque la première possède quatre niveaux et la deuxième cinq, le cousin du troisième niveau de la première hiérarchie est le troisième niveau de la deuxième hiérarchie.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous récupère le cousin du quatrième trimestre de l'année fiscale 2002 en se basant sur son ancêtre au niveau de l'année fiscale 2003. Le cousin récupéré est le quatrième trimestre de l'année fiscale 2003.  
  
```  
SELECT Cousin   
   ( [Date].[Fiscal].[Fiscal Quarter].[Q4 FY 2002],  
     [Date].[Fiscal].[FY 2003]  
   ) ON 0  
FROM [Adventure Works]  
```  
  
 L'exemple ci-dessous récupère le cousin du mois de juillet de l'année fiscale 2002 en se basant sur son ancêtre au niveau du deuxième trimestre de l'année fiscale 2004. Le cousin récupéré est le mois d'octobre 2003.  
  
```  
SELECT Cousin   
   ([Date].[Fiscal].[Month].[July 2002] ,  
    [Date].[Fiscal].[Fiscal Quarter].[Q2 FY 2004]  
   ) ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

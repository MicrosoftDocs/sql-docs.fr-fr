---
description: MemberValue (MDX)
title: MemberValue (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: aaa7592c5f2d3413a63fbc0867bb8857e5d87d73
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88483792"
---
# <a name="membervalue-mdx"></a>MemberValue (MDX)


  Retourne la valeur d'un membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression.MemberValue  
```  
  
## <a name="arguments"></a>Arguments  
 *Member_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui prend la valeur d'un membre.  
  
## <a name="return-value"></a>Valeur de retour  
 La valeur de membre retournée contient les informations suivantes, répertoriées ci-dessous selon leur ordre d'apparition dans cette valeur :  
  
-   La liaison de la valeur, si elle a été définie.  
  
-   La clé avec le type de données d'origine, s'il n'y a aucune liaison de nom ou si la clé et la légende sont liées à la même colonne.  
  
-   La légende du membre.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-après retourne la liaison de la valeur, la clé de membre et la légende de la première date dans la dimension Date du cube Adventure Works.  
  
```  
WITH MEMBER Measures.ValueColumn as [Date].[Calendar].[July 1, 2001].MemberValue  
MEMBER Measures.KeyColumn as [Date].[Calendar].[July 1, 2001].Member_Key  
MEMBER Measures.NameColumn as [Date].[Calendar].[July 1, 2001].Member_Name  
  
SELECT {Measures.ValueColumn, Measures.KeyColumn, Measures.NameColumn}  ON 0  
from [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

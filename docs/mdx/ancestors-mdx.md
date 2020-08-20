---
description: Ancestors (MDX)
title: Ancêtres (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: d92f15f20c872fbe63db09a55356b5d1e35ff0d6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461681"
---
# <a name="ancestors-mdx"></a>Ancestors (MDX)


  Cette fonction retourne le jeu de tous les ancêtres d'un membre spécifié à un niveau ou une distance spécifiée du membre. Avec [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , le jeu retourné se compose toujours d’un seul membre [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . il ne prend pas en charge plusieurs parents pour un seul membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Level syntax  
Ancestors(Member_Expression, Level_Expression)  
  
Numeric syntax  
Ancestors(Member_Expression, Distance)  
```  
  
## <a name="arguments"></a>Arguments  
 *Member_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *Distance*  
 Expression numérique valide qui spécifie la distance depuis le membre spécifié.  
  
## <a name="remarks"></a>Notes  
 Avec la fonction **ancêtres** , vous fournissez la fonction avec une expression de membre MDX, puis vous fournissez soit une expression MDX d’un niveau qui est un ancêtre de ce membre, soit une expression numérique qui représente le nombre de niveaux au-dessus de ce membre. Avec ces informations, la fonction **ancêtres** retourne le jeu de membres (qui sera un jeu composé d’un membre) à ce niveau.  
  
> [!NOTE]  
>  Pour retourner un membre ancêtre, plutôt qu’un jeu d’ancêtres, utilisez la fonction [Ancestor](../mdx/ancestor-mdx.md) .  
  
 Si une expression de niveau est spécifiée, la fonction **ancêtres** retourne le jeu de tous les ancêtres du membre spécifié au niveau spécifié. Si un membre spécifié n'apparaît pas dans la même hiérarchie en tant que niveau spécifié, la fonction retourne une erreur.  
  
 Si une distance est spécifiée, la fonction **ancêtres** retourne le jeu de tous les membres qui sont le nombre d’étapes spécifiées dans la hiérarchie spécifiée par l’expression de membre. Un membre peut être spécifié en tant que membre d’une hiérarchie d’attribut, d’une hiérarchie définie par l’utilisateur ou, dans certains cas, d’une hiérarchie parent-enfant. Un nombre 1 retourne le jeu des membres au niveau parent ; un nombre 2 retourne le jeu des membres situés au niveau grand-parent (si ce niveau existe). Un nombre 0 retourne le jeu comprenant uniquement le membre lui-même.  
  
> [!NOTE]  
>  Utilisez cette forme de la fonction **ancêtres** pour les cas où le niveau du parent est inconnu ou ne peut pas être nommé.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise la fonction **ancêtres** pour retourner la mesure du montant des ventes sur Internet pour un membre, son parent et son grand-parent. Cet exemple se base sur des expressions de niveau pour spécifier les niveaux à retourner. Les niveaux se trouvent dans la même hiérarchie que le membre spécifié dans l'expression de membre.  
  
```  
SELECT {  
    Ancestors([Product].[Product Categories].[Product].[Mountain-100 Silver, 38],[Product].[Product Categories].[Category]),  
    Ancestors([Product].[Product Categories].[Product].[Mountain-100 Silver, 38],[Product].[Product Categories].[Subcategory]),  
    Ancestors([Product].[Product Categories].[Product].[Mountain-100 Silver, 38],[Product].[Product Categories].[Product])  
    } ON 0,  
[Measures].[Internet Sales Amount] ON 1  
FROM [Adventure Works]  
```  
  
 L’exemple suivant utilise la fonction **ancêtres** pour retourner la mesure du montant des ventes sur Internet pour un membre, son parent et son grand-parent. Cet exemple se base sur des expressions numériques pour spécifier les niveaux retournés. Les niveaux se trouvent dans la même hiérarchie que le membre spécifié dans l'expression de membre.  
  
```  
SELECT {  
   Ancestors(  
      [Product].[Product Categories].[Product].[Mountain-100 Silver, 38],2  
      ),  
   Ancestors(  
      [Product].[Product Categories].[Product].[Mountain-100 Silver, 38],1  
      ),  
   Ancestors(  
      [Product].[Product Categories].[Product].[Mountain-100 Silver, 38],0  
      )  
   } ON 0,  
[Measures].[Internet Sales Amount] ON 1  
FROM  [Adventure Works]  
```  
  
 L’exemple suivant utilise la fonction **ancêtres** pour retourner la mesure Internet Sales Amount pour le parent d’un membre d’une hiérarchie d’attribut. Cet exemple se base sur une expression numérique pour spécifier le niveau retourné. Du fait que le membre dans l'expression de membre correspond à un membre issu d'une hiérarchie d'attribut, son parent est le niveau [All].  
  
```  
SELECT {  
   Ancestors(  
      [Product].[Product].[Mountain-100 Silver, 38],1  
      )  
   } ON 0,  
[Measures].[Internet Sales Amount] ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

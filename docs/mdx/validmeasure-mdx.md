---
description: ValidMeasure (MDX)
title: ValidMeasure (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: cd2bc2385a3445076636fb1b4001120463d1cd89
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494829"
---
# <a name="validmeasure-mdx"></a>ValidMeasure (MDX)


  Retourne la valeur d'une mesure dans un cube en imposant le niveau All (Tous) (ou le membre par défaut s'il ne peut être agrégé) aux dimensions inapplicables lors du retour du résultat d'un tuple spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ValidMeasure(Tuple_Expression)   
```  
  
## <a name="arguments"></a>Arguments  
 *Tuple_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un tuple.  
  
## <a name="remarks"></a>Notes  
 La fonction **ValidMeasure** retourne la valeur d’un tuple, en ignorant les attributs qui n’ont aucune relation avec le groupe de mesures de la mesure dont le tuple retourne la valeur. Un attribut peut être sans relation avec une mesure pour deux raisons :  
  
-   La dimension de l'attribut n'a aucune relation avec le groupe de mesures de la mesure dans le tuple.  
  
-   La dimension de l'attribut n'a pas de relation avec le groupe de mesures de la mesure, mais l'attribut de granularité n'est pas l'attribut de clé, et l'attribut de granularité n'a pas de relation directe avec l'attribut dans le tuple.  
  
 Le comportement spécifié par cette fonction est le comportement côté serveur par défaut et est contrôlé par la propriété **IgnoreUnrelatedDimensions** sur l’objet de groupe de mesures.  
  
 Pour chaque attribut du tuple spécifié avec granularité (c'est-à-dire lorsque le membre du tuple n'est pas le membre All), la coordonnée actuelle de cet attribut est déplacée comme suit :  
  
-   Les attributs associés au membre d'attribut spécifié sont déplacés vers le membre existant avec le membre actuel.  
  
-   Les attributs associés au membre d'attribut spécifié sont déplacés vers le membre All (ou le membre par défaut si la hiérarchie ne peut pas être agrégée).  
  
-   Les attributs non associés sont déplacés vers le membre All (en fonction de la mesure).  
  
## <a name="example"></a>Exemple  
 La requête suivante montre comment la fonction ValidMeasure peut être utilisée pour remplacer le comportement de la propriété IgnoreUnrelatedDimensions. Dans le cube Adventure Works, le jeu IgnoreUnrelatedDimensions du groupe de mesures Cibles de ventes est défini sur False ; puisque la dimension Date est jointe à ce groupe de mesures à la granularité Trimestre calendrier, cela signifie que la mesure Sales Quota, par défaut, retourne null sous Trimestre calendrier (bien qu'il y ait également un calcul dans le Script MDX qui alloue aussi des valeurs au niveau Mois). L'utilisation de la fonction ValidMeasure dans une mesure calculée peut permettre de faire en sorte que la mesure Sales Quota se comporte comme si IgnoreUnrelatedDimensions avait été défini sur True et de forcer Sales Quota à afficher la valeur du Trimestre calendrier actuel.  
  
```  
WITH MEMBER MEASURES.VTEST AS VALIDMEASURE([Measures].[Sales Amount Quota])  
SELECT {[Measures].[Sales Amount Quota], MEASURES.VTEST} ON 0,  
[Date].[Calendar].MEMBERS ON 1  
FROM [Adventure Works]  
```  
  
 De la même façon, le groupe de mesures Cibles de ventes n'a aucune relation avec la dimension Promotion, il retournera donc null sous le membre Tous d'une hiérarchie de la dimension Promotion. Une fois encore, ce comportement peut être modifié à l'aide de ValidMeasure :  
  
 `WITH MEMBER MEASURES.VTEST AS VALIDMEASURE([Measures].[Sales Amount Quota])`  
  
 `SELECT {[Measures].[Sales Amount Quota], MEASURES.VTEST} ON 0,`  
  
 `[Promotion].[Promotions].members ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

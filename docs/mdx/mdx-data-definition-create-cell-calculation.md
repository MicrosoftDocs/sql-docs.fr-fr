---
description: Définition de données MDX - CREATE CELL CALCULATION
title: Instruction CREATe CELL CALCULation (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 94f2a2286b72aac5a8698fad7ba0085f2a6ad8d0
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196996"
---
# <a name="mdx-data-definition---create-cell-calculation"></a>Définition de données MDX - CREATE CELL CALCULATION


  Crée un calcul qui évalue une expression MDX (Multidimensional Expressions) par rapport à un ensemble spécifié de tuples au sein d'un cube.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
[WITH <CELL CALCULATION clause> Calculation_Name  
   [,WITH <CELL CALCULATION clause> Calculation_Name...n]  
CREATE CELL CALCULATION CURRENTCUBE | Cube_Name.Calculation_Name   
  
<CELL CALCULATION clause> ::=  
   FOR Set_Expression AS 'MDX_Expression'   
      [ [ CONDITION = 'Logical_Expression' ]   
    | [ DISABLED = { TRUE | FALSE } ]   
    | [ DESCRIPTION =String ]   
    | [ CALCULATION_PASS_NUMBER = Integer]   
    | [ CALCULATION_PASS_DEPTH = Integer]   
    | [ SOLVE_ORDER = Integer]   
    | [ Calculation_Name= Scalar_Expression ], ...n]  
```  
  
## <a name="arguments"></a>Arguments  
 *Cube_Name*  
 Chaîne valide qui précise le nom d'un cube.  
  
 *Calculation_Name*  
 Chaîne valide qui précise le nom d'un calcul de cellule.  
  
 *Set_Expression*  
 Expression MDX valide qui retourne un jeu.  
  
 *Chaîne*  
 Valeur de chaîne valide.  
  
 *MDX_Expression*  
 Expression MDX valide.  
  
 *Logical_Expression*  
 Expression logique MDX valide.  
  
 *Integer*  
 Valeur d'entier valide.  
  
 *Calculation_Name*  
 Chaîne valide qui précise le nom d'une propriété de calcul de cellule.  
  
 *Scalar_Expression*  
 Expression scalaire MDX valide.  
  
## <a name="remarks"></a>Notes  
 En utilisant des cellules calculées, l'application cliente peut spécifier une valeur de cumul pour un jeu de cellules particulier, et non pas pour la totalité d'un jeu de cellules comme c'est le cas dans une formule de cumul personnalisée ou dans un membre calculé. Par exemple, il est possible de spécifier que toute cellule d'un jeu défini par `{[Canada],[Time].[2000]}` peut contenir une valeur définie par une formule. Toutes les autres cellules qui ne sont pas contenues dans ce jeu sont calculées normalement.  
  
> [!NOTE]  
>  La notation BNF (Backus-Naur Form) de `{*(<comment> | <whitespace> | <newline>)}` sera analysée en tant que `{*}` à des fins de compatibilité descendante.  
  
## <a name="see-also"></a>Voir aussi  
 [Création de Session-Scoped des cellules calculées](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-session-scoped-calculated-cells)   
 [Création de calculs de cellules Query-Scoped &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-query-scoped-cell-calculations)   
 [Génération de calculs de cellules dans MDX &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-build-cell-calculations)   
 [Utilisation des propriétés de cellule &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-using-cell-properties)   
 [Contenu de FORMAT_STRING &#40;&#41;MDX ](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-format-string-contents)   
 [Contenu FORE_COLOR et BACK_COLOR &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-fore-color-and-back-color-contents)   
 [Instructions de définition de données MDX &#40;&#41;MDX ](../mdx/mdx-data-definition-statements-mdx.md)  
  

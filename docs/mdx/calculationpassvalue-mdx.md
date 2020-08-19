---
description: CalculationPassValue (MDX)
title: CalculationPassValue (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 98d30326b709f7bd651b7941e48d412a7b875ffd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88425041"
---
# <a name="calculationpassvalue-mdx"></a>CalculationPassValue (MDX)


  Retourne la valeur numérique ou la valeur de chaîne d'une expression MDX (Multidimensional Expressions) évaluée pendant le test de calcul spécifié d'un cube.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
      Numeric syntax  
CalculationPassValue(Numeric_Expression,Pass_Value [, ABSOLUTE | RELATIVE [,ALL]])  
String syntax  
CalculationPassValue(String_Expression ,Pass_Value [, ABSOLUTE | RELATIVE [,ALL]])  
```  
  
## <a name="arguments"></a>Arguments  
 *Numeric_Expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
 *String_Expression*  
 Expression de chaîne valide qui correspond généralement à une expression MDX (Multidimensional Expressions) valide des coordonnées des cellules qui retournent un nombre exprimé sous forme de chaîne.  
  
 *Pass_Value*  
 Expression numérique valide qui précise le numéro du test de calcul.  
  
 ABSOLUTE  
 Valeur de l’indicateur d’accès qui spécifie que le paramètre *Pass_Value* contient l’index de base zéro du test de calcul. ABSOLUTE est la valeur d'indicateur d'accès par défaut si aucune valeur de ce type n'est spécifiée.  
  
 RELATIVE  
 Valeur de l’indicateur d’accès qui spécifie que le paramètre *Pass_Value* contient un décalage relatif par rapport au test de calcul du calcul de déclenchement. Si le décalage se résout à un index de test de calcul inférieur à 0, le test de calcul 0 est utilisé et aucune erreur ne survient.  
  
 ALL  
 Lorsque vous définissez cet indicateur, toutes les valeurs sont NULL, sauf les valeurs chargées par le moteur de stockage. Si l'indicateur n'est pas défini, les valeurs sont agrégées sans qu'aucun calcul ne soit appliqué.  
  
## <a name="remarks"></a>Notes  
 Si une expression numérique est spécifiée, la fonction retourne une valeur numérique en évaluant l'expression numérique MDX précisée dans le test de calcul spécifié, et éventuellement modifiée par un indicateur d'accès et un modificateur d'indicateur d'accès.  
  
 Si une expression de chaîne est fournie, la fonction retourne une valeur de chaîne en évaluant l’expression de chaîne MDX spécifiée dans le test de calcul spécifié, et éventuellement modifiée par un indicateur d’accès et un modificateur d’indicateur d’accès *.*  
  
 Avec la résolution de récurrence automatique dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , cette fonction n’a que peu d’utilité.  
  
> [!NOTE]  
>  Seuls les administrateurs peuvent utiliser la fonction **CalculationPassValue** dans un script MDX. Une erreur se produit si un script MDX contenant cette fonction est exécuté dans le contexte d'un rôle qui ne dispose pas des droits d'administrateur.  
  
## <a name="see-also"></a>Voir aussi  
 [CalculationCurrentPass&#41;MDX &#40;](../mdx/calculationcurrentpass-mdx.md)   
 [&#41;de &#40;IIf ](../mdx/iif-mdx.md)   
 [Guide de référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  

---
description: Opérateurs (expression SSIS)
title: Opérateurs (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS, operators
- SQL Server Integration Services, operators
- operators [Integration Services]
- expressions [Integration Services], operators
ms.assetid: 33df3a3d-1f5c-429b-a3b9-52b7d8689089
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f2c4ddb9dad3a1b6c922201906628ad3b6d25f94
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88477476"
---
# <a name="operators-ssis-expression"></a>Opérateurs (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cette section décrit les opérateurs du langage d'expression ainsi que l'associativité et la priorité des opérateurs utilisées par l'évaluateur d'expression.  
  
 Le tableau suivant indique les rubriques relatives aux opérateurs décrits dans cette section.  
  
|Opérateur|Description|  
|--------------|-----------------|  
|[Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md)|Convertit une expression d'un type de données en un autre type de données.|  
|[&#40;&#41; &#40;Parenthèses&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/parentheses-ssis-expression.md)|Identifie l'ordre d'évaluation des expressions.|  
|[+ &#40;Additionner&#41; &#40;SSIS&#41;](../../integration-services/expressions/add-ssis.md)|Additionne deux expressions numériques.|  
|[+ &#40;Concaténer&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/concatenate-ssis-expression.md)|Concatène deux expressions.|  
|[- &#40;Soustraire&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/subtract-ssis-expression.md)|Soustrait la deuxième expression numérique de la première.|  
|[- &#40;Inverser&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/negate-ssis-expression.md)|Inverse une expression numérique.|  
|[&#42; &#40;Multiplier&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/multiply-ssis-expression.md)|Multiplie deux expressions numériques.|  
|[/ (Diviser) &#40;expression SSIS&#41;](../../integration-services/expressions/divide-ssis-expression.md)|Divise la première expression numérique par la deuxième.|  
|[% &#40;Modulo&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/modulo-ssis-expression.md)|Fournit le reste entier de la division de la première expression numérique par la deuxième.|  
|[&#124;&#124; &#40;OR logique&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/logical-or-ssis-expression.md)|Effectue une opération OR logique.|  
|[&& &#40;ET logique&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/logical-and-ssis-expression.md)|Effectue une opération AND logique.|  
|[\! &#40;NOT logique&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/logical-not-ssis-expression.md)|Inverse un opérande booléen.|  
|[&#124; &#40;opération OR inclusive au niveau du bit&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/bitwise-inclusive-or-ssis-expression.md)|Effectue une opération OR au niveau du bit avec deux valeurs entières.|  
|[^ &#40;OR exclusif au niveau du bit&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/bitwise-exclusive-or-ssis-expression.md)|Effectue une opération OR exclusive au niveau du bit avec deux valeurs entières.|  
|[& &#40;AND au niveau du bit&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/bitwise-and-ssis-expression.md)|Effectue une opération AND au niveau du bit avec deux valeurs entières.|  
|[~ &#40;NOT au niveau du bit&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/bitwise-not-ssis-expression.md)|Effectue une négation au niveau du bit d'un entier.|  
|[== &#40;Égal&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/equal-ssis-expression.md)|Effectue une comparaison pour déterminer si deux expressions sont égales.|  
|[\!= &#40;Non égal&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/unequal-ssis-expression.md)|Effectue une comparaison pour déterminer si deux expressions sont différentes.|  
|[&#62; &#40;Supérieur à&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/greater-than-ssis-expression.md)|Effectue une comparaison pour déterminer si la première expression est supérieure à la deuxième.|  
|[&#60; &#40;Inférieur à&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/less-than-ssis-expression.md)|Effectue une comparaison pour déterminer si la première expression est inférieure à la deuxième.|  
|[&#62;= &#40;Supérieur ou égal à&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/greater-than-or-equal-to-ssis-expression.md)|Effectue une comparaison pour déterminer si la première expression est supérieure ou égale à la deuxième.|  
|[&#60;= &#40;Inférieur ou égal à&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/less-than-or-equal-to-ssis-expression.md)|Effectue une comparaison pour déterminer si la première expression est inférieure ou égale à la deuxième.|  
|[? : &#40;Conditionnel&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/conditional-ssis-expression.md)|Renvoie une des deux expressions selon l'évaluation d'une expression booléenne.|  
  
 Pour plus d’informations sur la position de chaque opérateur dans la hiérarchie des priorités, consultez [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)   
 [Exemples d’expressions Integration Services avancées](../../integration-services/expressions/examples-of-advanced-integration-services-expressions.md)   
 [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  

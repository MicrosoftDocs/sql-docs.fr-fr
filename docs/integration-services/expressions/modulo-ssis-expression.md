---
description: (Modulo) (expression SSIS)
title: (Modulo) (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- '% (modulo operator)'
- remainder of division operation
- modulo operator (%)
ms.assetid: e2920821-2f5b-4c76-8db8-8b9eddf4606f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14ea6b261c63b06d5e6b3fbeb77822c0d3c6836f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457205"
---
# <a name="modulo-ssis-expression"></a>(Modulo) (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Fournit le reste entier de la division de la première expression numérique par la deuxième.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dividend % divisor  
  
```  
  
## <a name="arguments"></a>Arguments  
 *dividend*  
 Expression numérique à diviser. *dividend* peut être n’importe quelle expression numérique valide. Pour plus d’informations, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 *divisor*  
 Expression numérique par laquelle diviser le dividende. *divisor* peut être n’importe quelle expression numérique valide, sauf zéro.  
  
## <a name="result-types"></a>Types des résultats  
 Déterminés par les types de données des deux arguments. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
## <a name="remarks"></a>Remarques  
 Les valeurs des deux expressions doivent s'évaluer à des types de données entier signé ou non signé.  
  
 Si l'un des opérandes est NULL, le résultat est NULL.  
  
 Un modulo égal à zéro n'est pas autorisé.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant calcule le modulo à partir de deux littéraux numériques. Le résultat est 3.  
  
```  
42 % 13  
```  
  
 L’exemple suivant calcule le modulo à partir de la colonne **SalesQuota** et d’un littéral numérique.  
  
```  
SalesQuota % 12  
```  
  
 L’exemple suivant calcule le modulo à partir de deux variables numériques : **Sales$** et **Month**. La variable **Sales$** doit figurer entre crochets car elle contient le caractère « $ ». Pour plus d’informations, consultez [Identificateurs &#40;SSIS&#41;](../../integration-services/expressions/identifiers-ssis.md).  
  
```  
@[Sales$] % @Month  
```  
  
 L’exemple suivant utilise l’opérateur modulo pour déterminer si la valeur de la variable **Value** est paire ou impaire, et utilise l’opérateur conditionnel pour renvoyer une chaîne décrivant le résultat. Pour plus d’informations, consultez [? : &#40;Conditionnel&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/conditional-ssis-expression.md).  
  
```  
@Value % 2 == 0? "even":"odd"  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  

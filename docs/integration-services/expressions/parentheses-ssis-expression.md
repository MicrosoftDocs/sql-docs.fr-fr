---
description: () (Parenthèses) (expression SSIS)
title: () (Parenthèses) (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- () (parentheses operator)
- evaluation order [Integration Services]
- parentheses operator ()
ms.assetid: 931e10eb-1707-4121-b2f1-43565561119f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 970ac5479d0d09276aae8eb095199abbbee910c5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88477532"
---
# <a name="-parentheses-ssis-expression"></a>() (Parenthèses) (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Identifie l'ordre d'évaluation des expressions. Les expressions entre parenthèses ont la priorité d'évaluation la plus élevée. L'évaluation des expressions imbriquées placées entre parenthèses commence par celle située le plus à l'intérieur, puis se poursuit jusqu'à celle située le plus à l'extérieur.  
  
 Les parenthèses permettent également de faciliter la compréhension des expressions complexes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
(expression)  
  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Peut être toute expression valide.  
  
## <a name="result-types"></a>Types des résultats  
 Le type de données *expression*. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Cet exemple illustre la façon dont l'utilisation des parenthèses modifie la priorité des opérateurs. La première expression totalise 100 tandis que la seconde cumule à 31.  
  
```  
(5 + 5) * (4 + 6)  
5 + 5 * 4 + 6  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  

---
description: Diviser (expression SSIS)
title: Diviser (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- / (divide)
- divide operator (/)
ms.assetid: 5bde9223-872d-443e-8a27-57735e1d8f3d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 516d8705f38f3bdd7234f1975f5188d32f501efe
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88430601"
---
# <a name="divide-ssis-expression"></a>Diviser (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Divise la première expression numérique par la deuxième.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dividend / divisor  
  
```  
  
## <a name="arguments"></a>Arguments  
 *dividend*  
 Expression numérique à diviser. *dividend* peut être n’importe quelle expression numérique valide. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 *divisor*  
 Expression numérique par laquelle diviser le dividende. *divisor* peut être n’importe quelle expression numérique valide, sauf zéro.  
  
## <a name="result-types"></a>Types des résultats  
 Déterminés par les types de données des deux arguments. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
## <a name="remarks"></a>Notes  
 Si l'un des opérandes est NULL, le résultat est NULL.  
  
 La division par zéro n'est pas autorisée. L’évaluation de la sous-expression *divisor* génère l’une des erreurs suivantes :  
  
-   Si la sous-expression *divisor* qui donne la valeur zéro est une constante, l’erreur se produit au moment de la conception et provoque l’échec de la validation de l’expression.  
  
-   Si la sous-expression *divisor* qui donne la valeur zéro contient des variables, mais aucune colonne d’entrée, le composant auquel l’expression appartient ne parvient pas à valider l’exécution préalable du package.  
  
-   Si la sous-expression *divisor* qui donne la valeur zéro contient des colonnes d’entrée, l’erreur se produit au moment de l’exécution et est gérée en fonction des règles de flux d’erreur du composant de flux de données.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant divise deux littéraux numériques.  
  
```  
25 / 5  
```  
  
 L’exemple suivant divise les valeurs de la colonne **ListPrice** par celles de la colonne **StandardCost** .  
  
```  
ListPrice / StandardCost  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  

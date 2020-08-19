---
description: ^ (OR exclusif au niveau du bit) (expression SSIS)
title: ^ (OR exclusif au niveau du bit) (expression SSIS)| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- ^ (bitwise exclusive OR operator)
- bitwise exclusive OR (^)
ms.assetid: 6ac53cab-29c4-4835-9f87-371b058b2f38
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8887ddc1b7b7fab04a510d856f22e90723858f51
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88425581"
---
# <a name="-bitwise-exclusive-or-ssis-expression"></a>^ (OR exclusif au niveau du bit) (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Effectue une opération OR exclusive au niveau du bit avec deux valeurs entières. Cette fonction compare chaque bit de son premier opérande au bit correspondant de son second opérande. Si un bit a pour valeur 0 et que l'autre a pour valeur 1, le bit obtenu correspondant a pour valeur 1. Si les deux bits ont pour valeur 0 ou 1, le bit obtenu correspondant a pour valeur 0.  
  
 Les deux conditions doivent être de type de données signed integer ou unsigned integer.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
integer_expression1 ^ integer_expression2  
  
```  
  
## <a name="arguments"></a>Arguments  
 *integer_expression1, integer_expression2*  
 Toute expression valide de type de données signed integer ou unsigned integer. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="result-types"></a>Types des résultats  
 Déterminés par les types de données des deux arguments. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
## <a name="remarks"></a>Notes  
 Si l'une des deux conditions est NULL, le résultat de l'expression est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L’exemple suivant effectue une opération OR exclusive au niveau du bit entre les variables **NumberA** et **NumberB**. La variable**NumberA** contient la valeur 3 (00000011) et la variable **NumberB** contient la valeur 7 (00000111).  
  
```  
@NumberA ^ @NumberB  
```  
  
 L'expression renvoie la valeur 4 (00000100).  
  
 00000011  
  
 00000111  
  
 ----------\-  
  
 00000100  
  
 L’exemple suivant effectue une opération OR exclusive au niveau du bit entre les colonnes **ReorderPoint** et **SafetyStockLevel** .  
  
```  
ReorderPoint ^ SafetyStockLevel  
```  
  
 Si la colonne **ReorderPoint** contient la valeur 10 et la colonne **SafetyStockLevel** la valeur 8, l’expression renvoie la valeur 2 (00000010).  
  
 00001010  
  
 00001000  
  
 ----------\-  
  
 00000010  
  
 L'exemple suivant effectue une opération OR exclusive au niveau du bit entre deux entiers.  
  
```  
3 ^ 5   
```  
  
 L'expression renvoie la valeur 6 (00000110).  
  
 00000011  
  
 00000101  
  
 ----------\-  
  
 00000110  
  
## <a name="see-also"></a>Voir aussi  
 [&#124;&#124; &#40;OR logique&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/logical-or-ssis-expression.md)   
 [&#124; &#40;opération OR inclusive au niveau du bit&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/bitwise-inclusive-or-ssis-expression.md)   
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  

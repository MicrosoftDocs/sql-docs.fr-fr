---
description: HEX (expression SSIS)
title: HEX (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- hexadecimal data
- HEX function
ms.assetid: f5d471ee-aeef-421c-b6e1-55b9676c3842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a2fdba3ec151b363766ffb29e88afbdfc4575bc2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484469"
---
# <a name="hex-ssis-expression"></a>HEX (expression SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Renvoie une chaîne représentant la valeur hexadécimale d'un entier.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HEX(integer_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *integer_expression*  
 Entier signé ou non signé.  
  
## <a name="result-types"></a>Types des résultats  
 DT_WSTR  
  
## <a name="remarks"></a>Notes  
 HEX retourne null si *integer_expression* est null.  
  
 L’argument *integer_expression* doit correspondre à un nombre entier. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Le résultat obtenu ne comprend pas de qualificateurs tels que le préfixe « 0x ». Pour inclure un préfixe, utilisez l'opérateur « + » (concaténer). Pour plus d’informations, consultez [+ &#40;Concaténer&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/concatenate-ssis-expression.md).  
  
 Les lettres « A » à « F », utilisées en notation hexadécimale, apparaissent en caractères majuscules.  
  
 La longueur de la chaîne obtenue pour les types de données entiers est la suivante :  
  
-   Les types de données DT_I1 et DT_UI1 renvoient une chaîne d'une longueur maximale de 2.  
  
-   Les types de données DT_I2 et DT_UI2 renvoient une chaîne d'une longueur maximale de 4.  
  
-   Les types de données DT_I4 et DT_UI4 renvoient une chaîne d'une longueur maximale de 8.  
  
-   Les types de données DT_I8 et DT_UI8 renvoient une chaîne d'une longueur maximale de 16.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant utilise un littéral numérique. La fonction retourne la valeur 190.  
  
```  
HEX(400)   
```  
  
 L’exemple suivant utilise la colonne **ReorderPoint** . Le type de données de la colonne est **smallint**. Si la variable **ReorderPoint** a pour valeur 750, la fonction renvoie 2EE.  
  
```  
HEX(ReorderPoint)   
```  
  
 L’exemple suivant utilise la variable système **LocaleID**. Si la variable **LocaleID** a pour valeur 1033, la fonction renvoie 409.  
  
```  
HEX(@LocaleID)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  

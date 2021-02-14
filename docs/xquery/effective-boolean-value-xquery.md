---
title: Valeur booléenne effective (XQuery) | Microsoft Docs
description: En savoir plus sur les valeurs booléennes effectives dans XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- effective Boolean value [XQuery]
- Boolean values
- XQuery, effective Boolean values
- EBV
ms.assetid: 506682b1-b6c9-45e2-aa54-7abd5844c3f1
author: rothja
ms.author: jroth
ms.openlocfilehash: c9e390546feb848f965df2bfd60a2cc68c8fb3ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341550"
---
# <a name="effective-boolean-value-xquery"></a>Valeur booléenne effective (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Voici les valeurs booléennes effectives :  
  
-   False si l'opérande est une séquence vide ou une valeur booléenne fausse.  
  
-   Sinon, la valeur est true.  
  
 La valeur booléenne effective peut être calculée pour les expressions qui renvoient une seule valeur booléenne, une séquence de nœuds ou une séquence vide. Notez que la valeur booléenne est calculée de manière implicite lors du traitement des types d'expressions suivants :  
  
-   Expressions logiques  
  
-   La [fonction not](../xquery/functions-on-boolean-values-not-function.md)  
  
-   Clause WHERE d'une expression FLWOR  
  
-   [Expressions conditionnelles](../xquery/conditional-expressions-xquery.md)  
  
-   [Expressions quantifiées](../xquery/quantified-expressions-xquery.md)  
  
 Voici un exemple de valeur booléenne effective. Lorsque l’expression **If** est traitée, la valeur booléenne effective de la condition est déterminée. Puisque l'expression `/a[1]` renvoie une séquence vide, la valeur booléenne effective est false. Le résultat est renvoyé au format XML avec un nœud de texte (false).  
  
```  
value is false  
DECLARE @x XML  
SET @x = '<b/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Dans l'exemple suivant, la valeur booléenne effective est true puisque l'expression renvoie une séquence non vide.  
  
```  
DECLARE @x XML  
SET @x = '<a/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Lors de l’interrogation de colonnes ou de variables **XML** typées, vous pouvez avoir des nœuds de type booléen. Dans ce cas, les **données ()** retournent une valeur booléenne. Si l'expression de la requête renvoie une valeur booléenne true, la valeur booléenne effective est true, comme le montre l'exemple qui suit. Les points suivants sont également illustrés dans l'exemple suivant :  
  
-   Une collection de schémas XML est créée. L’élément \<b> dans la collection est de type booléen.  
  
-   Une variable **XML** typée est créée et interrogée.  
  
-   L'expression `data(/b[1])` renvoie une valeur booléenne true. Par conséquent, la valeur booléenne effective est true dans ce cas.  
  
-   L’expression `data(/b[2])` retourne une valeur booléenne false. Par conséquent, la valeur booléenne effective est false dans ce cas.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="s" type="string"/>  
      <element name="b" type="boolean"/>  
</schema>'  
go  
DECLARE @x XML(SC)  
SET @x = '<b>true</b><b>false</b>'  
SELECT @x.query('if (data(/b[1])) then "true" else "false"')  
SELECT @x.query('if (data(/b[2])) then "true" else "false"')  
go  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Notions de base de XQuery](../xquery/xquery-basics.md)   
 [Instruction et itération FLWOR &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)  
  
  

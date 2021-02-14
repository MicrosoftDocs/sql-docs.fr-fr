---
title: Gestion des erreurs (XQuery) | Microsoft Docs
description: En savoir plus sur la gestion des erreurs dans XQuery et afficher des exemples de gestion des erreurs dynamiques.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- static errors
- errors [XQuery]
- XQuery, error handling
- dynamic errors [XQuery]
ms.assetid: 7dee3c11-aea0-4d10-9126-d54db19448f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e6b8b53e4519eb88f94f9521434b2e48aac40d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335860"
---
# <a name="error-handling-xquery"></a>Gestion des erreurs (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  La spécification W3C permet de déclencher des erreurs de type statiquement ou dynamiquement et définit des erreurs statiques, dynamiques et de type.  
  
## <a name="compilation-and-error-handling"></a>Compilation et gestion des erreurs  
 Des erreurs de compilation sont renvoyées en cas de syntaxe incorrecte des expressions Xquery et des instructions XML DML. La phase de compilation permet de vérifier l'exactitude des types statiques des expressions XQuery et des instructions DML, et d'utiliser des schémas XML pour les inférences de type pour le code XML typé. Des erreurs de type statique sont générées si une expression risque d'échouer lors de l'exécution suite à la violation de la cohérence des types. On trouve parmi les erreurs statiques l'ajout d'une chaîne à un entier ou encore l'interrogation d'un nœud inexistant pour des données typées.  
  
 Conformément à la norme du W3C, les erreurs d'exécution XQuery sont converties en séquences vides. Ces séquences peuvent se propager sous forme de valeur XML vide ou NULL dans le résultat des requêtes selon le contexte d'invocation.  
  
 Une conversion explicite en type correct permet aux utilisateurs d'éviter les erreurs statiques même si les erreurs de conversion à l'exécution sont transformées en séquences vides.  
  
## <a name="static-errors"></a>Erreurs statiques  
 Les erreurs statiques sont renvoyées à l'aide du mécanisme d'erreur [!INCLUDE[tsql](../includes/tsql-md.md)]. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les erreurs de type XQuery sont renvoyées statiquement. Pour plus d’informations, consultez [XQuery et typage statique](../xquery/xquery-and-static-typing.md).  
  
## <a name="dynamic-errors"></a>Erreurs dynamiques  
 Dans XQuery, la plupart des erreurs dynamiques sont mappées à une séquence vide (« () »). Il existe toutefois deux exceptions : les conditions de dépassement dans les fonctions d'agrégation XQuery et les erreurs de validation XML-DML. La plupart des erreurs dynamiques sont mappées à une séquence vide. Sinon, l'exécution de requêtes qui tire parti des index XML peut déclencher des erreurs inattendues. Par conséquent, pour permettre une exécution efficace sans générer d'erreurs inattendues, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] mappe les erreurs dynamiques à ().  
  
 Si l'erreur dynamique devait se produire dans un prédicat, il est courant que son non-déclenchement épargne la sémantique car () est mappé à False. Toutefois, dans certains cas, le renvoi de () au lieu d'une erreur dynamique peut générer des résultats inattendus. Les exemples suivants illustrent ce point.  
  
### <a name="example-using-the-avg-function-with-a-string"></a>Exemple : utilisation de la fonction avg() avec une chaîne  
 Dans l’exemple suivant, la [fonction AVG](../xquery/aggregate-functions-avg.md) est appelée pour calculer la moyenne des trois valeurs. L'une de ces valeurs est une chaîne. L'instance XML étant, dans ce cas, non typée, toutes les données qu'elle contient sont de type atomique non typé. La fonction **AVG ()** convertit d’abord ces valeurs en **XS : double** avant le calcul de la moyenne. Toutefois, la valeur, `"Hello"` , ne peut pas être convertie en **XS : double** et crée une erreur dynamique. Dans ce cas, au lieu de renvoyer une erreur dynamique, la conversion de `"Hello"` en **XS : double** provoque une séquence vide. La fonction **AVG ()** ignore cette valeur, calcule la moyenne des deux autres valeurs et retourne 150.  
  
```  
DECLARE @x xml  
SET @x=N'<root xmlns:myNS="test">  
 <a>100</a>  
 <b>200</b>  
 <c>Hello</c>  
</root>'  
SELECT @x.query('avg(//*)')  
```  
  
### <a name="example-using-the-not-function"></a>Exemple: utilisation de la fonction not  
 Lorsque vous utilisez la [fonction not](../xquery/functions-on-boolean-values-not-function.md) dans un prédicat, par exemple, `/SomeNode[not(Expression)]` , et que l’expression provoque une erreur dynamique, une séquence vide est retournée à la place d’une erreur. L’application de **not ()** à la séquence vide retourne la valeur true, au lieu d’une erreur.  
  
### <a name="example-casting-a-string"></a>Exemple: conversion d'une chaîne  
 Dans l'exemple suivant, la chaîne littérale « NaN » est convertie au format xs:string, puis au format xs:double. Le résultat est un ensemble de lignes vide. Le fait que la chaîne « NaN » ne puisse pas être correctement convertie au format xs:double n'est détecté qu'à l'exécution car elle est d'abord convertie au format xs:string.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double(xs:string("NaN")) ')  
GO  
```  
  
 Toutefois, dans cet exemple, une erreur de type statique se produit.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double("NaN") ')  
GO  
```  
  
#### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 La fonction **FN : Error ()** n’est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Concepts de base de XQuery](../xquery/xquery-basics.md)  
  
  

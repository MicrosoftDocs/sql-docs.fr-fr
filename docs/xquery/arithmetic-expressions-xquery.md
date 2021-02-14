---
title: Expressions arithmétiques (XQuery) | Microsoft Docs
description: En savoir plus sur les expressions arithmétiques dans XQuery et les opérateurs arithmétiques pris en charge.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- expressions [XQuery], arithmetic
- arithmetic expressions
ms.assetid: 90d675bf-56da-459a-9771-8cd13920a9fc
author: rothja
ms.author: jroth
ms.openlocfilehash: f63fd1f6064d28a7ee977162e8993f53f9233519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340478"
---
# <a name="arithmetic-expressions-xquery"></a>Expressions arithmétiques (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Tous les opérateurs arithmétiques sont pris en charge, à l’exception de **IDIV**. Les exemples suivants illustrent l'utilisation de base des opérateurs arithmétiques :  
  
```  
DECLARE @x xml  
SET @x=''  
SELECT @x.query('2 div 2')  
SELECT @x.query('2 * 2')  
```  
  
 Comme **IDIV** n’est pas pris en charge, une solution consiste à utiliser le constructeur **XS : Integer ()** :  
  
```  
DECLARE @x xml  
SET @x=''  
-- Following will not work  
-- SELECT @x.query('2 idiv 2')  
-- Workaround   
SELECT @x.query('xs:integer(2 div 3)')  
```  
  
 Le type issu d'un opérateur arithmétique est basé sur les types des valeurs d'entrée. Si les opérandes sont de types différents, l'un d'eux, voire les deux si nécessaire, est converti dans un type de base primitif courant en fonction de la hiérarchie des types. Pour plus d’informations sur la hiérarchie des types, consultez [règles de conversion de type dans XQuery](../xquery/type-casting-rules-in-xquery.md).  
  
 La promotion du type numérique se produit si les deux opérations sont de types de base numériques différents. Par exemple, si vous ajoutez un **XS : Decimal** à un **XS : double** , vous remplacez tout d’abord la valeur décimale par un double. L'ajout suivant aboutirait à une valeur double.  
  
 Les valeurs atomiques non typées sont converties dans le type de base numérique de l’autre opérande, ou en **XS : double** si l’autre opérande est également non typé.  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Les arguments des opérateurs arithmétiques doivent être de type numérique ou **untypedAtomic**.  
  
-   Les opérations sur les valeurs **XS : Integer** produisent une valeur de type **XS : Decimal** au lieu de **XS : Integer**.  
  
  

---
description: = (Opérateur d’assignation) (Transact-SQL)
title: = (Opérateur d’assignation) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], assignment
- assignment operators [Transact-SQL]
- headings [SQL Server columns]
- relationships [SQL Server], assignment operators
- column headings [SQL Server]
ms.assetid: c3040db6-21d6-40ac-a783-82c98ec006cc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9b68e1d58926751166820dfe1d2c697efa6d6f7e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100926"
---
# <a name="-assignment-operator-transact-sql"></a>= (Opérateur d’assignation) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all_md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Le signe égal (=) est le seul opérateur d'affectation de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dans l'exemple qui suit, la variable `@MyCounter` est créée, puis l'opérateur d'affectation attribue à `@MyCounter` une valeur renvoyée par une expression.  
  
```sql  
DECLARE @MyCounter INT;  
SET @MyCounter = 1;  
```  
  
 L'opérateur d'affectation peut également être utilisé pour définir la relation entre l'en-tête d'une colonne et l'expression définissant les valeurs de la colonne. L'exemple suivant affiche les en-têtes de colonne `FirstColumnHeading` et `SecondColumnHeading`. La chaîne `xyz` s'affiche dans l'en-tête de colonne `FirstColumnHeading` pour toutes les lignes. Ensuite, chaque ID de produit extrait de la table `Product` est présenté dans l'en-tête de colonne `SecondColumnHeading`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstColumnHeading = 'xyz',  
       SecondColumnHeading = ProductID  
FROM Production.Product;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  

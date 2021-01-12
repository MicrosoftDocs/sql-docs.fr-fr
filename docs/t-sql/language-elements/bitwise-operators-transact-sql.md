---
description: Opérateurs au niveau du bit (Transact-SQL)
title: Opérateurs au niveau du bit (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], bitwise
- bitwise operators
- bit manipulations
ms.assetid: 2b994cf5-2daa-438a-b8c7-4bd8d451ac8d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e41fff219addb547d9706819a9fd5d9327f24e36
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100954"
---
# <a name="bitwise-operators-transact-sql"></a>Opérateurs au niveau du bit (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les opérateurs au niveau du bit exécutent des manipulations de bits entre deux expressions de tout type de données de la catégorie entier.  
  Les opérateurs au niveau du bit permettent de convertir deux valeurs entières en bits binaires, d’effectuer l’opération AND, OR, ou NOT sur chaque bit et de produire un résultat. Ils convertissent ensuite le résultat en entier.  
  
  Par exemple, l’entier 170 se convertit en binaire 1010 1010.
L’entier 75 se convertit en binaire 0100 1011.

|operator|mathématiques au niveau du bit|
|---- |---- |
|AND <br> Si les bits, quel que soit leur emplacement, égalent tous les deux 1, le résultat est 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 0000 1010 = 10 |
|OU <br> Si l’un ou l’autre des bits, quel que soit son emplacement, égale 1, le résultat est 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 1110 1011 = 235|
|NOT  <br> Inverse la valeur de bit à chaque emplacement de bit. |1010 1010 = 170 <br>----------------- <br>  0101 0101 = 85 |
  
Consultez les rubriques suivantes :   
* [& &#40;Opérateur AND au niveau du bit&#41;](../../t-sql/language-elements/bitwise-and-transact-sql.md)  
* [& = &#40;Affectation après AND au niveau du bit&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
* [&#124; &#40;Opérateur OR au niveau du bit&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md)  
* [&#124;= &#40;Affectation après OR au niveau du bit&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
* [^ &#40;Opérateur OR exclusif au niveau du bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)  
* [^= &#40;Affectation après OR exclusif au niveau du bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)  
* [~ &#40;Opérateur NOT au niveau du bit&#41;](../../t-sql/language-elements/bitwise-not-transact-sql.md)  
  
 Les opérandes des opérateurs au niveau du bit peuvent être de tout type de données des catégories chaîne binaire ou entier (à l’exception du type de données **image**), mais les deux opérandes ne peuvent être de n’importe quel type de données de la catégorie chaîne binaire. Ce tableau indique les types de données d'opérandes pris en charge.  
  
|Opérande de gauche|Opérande de droite|  
|------------------|-------------------|  
|[binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** ou **tinyint**|  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|**int**, **smallint**, **tinyint** ou **bit**|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**bigint**, **int**, **smallint**, **tinyint**, **binary** ou **varbinary**|  
|[int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** ou **varbinary**|  
|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** ou **varbinary**|  
|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** ou **varbinary**|  
|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** ou **tinyint**|  
  
## <a name="see-also"></a>Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)

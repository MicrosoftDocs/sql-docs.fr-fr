---
description: ^ (OR exclusif au niveau du bit) (Transact-SQL)
title: ^= (Opérateur OR exclusif au niveau du bit) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ^_TSQL
- Bitwise Exclusive OR
- Exclusive
- ^
- bitwise
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- ^ (bitwise exclusive OR operator)
- OR operator
- exclusive OR mathematical operations
- bitwise exclusive OR (^)
ms.assetid: f38f0ad4-46d0-40ea-9851-0f928fda5293
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d26cc856791bf46ec8a7f77fe2261ab41b635f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093549"
---
# <a name="-bitwise-exclusive-or-transact-sql"></a>^ (OR exclusif au niveau du bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Effectue une opération OR exclusive au niveau du bit entre deux valeurs entières.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
expression ^ expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de tout type de données de la catégorie entier, ou de type **bit**, **binary** ou **varbinary**. L’*expression* est traitée comme un nombre binaire pour l’opération au niveau du bit.  
  
> [!NOTE]  
>  Une seule *expression* peut être de type **binary** ou **varbinary** dans une opération au niveau du bit.  
  
## <a name="result-types"></a>Types des résultats  
 **int** si les valeurs d’entrée sont **int**.  
  
 **smallint** si les valeurs d’entrée sont **smallint**.  
  
 **tinyint** si les valeurs d’entrée sont **tinyint**.  
  
## <a name="remarks"></a>Notes  
 L’opérateur au niveau du bit **^** effectue une opération OR logique au niveau du bit entre les deux expressions, évaluant chaque bit correspondant dans les deux expressions. Dans le résultat, les bits prennent la valeur 1 si l'un ou l'autre (mais pas les deux) des bits correspondants (pour la position en cours d'évaluation) dans les deux expressions d'entrées ont une valeur de 1 Si les deux bits ont pour valeur 0 ou 1, le bit du résultat prend la valeur 0.  
  
 Si les expressions de droite et de gauche ont des types de données integer (entier) différents (par exemple, l’*expression* de gauche est de type **smallint** et l’*expression* de droite est de type **int**), l’argument du type de données le plus petit est converti dans le type de données plus grand. Dans cet exemple, l’**expression** _smallint_ est convertie en un **int**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une table avec le type de données **int** pour stocker les valeurs d’origine et insère deux valeurs dans une ligne.  
  
```sql  
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 La requête suivante effectue l'opération OR exclusif au niveau du bit sur les colonnes `a_int_value` et `b_int_value`.  
  
```sql  
SELECT a_int_value ^ b_int_value  
FROM bitwise;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
-----------   
225           
  
(1 row(s) affected)  
```  
  
 La représentation binaire de 170 (`a_int_value` ou `A`) est `0000 0000 1010 1010`. La représentation binaire de 75 (`b_int_value` ou `B`) est `0000 0000 0100 1011`. L'exécution de l'opération OR exclusif au niveau du bit sur ces deux valeurs donne le résultat binaire `0000 0000 1110 0001`, soit 225 en notation décimale.  
  
```  
(A ^ B)     
         0000 0000 1010 1010  
         0000 0000 0100 1011  
         -------------------  
         0000 0000 1110 0001  
```  
  

  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [^= &#40;Affectation après OR exclusif au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  



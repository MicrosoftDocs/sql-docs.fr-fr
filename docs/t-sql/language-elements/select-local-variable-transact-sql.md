---
description: SELECT @local_variable (Transact-SQL)
title: SELECT @local_variable (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- variable_TSQL
- '@loca_variable'
- '@local'
- variable
- '@loca_variable_TSQL'
- '@local_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- variables [SQL Server], assigning
- SELECT statement [SQL Server], @local_variable
- '@local_variable'
- local variables [SQL Server]
ms.assetid: 8e1a9387-2c5d-4e51-a1fd-a2a95f026d6f
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||= azure-sqldw-latest||>= sql-server-linux-2017
ms.openlocfilehash: 3b1545d50733a5139c0a4d6ef22f22b719691fc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100323"
---
# <a name="select-local_variable-transact-sql"></a>SELECT @local_variable (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Affecte à une variable locale la valeur d’une expression.  
  
 Pour affecter des valeurs aux variables, il est recommandé d’utiliser [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md) à la place de SELECT @*local_variable*.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SELECT { @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression } 
    [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

@*local_variable*  
 Variable déclarée à laquelle doit être assignée une valeur.  
  
{= \| += \| -= \| \*= \| /= \| %= \| &= \| ^= \| \|= }  
Assignez la valeur située à droite à la variable située à gauche.  
  
Opérateur d'assignation composé :  

| operator | action |  
| -------- | ------ |  
| = | Affecte à la variable l’expression qui suit. |  
| += | Additionner et assigner |  
| -= | Soustraire et assigner |  
| \*= | Multiplier et assigner |  
| /= | Diviser et assigner |  
| %= | Modulo et assigner |  
| &= | AND au niveau du bit et assigner |  
| ^= | XOR au niveau du bit et assigner |  
| \|= | OR au niveau du bit et assigner |  

*expression*  
Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide. Cela comprend une sous-requête scalaire.  

## <a name="remarks"></a>Notes

L’instruction SELECT @*local_variable* est généralement utilisée pour retourner une valeur unique vers la variable. Toutefois, quand *expression* correspond au nom d’une colonne, plusieurs valeurs peuvent être retournées. Si l'instruction SELECT retourne plusieurs valeurs, la dernière valeur retournée est affectée à la variable.  

Si l'instruction SELECT ne retourne aucune ligne, la variable conserve sa valeur actuelle. Si *expression* est une sous-requête scalaire qui ne retourne aucune valeur, la valeur affectée à la variable est NULL.  

Une instruction SELECT peut initialiser plusieurs variables locales.  

> [!NOTE]
> Une instruction SELECT contenant une affectation de variable ne peut pas être utilisée pour effectuer également des opérations d'extraction habituelles dans les jeux de résultats.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-use-select-local_variable-to-return-a-single-value"></a>R. Utiliser SELECT @local_variable pour retourner une valeur unique  
 Dans l'exemple suivant, la variable `@var1` reçoit la valeur `Generic Name`. La requête sur la table `Store` ne retourne aucune ligne car la valeur spécifiée pour `CustomerID` n'existe pas dans la table. La variable conserve la valeur `Generic Name`.  
  
```sql  
-- Uses AdventureWorks    
  
DECLARE @var1 VARCHAR(30);         
SELECT @var1 = 'Generic Name';         
SELECT @var1 = Name         
FROM Sales.Store         
WHERE CustomerID = 1000 ;        
SELECT @var1 AS 'Company Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Company Name  
 ------------------------------  
 Generic Name  
 ```  
  
### <a name="b-use-select-local_variable-to-return-null"></a>B. Utiliser SELECT @local_variable pour retourner une valeur Null  
 Dans l'exemple suivant, une sous-requête est utilisée pour affecter une valeur à `@var1`. Comme la valeur demandée pour `CustomerID` n'existe pas, la sous-requête ne retourne pas de valeur et la variable se voit affecter la valeur `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @var1 VARCHAR(30)   
SELECT @var1 = 'Generic Name'   
SELECT @var1 = (SELECT Name   
FROM Sales.Store   
WHERE CustomerID = 1000)   
SELECT @var1 AS 'Company Name' ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Company Name  
----------------------------  
NULL  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  

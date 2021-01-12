---
description: NULLIF (Transact-SQL)
title: NULLIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NULLIF
- NULLIF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], equivalent expressions
- expressions [SQL Server], null values
- NULLIF function
- equivalent expressions [SQL Server]
ms.assetid: 44c7b67e-74c7-4bb9-93a4-7a3016bd2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bcf87ce6ab8dae4bcf509a204d486e9a809eedd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093539"
---
# <a name="nullif-transact-sql"></a>NULLIF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une valeur NULL si les deux expressions spécifiées sont égales. Par exemple, `SELECT NULLIF(4,4) AS Same, NULLIF(5,7) AS Different;` retourne la valeur NULL pour la première colonne (4 et 4) car les deux valeurs d’entrée sont identiques. La deuxième colonne retourne la première valeur (5) car les deux valeurs d’entrée sont différentes. 
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
NULLIF ( expression , expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) scalaire valide.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Retourne le même type que la première *expression*.  
  
 NULLIF retourne la première *expression* si les deux expressions ne sont pas égales. Si elles sont égales, NULLIF retourne une valeur Null du type de la première *expression*.  
  
## <a name="remarks"></a>Remarques  
 NULLIF est équivalent à l'exécution d'une expression CASE dans laquelle les deux expressions sont identiques et l'expression résultante est NULL.  
  
 Il est recommandé de ne pas utiliser de fonction dépendant du temps, telle que RAND(), dans une fonction NULLIF. Ceci pourrait entraîner une évaluation à deux reprises de la fonction, et des résultats différents pourraient être retournés à partir des deux appels.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-budget-amounts-that-have-not-changed"></a>R. Retour des montants de budget qui n'ont pas changé  
 L'exemple suivant crée une table `budgets` où sont répertoriés un service (`dept`), son budget pour l'année en cours (`current_year`) et son budget pour l'année précédente (`previous_year`). Pour l'année en cours, `NULL` est utilisé pour les services dont les budgets n'ont pas été modifiés par rapport à l'année précédente et `0` est utilisé pour les budgets qui n'ont pas été déterminés. Pour obtenir la moyenne des seuls services auxquels un budget a été attribué et pour inclure la valeur budgétaire de l'année précédente (utilisez la valeur `previous_year` dans les cas où la valeur pour `current_year` est `NULL`), combinez les fonctions `NULLIF` et `COALESCE`.  
  
```sql  
CREATE TABLE dbo.budgets  
(  
   dept            TINYINT   IDENTITY,  
   current_year    DECIMAL   NULL,  
   previous_year   DECIMAL   NULL  
);  
INSERT budgets VALUES(100000, 150000);  
INSERT budgets VALUES(NULL, 300000);  
INSERT budgets VALUES(0, 100000);  
INSERT budgets VALUES(NULL, 150000);  
INSERT budgets VALUES(300000, 250000);  
GO    
SET NOCOUNT OFF;  
SELECT AVG(NULLIF(COALESCE(current_year,  
   previous_year), 0.00)) AS 'Average Budget'  
FROM budgets;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Average Budget  
 --------------  
 212500.000000  
 (1 row(s) affected)
 ```  
  
### <a name="b-comparing-nullif-and-case"></a>B. Comparaison de NULLIF et CASE  
 Pour montrer la similarité entre `NULLIF` et `CASE`, les requêtes suivantes déterminent si les valeurs des colonnes `MakeFlag` et `FinishedGoodsFlag` sont les mêmes. La première requête utilise `NULLIF`. La seconde requête utilise l'expression `CASE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,   
   NULLIF(MakeFlag,FinishedGoodsFlag) AS 'Null if Equal'  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,'Null if Equal' =  
   CASE  
       WHEN MakeFlag = FinishedGoodsFlag THEN NULL  
       ELSE MakeFlag  
   END  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
```  

### <a name="c-returning-budget-amounts-that-contain-no-data"></a>C. Retour des montants de budget qui ne contiennent aucune donnée  
 L’exemple suivant crée une table `budgets`, charge les données et utilise `NULLIF` pour retourner une valeur Null si `current_year` et `previous_year` ne contiennent aucune donnée.  
  
```sql  
CREATE TABLE budgets (  
   dept           TINYINT,  
   current_year   DECIMAL(10,2),  
   previous_year  DECIMAL(10,2)  
);  
  
INSERT INTO budgets VALUES(1, 100000, 150000);  
INSERT INTO budgets VALUES(2, NULL, 300000);  
INSERT INTO budgets VALUES(3, 0, 100000);  
INSERT INTO budgets VALUES(4, NULL, 150000);  
INSERT INTO budgets VALUES(5, 300000, 300000);  
  
SELECT dept, NULLIF(current_year,  
   previous_year) AS LastBudget  
FROM budgets;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 dept   LastBudget  
 ----   -----------  
 1      100000.00  
 2      null 
 3      0.00  
 4      null  
 5      null
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  


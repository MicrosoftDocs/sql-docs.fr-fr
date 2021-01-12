---
description: BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d41c356b20dd0bb0577f10ee0abc47681ed38e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096009"
---
# <a name="between-transact-sql"></a>BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Définit un intervalle sur lequel la recherche doit porter.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *test_expression*  
 Correspond à l’[expression](../../t-sql/language-elements/expressions-transact-sql.md) à tester dans la plage définie par *begin_expression* et *end_expression*. *test_expression* doit être du même type de données que *begin_expression* et *end_expression*.  
  
 NOT  
 Indique la négation du résultat du prédicat.  
  
 *begin_expression*  
 Peut être toute expression valide. *begin_expression* doit être du même type de données que *test_expression* et *end_expression*.  
  
 *end_expression*  
 Peut être toute expression valide. *end_expression* doit être du même type de données que *test_expression* et *begin_expression*.  
  
 AND  
 Agit en tant qu’espace réservé qui indique que *test_expression* doit se trouver dans la plage indiquée par *begin_expression* et *end_expression*.  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-value"></a>Valeur des résultats  
 BETWEEN retourne **TRUE** si la valeur de *test_expression* est supérieure ou égale à la valeur de *begin_expression* et inférieure ou égale à la valeur de *end_expression*.  
  
 NOT BETWEEN retourne **TRUE** si la valeur de *test_expression* est inférieure à la valeur de *begin_expression* ou supérieure à la valeur de *end_expression*.  
  
## <a name="remarks"></a>Remarques  
 Pour spécifier un intervalle exclusif, employez les opérateurs « supérieur à » (>) et « inférieur à » (<). Si l'un des paramètres fournis avec le prédicat BETWEEN ou NOT BETWEEN a la valeur NULL, le résultat est UNKNOWN.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-between"></a>R. Utilisation de BETWEEN  
 L’exemple suivant retourne des informations sur les rôles de base de données dans une base de données. La première requête retourne tous les rôles. Le deuxième exemple utilise la clause `BETWEEN` pour limiter les rôles aux valeurs `database_id` spécifiées.  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>B. Utilisation de > et de < à la place de BETWEEN  
 L'exemple suivant utilise les opérateurs « supérieur à » (`>`) et « inférieur à » (`<`) ; ceux-ci n'étant pas inclusifs, il retourne neuf lignes au lieu des dix retournées dans l'exemple précédent.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>C. Utilisation de NOT BETWEEN  
 L'exemple suivant recherche toutes les lignes situées en dehors d'une plage spécifiée allant de `27` à `30`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>D. Utilisation de BETWEEN avec des valeurs datetime  
 L’exemple suivant récupère les lignes dans lesquelles les valeurs **datetime** sont situées entre `'20011212'` et `'20020105'` incluses.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 La requête récupère les lignes attendues car les valeurs de date dans la requête et les valeurs **datetime** stockées dans la colonne `RateChangeDate` ont été spécifiées sans la partie heure de la date. Lorsque la partie heure n'est pas spécifiée, la valeur par défaut est 12:00 A.M. Notez qu'une ligne qui contient une heure ultérieure à 12h00 le 5 janvier 2002 ne serait pas retournée par cette requête car cette date se situe hors de la plage.  
  
  
## <a name="see-also"></a>Voir aussi  
 [&#62; &#40;Supérieur à&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60; &#40;Inférieur à&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/less-than-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  



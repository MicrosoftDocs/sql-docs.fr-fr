---
description: Fonctions logiques - CHOOSE (Transact-SQL)
title: CHOOSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHOOSE
- CHOOSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHOOSE function
ms.assetid: 1c382c83-7500-4bae-bbdc-c1dbebd3d83f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6aad317f1b8677b2c9d1c5de1013df7beb303d7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208839"
---
# <a name="logical-functions---choose-transact-sql"></a>Fonctions logiques - CHOOSE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne l'élément à l'index spécifié à partir d'une liste de valeurs dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CHOOSE ( index, val_1, val_2 [, val_n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *index*  
 Expression entière qui représente un index de base 1 dans la liste des éléments qui suit.  
  
 Si la valeur d’index fournie a un type de données numérique autre que **int**, la valeur est implicitement convertie en un entier. Si la valeur d'index dépasse les limites du tableau de valeurs, CHOOSE renvoie la valeur null.  
  
 *val_1 ... val_n*  
 Liste de valeurs séparées par des virgules de tous les types de données.  
  
## <a name="return-types"></a>Types de retour  
 Retourne le type de données ayant la priorité la plus élevée à partir de l'ensemble de types transmis à la fonction. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Remarques  
 CHOOSE agit comme un index dans un tableau, où le tableau est composé des arguments qui suivent l'argument d'index. L'argument d'index détermine, parmi les valeurs suivantes, celle qui sera retournée.  
  
## <a name="examples"></a>Exemples  

### <a name="a-simple-choose-example"></a>R. Exemple CHOOSE simple

 L'exemple suivant retourne le troisième élément de la liste de valeurs fournie.  
 
```sql 
SELECT CHOOSE ( 3, 'Manager', 'Director', 'Developer', 'Tester' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
-------------  
Developer  
  
(1 row(s) affected)  
```  

### <a name="b-simple-choose-example-based-on-column"></a>B. Exemple CHOOSE simple basé sur la colonne

 L'exemple suivant retourne une chaîne de caractères simple en fonction de la valeur de la colonne `ProductCategoryID`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductCategoryID, CHOOSE (ProductCategoryID, 'A','B','C','D','E') AS Expression1  
FROM Production.ProductCategory;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductCategoryID Expression1  
----------------- -----------  
3                 C  
1                 A  
2                 B  
4                 D  
  
(4 row(s) affected)  
  
```  

### <a name="c-choose-in-combination-with-month"></a>C. CHOOSE en association avec MONTH
  
 L'exemple suivant retourne la saison au cours de laquelle l'employé a été embauché. La fonction MONTH est utilisée pour retourner la valeur de mois de la colonne `HireDate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT JobTitle, HireDate, CHOOSE(MONTH(HireDate),'Winter','Winter', 'Spring','Spring','Spring','Summer','Summer',   
                                                  'Summer','Autumn','Autumn','Autumn','Winter') AS Quarter_Hired  
FROM HumanResources.Employee  
WHERE  YEAR(HireDate) > 2005  
ORDER BY YEAR(HireDate);  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
JobTitle                                           HireDate   Quarter_Hired  
-------------------------------------------------- ---------- -------------  
Sales Representative                               2006-11-01 Autumn  
European Sales Manager                             2006-05-18 Spring  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2007-07-01 Summer  
Pacific Sales Manager                              2007-04-15 Spring  
Sales Representative                               2007-07-01 Summer  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)  
  
  

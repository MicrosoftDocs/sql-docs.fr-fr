---
description: GROUPING (Transact-SQL)
title: GROUPING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GROUPING
- GROUPING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], GROUPING function
- grouping columns
- ROLLUP operator
- GROUP BY clause, GROUPING function
- GROUPING function
- CUBE operator
ms.assetid: 4efa3868-1fc4-4626-8fb1-e863cc03e422
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9ced8a83cff51304050b71373a3272cdc2a80af6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100449"
---
# <a name="grouping-transact-sql"></a>GROUPING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Indique si une expression de colonne spécifiée dans une liste GROUP BY est agrégée ou non. GROUPING retourne 1 pour agrégé ou 0 pour non agrégé dans le jeu de résultats. GROUPING ne peut être utilisé que dans les clauses SELECT \<select>, HAVING et ORDER BY lorsque GROUP BY est spécifié.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GROUPING ( <column_expression> )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 \<column_expression>  
 Colonne ou expression qui contient une colonne dans une clause [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-types"></a>Types de retour  
 **tinyint**  
  
## <a name="remarks"></a>Notes  
 GROUPING sert à distinguer les valeurs NULL retournées par CUBE, ROLLUP ou GROUPING SETS des valeurs NULL standard. La valeur NULL retournée comme résultat d'une opération CUBE, ROLLUP ou GROUPING SETS est une utilisation spéciale de NULL. Elle agit comme espace réservé de colonne dans le jeu de résultats et signifie « All » (tout).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant groupe `SalesQuota` et agrège les montants `SaleYTD` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La fonction `GROUPING` est appliquée à la colonne `SalesQuota`.  
  
```sql 
SELECT SalesQuota, SUM(SalesYTD) 'TotalSalesYTD', GROUPING(SalesQuota) AS 'Grouping'  
FROM Sales.SalesPerson  
GROUP BY SalesQuota WITH ROLLUP;  
GO  
```  
  
 Le jeu de résultats indique deux valeurs NULL sous `SalesQuota`. La première valeur `NULL` représente le groupe des valeurs NULL de cette colonne dans la table. La seconde valeur `NULL` se trouve dans la ligne résumée ajoutée par l'opération ROLLUP. La ligne du total indique les montants `TotalSalesYTD` pour tous les groupes `SalesQuota` et est indiquée par `1` dans la colonne `Grouping`.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 SalesQuota     TotalSalesYTD       Grouping  
------------   -----------------   --------  
NULL           1533087.5999          0  
250000.00      33461260.59           0  
300000.00      9299677.9445          0  
NULL           44294026.1344         1  

(4 row(s) affected)
```  
  
## <a name="see-also"></a>Voir aussi  
 [GROUPING_ID &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-id-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  

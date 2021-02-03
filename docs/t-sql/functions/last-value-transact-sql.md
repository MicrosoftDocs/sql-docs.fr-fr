---
description: LAST_VALUE (Transact-SQL)
title: LAST_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LAST_VALUE
- LAST_VALUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, LAST_VALUE
- LAST_VALUE function
ms.assetid: fd833e34-8092-42b7-80fc-95ca6b0eab6b
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bc1a921244b7e4a6f19f95ebbb626f19265872fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202128"
---
# <a name="last_value-transact-sql"></a>LAST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Retourne la dernière valeur dans un jeu de valeurs ordonné.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  

```syntaxsql 
LAST_VALUE ( [ scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause rows_range_clause )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *scalar_expression*  
 Valeur à renvoyer. *scalar_expression* peut être une colonne, une sous-requête ou une autre expression qui aboutit à une valeur unique. Les autres fonctions analytiques ne sont pas autorisées.  
  
 [ IGNORER NULLS | RESPECTER NULLS ]     
 **S’applique à** : Azure SQL Edge

 IGNORER NULLS : Ignore les valeurs null dans le jeu de données lors du calcul de la dernière valeur sur une partition.     
 RESPECTER NULLS : Respecte les valeurs null dans le jeu de données lors du calcul de la dernière valeur sur une partition.     
 
  Pour plus d’informations, consultez [Imputation de valeurs manquantes](/azure/azure-sql-edge/imputing-missing-values/).
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 *partition_by_clause* divise le jeu de résultats généré par la clause FROM en partitions auxquelles la fonction est appliquée. S'il n'est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en un seul groupe.  
  
 *order_by_clause* détermine l’ordre des données avant que la fonction soit appliquée. *order_by_clause* est requis. *rows_range_clause* limite davantage les lignes dans la partition en spécifiant les points de départ et de terminaison. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Types de retour  
 Type similaire à celui de *scalar_expression*.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 LAST_VALUE n'est pas déterministe. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-last_value-over-partitions"></a>R. Utilisation de LAST_VALUE sur les partitions  
 L'exemple suivant retourne la date d'embauche du dernier employé dans chaque service pour le salaire donné (taux). La clause PARTITION BY décompose les employés par service et la fonction LAST_VALUE est appliquée à chaque partition indépendamment. La clause ORDER BY spécifiée dans la clause OVER détermine l'ordre logique dans lequel la fonction LAST_VALUE est appliquée aux lignes dans chaque partition.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate, HireDate,   
    LAST_VALUE(HireDate) OVER (PARTITION BY Department ORDER BY Rate) AS LastValue  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
INNER JOIN HumanResources.EmployeePayHistory AS eph    
    ON eph.BusinessEntityID = edh.BusinessEntityID  
INNER JOIN HumanResources.Employee AS e  
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control');   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Department                  LastName                Rate         HireDate     LastValue  
--------------------------- ----------------------- ------------ ----------   ----------  
Document Control            Chai                    10.25        2003-02-23   2003-03-13  
Document Control            Berge                   10.25        2003-03-13   2003-03-13  
Document Control            Norred                  16.8269      2003-04-07   2003-01-17  
Document Control            Kharatishvili           16.8269      2003-01-17   2003-01-17  
Document Control            Arifin                  17.7885      2003-02-05   2003-02-05  
Information Services        Berg                    27.4038      2003-03-20   2003-01-24  
Information Services        Meyyappan               27.4038      2003-03-07   2003-01-24  
Information Services        Bacon                   27.4038      2003-02-12   2003-01-24  
Information Services        Bueno                   27.4038      2003-01-24   2003-01-24  
Information Services        Sharma                  32.4519      2003-01-05   2003-03-27  
Information Services        Connelly                32.4519      2003-03-27   2003-03-27  
Information Services        Ajenstat                38.4615      2003-02-18   2003-02-23  
Information Services        Wilson                  38.4615      2003-02-23   2003-02-23  
Information Services        Conroy                  39.6635      2003-03-08   2003-03-08  
Information Services        Trenary                 50.4808      2003-01-12   2003-01-12  
  
```  
  
### <a name="b-using-first_value-and-last_value-in-a-computed-expression"></a>B. Utilisation de FIRST_VALUE et LAST_VALUE dans une expression calculée  
 L'exemple suivant utilise les fonctions FIRST_VALUE et LAST_VALUE dans des expressions calculées pour afficher la différence entre la valeur de quota de ventes pour le trimestre actuel et, respectivement, le premier trimestre et le dernier trimestre de l'année pour un nombre donné d'employés. La fonction FIRST_VALUE retourne la valeur du quota de ventes pour le premier trimestre de l'année et la soustrait de la valeur du quota de ventes du trimestre en cours. Elle est retournée dans la colonne dérivée intitulée DifferenceFromFirstQuarter. Pour le premier trimestre d'une année, la valeur de la colonne DifferenceFromFirstQuarter est 0. La fonction LAST_VALUE retourne la valeur de quota de ventes pour le dernier trimestre de l'année et la soustrait de la valeur actuelle de quota de ventes du trimestre actuel. Elle est retournée dans la colonne dérivée intitulée DifferenceFromLastQuarter. Pour le dernier trimestre de l'année, la valeur de la colonne DifferenceFromLastQuarter est 0.  
  
 La clause « RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING » est obligatoire dans cet exemple pour les valeurs non nulles à retourner dans la colonne DifferenceFromLastQuarter, comme illustré ci-dessous. La plage par défaut est « RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW ». Dans cet exemple, l'utilisation de la plage par défaut (ou le fait de ne pas inclure de plage, ce qui entraînerait l'utilisation de la plage par défaut) provoquerait le renvoi de valeurs nulles dans la colonne DifferenceFromLastQuarter. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
SELECT BusinessEntityID, DATEPART(QUARTER,QuotaDate)AS Quarter, YEAR(QuotaDate) AS SalesYear,   
    SalesQuota AS QuotaThisQuarter,   
    SalesQuota - FIRST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate) ) AS DifferenceFromFirstQuarter,   
    SalesQuota - LAST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate)   
              RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING ) AS DifferenceFromLastQuarter   
FROM Sales.SalesPersonQuotaHistory   
WHERE YEAR(QuotaDate) > 2005   
AND BusinessEntityID BETWEEN 274 AND 275   
ORDER BY BusinessEntityID, SalesYear, Quarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Quarter     SalesYear   QuotaThisQuarter      DifferenceFromFirstQuarter DifferenceFromLastQuarter  
---------------- ----------- ----------- --------------------- --------------------------- -----------------------  
274              1           2006        91000.00              0.00                        -63000.00  
274              2           2006        140000.00             49000.00                    -14000.00  
274              3           2006        70000.00              -21000.00                   -84000.00  
274              4           2006        154000.00             63000.00                    0.00  
274              1           2007        107000.00             0.00                        -9000.00  
274              2           2007        58000.00              -49000.00                   -58000.00  
274              3           2007        263000.00             156000.00                   147000.00  
274              4           2007        116000.00             9000.00                     0.00  
274              1           2008        84000.00              0.00                        -103000.00  
274              2           2008        187000.00             103000.00                   0.00  
275              1           2006        502000.00             0.00                        -822000.00  
275              2           2006        550000.00             48000.00                    -774000.00  
275              3           2006        1429000.00            927000.00                   105000.00  
275              4           2006        1324000.00            822000.00                   0.00  
275              1           2007        729000.00             0.00                        -489000.00  
275              2           2007        1194000.00            465000.00                   -24000.00  
275              3           2007        1575000.00            846000.00                   357000.00  
275              4           2007        1218000.00            489000.00                   0.00  
275              1           2008        849000.00             0.00                        -20000.00  
275              2           2008        869000.00             20000.00                    0.00  
  
(20 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Voir aussi  

 [First_Value &#40;Transact-SQL&#41;](first-value-transact-sql.md)  

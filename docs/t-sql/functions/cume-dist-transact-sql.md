---
description: CUME_DIST (Transact-SQL)
title: CUME_DIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4252c237e234fdc380ca0f4b18cfcf55f6bf9ae4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093722"
---
# <a name="cume_dist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette fonction calcule la distribution cumulative d'une valeur dans un groupe de valeurs. Autrement dit, `CUME_DIST` calcule la position relative d'une valeur spécifiée dans un groupe de valeurs. En supposant un ordre croissant, le `CUME_DIST` d’une valeur à la ligne _r_ correspond au nombre de lignes avec des valeurs inférieures ou égales à la valeur de la ligne _r_, divisé par le nombre de lignes évaluées dans la partition ou le jeu de résultats de la requête. `CUME_DIST` est similaire à la fonction `PERCENT_RANK`.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_)  

_partition\_by\_clause_ divise le jeu de résultats généré par la clause FROM en partitions auxquelles la fonction est appliquée. Si l’argument _partition\_by\_clause_ n’est pas spécifié, `CUME_DIST` traite toutes les lignes du jeu de résultats de la requête comme un seul groupe. _order\_by\_clause_ détermine l’ordre logique dans lequel l’opération est effectuée. `CUME_DIST` nécessite _order\_by\_clause_. `CUME_DIST` n’accepte pas la \<rows or range clause> de la syntaxe OVER. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Types de retour
**float(53)**
  
## <a name="remarks"></a>Notes  
`CUME_DIST` retourne une plage de valeurs supérieures à 0, et inférieures ou égales à 1. Les valeurs égales sont toujours évaluées à la même valeur de distribution cumulative. `CUME_DIST` inclut les valeurs NULL par défaut et les traite comme les valeurs les plus basses possibles.
  
`CUME_DIST` n’est pas déterministe. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Exemples  
Cet exemple utilise la fonction `CUME_DIST` pour calculer le percentile de salaire pour chaque employé dans un service donné. `CUME_DIST` retourne une valeur qui représente le pourcentage d'employés dont le salaire est inférieur ou égal à celui de l'employé actuel dans le même service. La fonction `PERCENT_RANK` calcule le rang de pourcentage du salaire de l'employé dans un service. Pour partitionner les lignes du jeu de résultats par service, l’exemple spécifie la valeur _partition\_by\_clause_. La clause ORDER BY de la clause OVER ordonnance logiquement les lignes dans chaque partition. La clause ORDER BY de l'instruction SELECT détermine l'ordre d'affichage du jeu de résultats.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[PERCENT_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/percent-rank-transact-sql.md)
  
  

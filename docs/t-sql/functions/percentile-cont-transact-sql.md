---
description: PERCENTILE_CONT (Transact-SQL)
title: PERCENTILE_CONT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_CONT_TSQL
- PERCENTILE_CONT
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, PERCENTILE_CONT
- PERCENTILE_CONT function
ms.assetid: d019419e-5297-4994-97d5-e9c8fc61bbf4
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2e2ab7fa86b8888f105428847bdd751ac749bba5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186065"
---
# <a name="percentile_cont-transact-sql"></a>PERCENTILE_CONT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Calcule un percentile basé sur une distribution continue de la valeur de colonne dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le résultat est interpolé et peut ne pas être égale aux valeurs spécifiques de la colonne.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
PERCENTILE_CONT ( numeric_literal )   
    WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *numeric_literal*  
 Percentile à calculer. Il doit être compris entre 0.0 et 1.0.  
  
 WITHIN GROUP **(** ORDER BY *order_by_expression* [ **ASC** | DESC ] **)**  
 Spécifie une liste de valeurs numériques à trier et sur lesquelles calculer le percentile. Un seul argument *order_by_expression* est autorisé. L’expression doit correspondre à un type numérique exact ou approximatif, sans aucun autre type de données autorisé. Les types numériques exacts sont **int**, **bigint**, **smallint**, **tinyint**, **numeric**, **bit**, **decimal**, **smallmoney** et **money**. Les types numériques approximatifs sont **float** et **real**. L’ordre de tri par défaut est croissant.  
  
 OVER **(** \<partition_by_clause> **)**  
 Divise le jeu de résultats généré par la clause FROM en partitions auxquelles la fonction percentile est appliquée. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). La \<ORDER BY clause> et la \<rows or range clause> de la syntaxe OVER ne peuvent pas être spécifiées dans une fonction PERCENTILE_CONT.  
  
## <a name="return-types"></a>Types de retour  
 **float(53)**  
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité  
 Lorsque le niveau de compatibilité est 110 et supérieur, WITHIN GROUP est un mot clé réservé. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Toutes les valeurs NULL dans le jeu de données sont ignorées.  
  
 PERCENTILE_CONT n'est pas déterministe. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-basic-syntax-example"></a>R. Exemple de syntaxe de base  
 L'exemple suivant utilise PERCENTILE_CONT et PERCENTILE_DISC pour rechercher le salaire moyen d'un employé dans chaque service. Ces fonctions peuvent ne pas retourner la même valeur. PERCENTILE_CONT interpole la valeur appropriée, qui peut exister ou non dans le jeu de données, alors que PERCENTILE_DISC retourne toujours une valeur réelle à partir du jeu.  
  
```sql  
USE AdventureWorks2012;  
  
SELECT DISTINCT Name AS DepartmentName  
      ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianCont  
      ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianDisc  
FROM HumanResources.Department AS d  
INNER JOIN HumanResources.EmployeeDepartmentHistory AS dh   
    ON dh.DepartmentID = d.DepartmentID  
INNER JOIN HumanResources.EmployeePayHistory AS ph  
    ON ph.BusinessEntityID = dh.BusinessEntityID  
WHERE dh.EndDate IS NULL;  
```  
  
 Voici un jeu de résultats partiel.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.8269      16.8269
Engineering            34.375       32.6923
Executive              54.32695     48.5577
Human Resources        17.427850    16.5865
```  

### <a name="b-basic-syntax-example"></a>B. Exemple de syntaxe de base  
 L'exemple suivant utilise PERCENTILE_CONT et PERCENTILE_DISC pour rechercher le salaire moyen d'un employé dans chaque service. Ces fonctions peuvent ne pas retourner la même valeur. PERCENTILE_CONT interpole la valeur appropriée, qui peut exister ou non dans le jeu de données, alors que PERCENTILE_DISC retourne toujours une valeur réelle à partir du jeu.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT DepartmentName  
,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianCont  
,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianDisc  
FROM dbo.DimEmployee; 
```  
  
 Voici un jeu de résultats partiel.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.826900    16.8269
Engineering            34.375000    32.6923
Human Resources        17.427850    16.5865
Shipping and Receiving 9.250000      9.0000
```  
  
## <a name="see-also"></a>Voir aussi  
 [PERCENTILE_DISC &#40;Transact-SQL&#41;](../../t-sql/functions/percentile-disc-transact-sql.md)  
  
 

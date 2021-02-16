---
description: APPROX_COUNT_DISTINCT (Transact-SQL)
title: APPROX_COUNT_DISTINCT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPROX_COUNT_DISTINCT
dev_langs:
- TSQL
author: joesackmsft
ms.author: josack
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c041986f79779c5eeeeb6a1a11b7f77abfe5eebe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270652"
---
# <a name="approx_count_distinct-transact-sql"></a>APPROX_COUNT_DISTINCT (Transact-SQL)

[!INCLUDE [sqlserver2019-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi-asa.md)]

Cette fonction retourne le nombre approximatif de valeurs non NULL uniques dans un groupe. 
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
APPROX_COUNT_DISTINCT ( expression )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type, sauf **image**, **sql_variant**, **ntext**, or **text**. 

## <a name="return-types"></a>Types de retour
 **bigint**  
  
## <a name="remarks"></a>Notes  
`APPROX_COUNT_DISTINCT( expression )`Évalue une expression pour chaque ligne d’un groupe et renvoie le nombre de valeurs non NULL uniques dans un groupe. Cette fonction est conçue pour fournir des agrégations dans de vastes jeux de données où la réactivité est plus importante que la précision absolue.  

`APPROX_COUNT_DISTINCT` est conçu pour une utilisation dans les scénarios big data et optimisé pour les conditions suivantes :
- Accès à des jeux de données comportant des millions de lignes ou plus *et*
- Agrégation d’une ou de plusieurs colonnes qui ont de nombreuses valeurs distinctes

L’implémentation de la fonction garantit un taux d’erreur pouvant atteindre 2 % avec une probabilité de 97 %. 

`APPROX_COUNT_DISTINCT` nécessite moins de mémoire qu’une opération COUNT DISTINCT exhaustive.  Étant donné l’encombrement mémoire plus faible, `APPROX_COUNT_DISTINCT` est moins susceptible de propager la mémoire dans le disque qu’une opération COUNT DISTINCT précise. Pour en savoir plus sur l’algorithme utilisé pour y parvenir, consultez [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog).

> [!NOTE]
> Avec des chaînes sensibles au classement, APPROX_COUNT_DISTINCT utilise une correspondance binaire et fournit des résultats qui auraient été générés en présence de classements BIN, mais pas BIN2. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-approx_count_distinct"></a>R. Utilisation d’APPROX_COUNT_DISTINCT 
Cet exemple retourne le nombre approximatif de clés d’ordre différent de la table d’ordres.
  
```sql
SELECT APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders;
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Approx_Distinct_OrderKey
------------------------
15164704
```
  
### <a name="b-using-approx_count_distinct-with-group-by"></a>B. Utilisation d’APPROX_COUNT_DISTINCT avec GROUPER PAR 
Cet exemple retourne le nombre approximatif de clés d’ordre différent par état d’ordre de la table d’ordres. 
  
```sql
SELECT O_OrderStatus, APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders
GROUP BY O_OrderStatus
ORDER BY O_OrderStatus; 
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
O_OrderStatus                                                    Approx_Distinct_OrderKey
---------------------------------------------------------------- ------------------------
F                                                                7397838
O                                                                7387803
P                                                                388036
```
    
## <a name="see-also"></a>Voir aussi
[Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md) 

---
description: Opérateurs de jeu - EXCEPT et INTERSECT (Transact-SQL)
title: EXCEPT et INTERSECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0a7ddf707ec6ed5bea3ff32fee215e29497767a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100315"
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>Opérateurs de jeu - EXCEPT et INTERSECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne des lignes distinctes en comparant les résultats de deux requêtes.  
  
EXCEPT retourne les lignes distinctes de la requête d’entrée à gauche mais non trouvées par la requête d’entrée à droite.  
 
INTERSECT retourne des lignes distinctes générées par l’opérateur des requêtes d’entrée à gauche et à droite.  
  
Voici les règles essentielles pour combiner les ensembles de résultats de deux requêtes utilisant l'opérande EXCEPT ou l'opérande INTERSECT :  
  
-   Le nombre et l'ordre des colonnes doivent être identiques dans toutes les requêtes.  
  
-   Les types de données doivent être compatibles.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
\<_query\_specification_> | ( \<_query\_expression_> )  
Spécification ou expression de requête qui retourne les données à comparer avec les données d'une autre spécification ou expression de requête. Les définitions des colonnes faisant partie d'une opération EXCEPT ou INTERSECT ne doivent pas forcément être identiques. Mais elles doivent néanmoins être comparables par le biais d'une conversion implicite. Lorsque les types de données diffèrent, les règles de [priorité des types de données](../../t-sql/data-types/data-type-precedence-transact-sql.md) déterminent le type de données exécuté pour la comparaison.  
  
Le résultat se détermine d'après les mêmes règles de combinaison d'expressions si les types sont les mêmes mais diffèrent en terme de précision, d'échelle ou de longueur. Pour plus d’informations, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
La spécification ou l’expression de requête ne peut pas retourner de colonnes de type **xml**, **text**, **ntext**, **image** ou CLR non binaire définis par l’utilisateur car ces types de données ne sont pas comparables.  
  
EXCEPT  
Retourne toute valeur distincte de la requête à gauche de l’opérateur EXCEPT. Ces valeurs sont retournées tans que la requête de droite ne retourne pas également ces valeurs.  
  
INTERSECT  
Retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur INTERSECT.  
  
## <a name="remarks"></a>Remarques  
Les types de données de colonnes comparables sont retournés par les requêtes à gauche et à droite des opérateurs EXCEPT ou INTERSECT. Ces types de données peuvent inclure des types de données de caractères avec des classements différents. Dans ce cas, la comparaison nécessaire est exécutée selon les règles de [priorité de classement](../../t-sql/statements/collation-precedence-transact-sql.md). Si vous ne pouvez pas exécuter cette conversion, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne une erreur.  
  
Lors de la comparaison des valeurs de colonnes pour trouver les lignes DISTINCT, deux valeurs NULL sont considérées comme égales.  
  
Les opérateurs EXCEPT et INTERSECT retournent les noms de colonnes du jeu de résultats qui sont les mêmes que les noms de colonnes retournés par la requête du côté gauche de l’opérateur.  
  
Les noms ou les alias de colonnes placés dans des clauses ORDER BY doivent faire référence aux noms de colonnes retournés par la requête de gauche.  
  
La propriété de toute colonne acceptant des valeurs NULL, lesquelles font partie du jeu de résultats retourné par EXCEPT ou INTERSECT, revient à la propriété à accepter des valeurs NULL par sa colonne correspondante retournée par la requête se trouvant à gauche de l'opérateur.  
  
Si EXCEPT ou INTERSECT sont utilisés conjointement avec d'autres opérateurs au sein d'une expression, l'expression finale s'évalue d'après la règle de précédence suivante :  
  
1.  Expressions entre parenthèses  
  
2.  opérateur INTERSECT ;  
  
3.  EXCEPT et UNION évaluée de gauche à droite d'après leur position dans l'expression.  
  
Vous pouvez utiliser EXCEPT ou INTERSECT pour comparer plus de deux jeux de requêtes. Dans ce cas, la conversion de type de données est déterminée en comparant deux requêtes à la fois et en suivant les règles précédemment mentionnées relatives à l'évaluation d'expressions.  
  
EXCEPT et INTERSECT ne peuvent pas être utilisés dans une définition de vue partitionnée distribuée ou des notifications de requêtes.  
 
Vous pouvez toujours les utiliser dans les requêtes distribuées à condition de ne les exécuter que sur un serveur local et de ne pas les envoyer à un serveur lié. Il se peut par conséquent que les performances soient affectées par l'utilisation d'EXCEPT et d'INTERSECT dans des requêtes distribuées.  
  
Vous pouvez utiliser des curseurs avant uniquement statiques ou rapides dans l'ensemble de résultats s'ils sont utilisés avec une opération EXCEPT ou INTERSECT. Vous pouvez également utiliser un curseur de jeu de clés ou un curseur dynamique avec une opération EXCEPT ou INTERSECT. Dans ce cas, le curseur du jeu de résultats de l’opération est converti en curseur statique.  
  
Si une opération EXCEPT est affichée à l’aide de la fonctionnalité Graphical Showplan de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], elle apparaît sous forme de [Left Anti Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md) alors qu’une opération INTERSECT apparaît sous forme de [Left Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="examples"></a>Exemples  
Les exemples suivants illustrent l’utilisation des opérateurs `INTERSECT` et `EXCEPT`. La première requête retourne toutes les valeurs de la table `Production.Product` pour qu'elles soient comparées aux résultats à l'aide de `INTERSECT` et de `EXCEPT`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
La requête suivante retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite. Les tables sont inversées par rapport à l'exemple précédent.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Les exemples suivants illustrent la manière d'utiliser les opérateurs `INTERSECT` et `EXCEPT`. La première requête retourne toutes les valeurs de la table `FactInternetSales` pour qu'elles soient comparées aux résultats à l'aide de `INTERSECT` et de `EXCEPT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
La requête suivante retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  

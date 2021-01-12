---
description: Expressions (Transact-SQL)
title: Expressions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Boolean expressions
- expressions [SQL Server], about expressions
- combining expressions
- Transact-SQL expressions
- expressions [SQL Server], combining
- simple expressions [SQL Server]
- complex expressions [SQL Server]
ms.assetid: ee53c5c8-e36c-40f9-8cd1-d933791b98fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6db31166cf0931e7ace6a6f1366fed9776509568
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100379"
---
# <a name="expressions-transact-sql"></a>Expressions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Combinaison de symboles et d'opérateurs que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] évalue pour obtenir une seule valeur. Les expressions simples peuvent être une seule constante, variable, colonne ou fonction scalaire. Les opérateurs peuvent être utilisés pour associer plusieurs expressions simples en une expression complexe.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
{ constant | scalar_function | [ table_name. ] column | variable   
    | ( expression ) | ( scalar_subquery )   
    | { unary_operator } expression   
    | expression { binary_operator } expression   
    | ranking_windowed_function | aggregate_windowed_function  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Expression in a SELECT statement  
<expression> ::=   
{  
    constant   
    | scalar_function   
    | column  
    | variable  
    | ( expression  )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE Windows_collation_name ]  
  
-- Scalar Expression in a DECLARE, SET, IF...ELSE, or WHILE statement  
<scalar_expression> ::=  
{  
    constant   
    | scalar_function   
    | variable  
    | ( expression  )  
    | (scalar_subquery )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE { Windows_collation_name ]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
  
|Terme|Définition|  
|----------|----------------|  
|*constant*|Symbole représentant une valeur de données spécifique et unique. Pour plus d’informations, consultez [Constantes &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).|  
|*scalar_function*|Unité de syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] qui fournit un service spécifique et retourne une valeur unique. *scalar_function* peut correspondre à des fonctions scalaires intégrées, comme les fonctions SUM, GETDATE ou CAST, ou bien à des fonctions scalaires définies par l’utilisateur.|  
|[ _table_name_ **.** ]|Nom ou alias d'une table.|  
|*column*|Nom d’une colonne. Seul le nom de la colonne est autorisé dans une expression.|  
|*variable*|Nom d'une variable ou d'un paramètre. Pour plus d’informations, consultez [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).|  
|**(** _expression_  **)**|Toute expression valide définie dans cette rubrique. Les parenthèses regroupent des opérateurs et garantissent que tous les opérateurs de l'expression entre parenthèses sont traités avant que l'expression qui en résulte ne soit combinée avec une autre.|  
|**(** _scalar_subquery_ **)**|Sous-requête qui renvoie une valeur. Par exemple :<br /><br /> `SELECT MAX(UnitPrice)`<br /><br /> `FROM Products`|  
|{ *unary_operator* }|Les opérateurs unaires ne s'appliquent qu'à des expressions qui évaluent un type de données de la catégorie des types de données numériques. Opérateur qui n'a qu'un seul opérande :<br /><br /> + indique un nombre positif ;<br /><br /> - indique un nombre négatif ;<br /><br /> ~ indique l'opérateur de complément.|  
|{ *binary_operator* }|Opérateur qui définit la manière de combiner deux expressions pour parvenir à un résultat unique. *binary_operator* peut être un opérateur arithmétique, l’opérateur d’assignation (=), un opérateur au niveau du bit, un opérateur de comparaison, un opérateur logique, l’opérateur de concaténation de chaînes (+) ou un opérateur unaire. Pour plus d’informations sur les opérateurs, consultez [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md).|  
|*ranking_windowed_function*|Toute fonction de classement [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations, consultez [Fonctions de classement &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md).|  
|*aggregate_windowed_function*|Toute fonction d'agrégation [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisée avec la clause OVER. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).|  
  
## <a name="expression-results"></a>Résultats des expressions  
 Pour une expression simple composée d'une seule constante ou variable ou d'une seule fonction scalaire ou nom de colonne : les type de données, classement, précision, échelle et valeur de l'expression sont ceux de l'élément référencé.  
  
 Lorsque deux expressions sont combinées avec des opérateurs logiques ou de comparaison, il en résulte un type de données booléen ayant l'une des trois valeurs suivantes : TRUE, FALSE ou UNKNOWN. Pour plus d’informations sur les types de données booléens, consultez [Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md).  
  
 Lorsque deux expressions sont combinées avec des opérateurs arithmétiques, de niveau bit ou de chaîne de caractères, l'opérateur détermine le type de donnée du résultat.  
  
 Les expressions complexes constituées de nombreux symboles et opérateurs conduisent à un résultat à valeur unique. Le type de données, le classement, la précision et la valeur de l'expression qui en résulte sont déterminés en combinant deux par deux les expressions, et ce jusqu'à l'obtention du résultat final. La séquence selon laquelle les expressions sont combinées est définie par le degré de priorité des opérateurs dans l'expression.  
  
## <a name="remarks"></a>Remarques  
 Un opérateur peut combiner deux expressions si elles ont toutes deux un type de données pris en charge par l'opérateur et si au moins l'une des conditions suivantes est vraie :  
  
-   les expressions ont le même type de données ;  
  
-   le type de données ayant le degré de priorité le moins élevé peut être implicitement converti dans le type de données ayant le degré de priorité le plus élevé ;  
  
 Si les expressions ne respectent pas ces conditions, les fonctions CAST ou CONVERT peuvent servir à convertir explicitement le type de données ayant le degré de priorité le moins élevé, soit dans le type de données ayant le degré de priorité le plus élevé, soit dans un type de données intermédiaire qui peut être implicitement converti dans le type de données ayant le degré de priorité le plus élevé.  
  
 Si aucune conversion implicite ou explicite n'est prise en charge, les deux expressions ne peuvent pas être combinées.  
  
 Le classement de toute expression qui s'évalue à une chaîne de caractères est défini d'après les règles de priorité des classements. Pour plus d’informations, consultez [Priorité de classement &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
 Dans un langage de programmation tel que C ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], une expression s’évalue toujours à un résultat unique. Cette règle varie pour les expressions dans une liste de sélection [!INCLUDE[tsql](../../includes/tsql-md.md)] : l'expression est évaluée individuellement pour chaque ligne dans le jeu de résultats. Une même expression peut avoir une valeur différente dans chaque ligne du jeu de résultats, mais chaque ligne n'a qu'une seule valeur pour l'expression. Par exemple, dans l'instruction `SELECT` suivante, `ProductID` et `1+2` dans la liste de sélection sont tous deux des expressions :  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, 1+2  
FROM Production.Product;  
GO  
```  
  
 L'expression `1+2` s'évalue à `3` dans chaque ligne du jeu de résultats. Bien que l'expression `ProductID` génère une valeur différente dans chaque ligne du jeu de résultats, chaque ligne n'a qu'une seule valeur pour `ProductID`.  
 
- [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] alloue une quantité de mémoire maximale fixe à chaque thread, de sorte qu’aucun thread ne peut utiliser toute la mémoire.  Une partie de cette mémoire est utilisée pour le stockage des expressions des requêtes.  Si une requête a trop d’expressions et que sa mémoire nécessaire dépasse la limite interne, le moteur ne l’exécute pas.  Pour éviter ce problème, les utilisateurs peuvent transformer la requête en plusieurs requêtes contenant chacune un nombre plus petit d’expressions. Par exemple, vous avez une requête contenant une longue liste d’expressions dans la clause WHERE : 

```sql
DELETE FROM dbo.MyTable 
WHERE
(c1 = '0000001' AND c2 = 'A000001') or
(c1 = '0000002' AND c2 = 'A000002') or
(c1 = '0000003' AND c2 = 'A000003') or
...

```
Remplacez cette requête par ceci :

```sql
DELETE FROM dbo.MyTable WHERE (c1 = '0000001' AND c2 = 'A000001');
DELETE FROM dbo.MyTable WHERE (c1 = '0000002' AND c2 = 'A000002');
DELETE FROM dbo.MyTable WHERE (c1 = '0000003' AND c2 = 'A000003');
...
```

## <a name="see-also"></a>Voir aussi  
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [NULLIF &#40;Transact-SQL&#41;](../../t-sql/language-elements/nullif-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  

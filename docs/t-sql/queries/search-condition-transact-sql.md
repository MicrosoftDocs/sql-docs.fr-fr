---
description: Condition de recherche (Transact-SQL)
title: Condition de recherche (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- search
- Search Condition
- condition
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator [Transact-SQL]
- CONTAINS predicate (Transact-SQL)
- ESCAPE keyword
- operators [Transact-SQL], search condition
- search conditions [SQL Server]
- WHERE clause, search conditions
- ALL keyword
- FREETEXT predicate (Transact-SQL)
- EXISTS keyword
- search conditions [SQL Server], about search conditions
- NOT operator [Transact-SQL]
- BETWEEN operator
- SOME | ANY keyword
- predicates [full-text search]
- AND, about search conditions
- logical operators [SQL Server], about search conditions
- precedence [SQL Server], logical operators
- logical operators [SQL Server], precedence
- LIKE comparisons
ms.assetid: 09974469-c5d2-4be8-bc5a-78e404660b2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5842596b33544192604f9a2c5ea8b231dc4a6b97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347552"
---
# <a name="search-condition-transact-sql"></a>Condition de recherche (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Combinaison d'un ou de plusieurs prédicats qui utilisent les opérateurs logiques AND, OR et NOT.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<search_condition> ::=  
    MATCH(<graph_search_pattern>) | <search_condition_without_match> | <search_condition> AND <search_condition>

<search_condition_without_match> ::= 
    { [ NOT ] <predicate> | ( <search_condition_without_match> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition_without_match> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | ! > | < | < = | ! < } expression   
    | string_expression [ NOT ] LIKE string_expression   
  [ ESCAPE 'escape_character' ]   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | CONTAINS   
  ( { column | * } , '<contains_search_condition>' )   
    | FREETEXT ( { column | * } , 'freetext_string' )   
    | expression [ NOT ] IN ( subquery | expression [ ,...n ] )   
    | expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
  { ALL | SOME | ANY} ( subquery )   
    | EXISTS ( subquery )     }   
    
<graph_search_pattern> ::=
    { <node_alias> { 
                      { <-( <edge_alias> )- } 
                    | { -( <edge_alias> )-> }
                    <node_alias> 
                   } 
    }
  
<node_alias> ::=
    node_table_name | node_table_alias 

<edge_alias> ::=
    edge_table_name | edge_table_alias
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
< search_condition > ::=   
    { [ NOT ] <predicate> | ( <search_condition> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | < | < = } expression   
    | string_expression [ NOT ] LIKE string_expression   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | expression [ NOT ] IN (subquery | expression [ ,...n ] )   
    | expression [ NOT ] EXISTS (subquery)     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 \<search_condition>  
 Pour une instruction SELECT, une expression de requête ou une sous-requête, spécifie les conditions des lignes retournées dans le jeu de résultats. Pour une instruction UPDATE, spécifie les lignes à mettre à jour. Pour une instruction DELETE, spécifie les lignes à supprimer. Le nombre de prédicats pouvant être inclus dans une condition de recherche d'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est illimité.  
  
 \<graph_search_pattern>  
 Spécifie le modèle de correspondance de graphe. Pour plus d’informations sur les arguments de cette clause, consultez [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md).
 
 NOT  
 Inverse l'expression booléenne spécifiée par le prédicat. Pour plus d’informations, consultez [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
 AND  
 Combine deux conditions et a la valeur TRUE lorsque les deux conditions ont l'une et l'autre la valeur TRUE. Pour plus d’informations, consultez [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md).  
  
 OR  
 Combine deux conditions et a la valeur TRUE lorsque l'une des deux conditions a la valeur TRUE. Pour plus d’informations, consultez [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md).  
  
 \< predicate >  
 Expression qui retourne la valeur TRUE, FALSE ou UNKNOWN.  
  
 *expression*  
 Nom de colonne, constante, fonction, variable, sous-requête scalaire ou toute combinaison de noms de colonnes, de constantes et de fonctions reliées par un ou plusieurs opérateurs ou par une sous-requête. L'expression peut également contenir l'expression CASE.  
  
> [!NOTE]  
>  Les variables et constantes chaîne non Unicode utilisent la page de codes qui correspond au classement par défaut de la base de données. L’utilisation exclusive de données caractères non Unicode et la référence aux types de données caractères non Unicode **char**, **varchar** et **text** peuvent entraîner des conversions de page de codes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit les variables et constantes chaîne non Unicode dans la page de codes qui correspond au classement de la colonne référencée ou au classement spécifié avec COLLATE, si cette page de codes est différente de celle du classement par défaut de la base de données. Quand un caractère est introuvable dans la nouvelle page de codes, il est converti en un caractère similaire, si un [mappage ajusté](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/) est trouvé. Sinon, il est converti en caractère de remplacement par défaut de « ? ».  
>  
> Si vous avez plusieurs pages de codes, faites précéder les constantes caractères de la lettre majuscule 'N' et utilisez des variables Unicode pour éviter les conversions de page de codes.  
  
 =  
 Opérateur utilisé pour tester l'égalité de deux expressions.  
  
 <>  
 Opérateur utilisé pour tester l'inégalité de deux expressions.  
  
 !=  
 Opérateur utilisé pour tester l'inégalité de deux expressions.  
  
 \>  
 Opérateur utilisé pour tester la supériorité d'une expression par rapport à une autre.  
  
 \>=  
 Opérateur utilisé pour tester la supériorité ou l'égalité d'une expression par rapport à une autre.  
  
 !>  
 Opérateur utilisé pour tester la non-supériorité d'une expression par rapport à une autre.  
  
 <  
 Opérateur utilisé pour tester l'infériorité d'une expression par rapport à une autre.  
  
 <=  
 Opérateur utilisé pour tester l'infériorité ou l'égalité d'une expression par rapport à une autre.  
  
 !<  
 Opérateur utilisé pour tester la non-infériorité d'une expression par rapport à une autre.  
  
 *string_expression*  
 Chaîne de caractères et de caractères génériques.  
  
 [ NOT ] LIKE  
 Indique que la chaîne de caractères suivante sera utilisée avec une syntaxe de correspondance au modèle. Pour plus d’informations, consultez [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md).  
  
 ESCAPE **'** _escape_ character_ *_'_*  
 Permet la recherche d'un caractère générique dans une chaîne de caractères et non en tant que caractère générique. *escape_character* est le caractère placé devant le caractère générique pour indiquer cette utilisation spéciale.  
  
 [ NOT ] BETWEEN  
 Définit une plage inclusive (ou exclusive) de valeurs. Utilisez AND pour séparer les valeurs de début et de fin. Pour plus d’informations, consultez [BETWEEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/between-transact-sql.md).  
  
 IS [ NOT ] NULL  
 Définit une recherche des valeurs NULL ou non NULL suivant les mots clé utilisés. Une expression avec un opérateur au niveau du bit ou un opérateur arithmétique retourne la valeur NULL si l'un des opérandes a la valeur NULL.  
  
 CONTAINS  
 Recherche dans des colonnes de caractères les concordances plus ou moins précises (*approximatives*) avec des mots isolés ou des expressions, la proximité des mots, ainsi que les concordances pondérées. Cette option ne peut être utilisée qu'avec les instructions SELECT. Pour plus d’informations, consultez [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  
  
 FREETEXT  
 Fournit une forme simple de requête en langage naturel qui recherche dans des colonnes de caractères les synonymes et non les termes exacts du prédicat. Cette option ne peut être utilisée qu'avec les instructions SELECT. Pour plus d’informations, consultez [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md).  
  
 [ NOT ] IN  
 Définit la recherche d'une expression, selon que cette dernière est incluse ou non dans une liste. L'expression de recherche peut être une constante ou un nom de colonne ; par ailleurs, la liste peut être un ensemble de constantes ou, plus généralement, une sous-requête. Mettez la liste des valeurs entre parenthèses. Pour plus d’informations, consultez [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md).  
  
 *subquery*  
 Peut être considérée comme une instruction SELECT restreinte et est similaire à \<query_expression> dans l’instruction SELECT. La clause ORDER BY et le mot clé INTO ne sont pas autorisés. Pour plus d’informations, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
 ALL  
 Utilisé avec un opérateur de comparaison et une sous-requête. Retourne TRUE pour \<predicate> si toutes les valeurs récupérées pour la requête imbriquée satisfont à la comparaison. Retourne FALSE si certaines valeurs ne satisfont pas à la comparaison ou si la requête imbriquée ne retourne aucune ligne à l’instruction externe. Pour plus d’informations, consultez [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md).  
  
 { SOME | ANY }  
 Utilisé avec un opérateur de comparaison et une sous-requête. Retourne TRUE pour \<predicate> au moins une valeur récupérée pour la requête imbriquée satisfait à la comparaison. Retourne FALSE si aucune valeur dans la requête imbriquée ne satisfait à la comparaison ou si la requête imbriquée ne retourne aucune ligne à l’instruction externe. Dans les autres cas, l'expression est inconnue (UNKNOWN). Pour plus d’informations, consultez [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 EXISTS  
 Utilisé avec une sous-requête, vérifie l’existence de lignes retournées par la sous-requête. Pour plus d’informations, consultez [EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 L'ordre de priorité des opérateurs logiques est NOT (la plus élevée), puis AND, puis OR. Les parenthèses peuvent être utilisées pour remplacer cette priorité dans un critère de recherche. L'ordre d'évaluation des opérateurs logiques peut varier en fonction des choix effectués par l'optimiseur de requête. Pour plus d’informations sur le fonctionnement des opérateurs logiques avec des valeurs logiques, consultez [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md), [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md) et [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-where-with-like-and-escape-syntax"></a>R. Utilisation de WHERE avec la syntaxe LIKE et ESCAPE  
 L’exemple suivant recherche les lignes dans lesquelles la colonne `LargePhotoFileName` contient les caractères `green_`. Il utilise l’option `ESCAPE`, car _ est un caractère générique. Sans l’option `ESCAPE`, la requête rechercherait toutes les valeurs de description contenant le mot `green` suivi de n’importe quel caractère autre que le caractère _.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT *   
FROM Production.ProductPhoto  
WHERE LargePhotoFileName LIKE '%greena_%' ESCAPE 'a' ;  
```  
  
### <a name="b-using-where-and-like-syntax-with-unicode-data"></a>B. Utilisation de la syntaxe WHERE et LIKE avec des données Unicode  
 Cet exemple utilise la clause `WHERE` pour récupérer l'adresse postale de toutes les sociétés qui se trouvent hors des États-Unis (`US`) et dans une ville dont le nom commence par `Pa`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT AddressLine1, AddressLine2, City, PostalCode, CountryRegionCode    
FROM Person.Address AS a  
JOIN Person.StateProvince AS s ON a.StateProvinceID = s.StateProvinceID  
WHERE CountryRegionCode NOT IN ('US')  
AND City LIKE N'Pa%' ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-where-with-like"></a>C. Utilisation de WHERE avec LIKE  
 L’exemple suivant recherche les lignes dans lesquelles la colonne `LastName` contient les caractères `and`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE '%and%';  
```  
  
### <a name="d-using-where-and-like-syntax-with-unicode-data"></a>D. Utilisation de la syntaxe WHERE et LIKE avec des données Unicode  
 L’exemple suivant utilise la clause `WHERE` pour effectuer une recherche de données Unicode dans la colonne `LastName`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE N'%and%';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  


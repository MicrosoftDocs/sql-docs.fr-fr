---
title: SELECT (Transact-SQL)
description: SELECT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SELECT_TSQL
- SELECT
dev_langs:
- TSQL
helpviewer_keywords:
- retrieving rows
- SELECT statement [SQL Server]
- SELECT statement [SQL Server], about SELECT statement
- row retrieval [SQL Server], SELECT statement
- DML [SQL Server], SELECT statement
- data manipulation language [SQL Server], SELECT statement
- row retrieval [SQL Server]
- queries [SQL Server], results
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 10/24/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dff1dc7f969435e4a4f112e995fe092e8b4da8be
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005370"
---
# <a name="select-transact-sql"></a>SELECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Récupère des lignes de la base de données et permet de sélectionner une ou plusieurs lignes ou colonnes d’une ou de plusieurs tables dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La syntaxe complète de l'instruction SELECT est complexe mais en voici les principales clauses :  
  
[ WITH { [[ XMLNAMESPACES ,]](../../t-sql/xml/with-xmlnamespaces.md) [[ \<common_table_expression> ]](../../t-sql/queries/with-common-table-expression-transact-sql.md) } ]
  
 [SELECT *select_list*](../../t-sql/queries/select-clause-transact-sql.md) [ [INTO *new_table*](../../t-sql/queries/select-into-clause-transact-sql.md) ]  
  
 [ [FROM *table_source*](../../t-sql/queries/from-transact-sql.md) ] [ [WHERE *search_condition*](../../t-sql/queries/where-transact-sql.md) ]  
  
 [ [GROUP BY *group_by_expression*](../../t-sql/queries/select-group-by-transact-sql.md) ]  
  
 [ [HAVING *search_condition*](../../t-sql/queries/select-having-transact-sql.md) ]  
  
 [ [ORDER BY *order_expression* [ ASC | DESC ]](../../t-sql/queries/select-order-by-clause-transact-sql.md)]  
  
 Les opérateurs [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md), [EXCEPT et INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md) peuvent être utilisés entre plusieurs requêtes pour combiner ou comparer leurs résultats dans un seul jeu de résultats.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<SELECT statement> ::=    
    [ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> [,...n] ] } ]  
    <query_expression>   
    [ ORDER BY <order_by_expression> ] 
    [ <FOR Clause>]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]   
<query_expression> ::=   
    { <query_specification> | ( <query_expression> ) }   
    [  { UNION [ ALL ] | EXCEPT | INTERSECT }  
        <query_specification> | ( <query_expression> ) [...n ] ]   
<query_specification> ::=   
SELECT [ ALL | DISTINCT ]   
    [TOP ( expression ) [PERCENT] [ WITH TIES ] ]   
    < select_list >   
    [ INTO new_table ]   
    [ FROM { <table_source> } [ ,...n ] ]   
    [ WHERE <search_condition> ]   
    [ <GROUP BY> ]   
    [ HAVING < search_condition > ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ WITH <common_table_expression> [ ,...n ] ]  
SELECT <select_criteria>  
[;]  
  
<select_criteria> ::=  
    [ TOP ( top_expression ) ]   
    [ ALL | DISTINCT ]   
    { * | column_name | expression } [ ,...n ]   
    [ FROM { table_source } [ ,...n ] ]  
    [ WHERE <search_condition> ]   
    [ GROUP BY <group_by_clause> ]   
    [ HAVING <search_condition> ]   
    [ ORDER BY <order_by_expression> ]  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes  
 En raison de la complexité de l'instruction SELECT, les éléments et les arguments de la syntaxe sont détaillés par clause :  

:::row:::
    :::column:::
        [WITH XMLNAMESPACES](../../t-sql/xml/with-xmlnamespaces.md)
    :::column-end:::
    :::column:::
        [HAVING](../../t-sql/queries/select-having-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WITH common_table_expression](../../t-sql/queries/with-common-table-expression-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SELECT (Clause)](../../t-sql/queries/select-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [EXCEPT et INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Clause INTO](../../t-sql/queries/select-into-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FROM](../../t-sql/queries/from-transact-sql.md)
    :::column-end:::
    :::column:::
        [Clause FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WHERE](../../t-sql/queries/where-transact-sql.md)
    :::column-end:::
    :::column:::
        [OPTION, clause](../../t-sql/queries/option-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 L'ordre des clauses dans une instruction SELECT est de première importance. Vous pouvez omettre n'importe quelle clause facultative mais, lorsque vous employez les clauses facultatives, elles doivent apparaître dans l'ordre adéquat.  
  
 Les instructions SELECT sont autorisées dans les fonctions définies par l'utilisateur uniquement si les listes de sélection de ces instructions contiennent des expressions qui attribuent des valeurs aux variables qui sont locales aux fonctions.  
  
 Un nom en quatre parties, dont la partie nom de serveur est établie avec la fonction OPENDATASOURCE, peut être utilisé comme source de table dans tous les cas où il est possible d’inclure un nom de table dans une instruction SELECT. Un nom en quatre parties ne peut pas être spécifié pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Certaines restrictions syntaxiques s'appliquent aux instructions SELECT impliquant des tables distantes.  
  
## <a name="logical-processing-order-of-the-select-statement"></a>Ordre logique de traitement de l’instruction SELECT  
 Les étapes suivantes indiquent l'ordre de traitement logique, ou ordre de liaison, d'une instruction SELECT. Cet ordre détermine à quel moment les objets définis au cours d'une étape deviennent disponibles pour les clauses des étapes suivantes. Par exemple, si le processeur de requêtes peut se lier (accéder) aux tables ou vues définies dans la clause FROM, ces objets et leurs colonnes deviennent disponibles pour toutes les étapes suivantes. À l'inverse, puisque la clause SELECT correspond à l'étape 8, aucun alias de colonne ni aucune colonne dérivée défini(e) dans cette clause ne peut être référencé(e) par les clauses précédentes. Cependant, ils peuvent être référencés par les clauses suivantes telles que la clause ORDER BY. L’exécution physique réelle de l’instruction est déterminée par le processeur de requêtes, et l’ordre peut différer de cette liste.  
  
1.  FROM  
2.  ACTIVÉ  
3.  JOIN  
4.  WHERE  
5.  GROUP BY  
6.  WITH CUBE ou WITH ROLLUP  
7.  HAVING  
8.  SELECT  
9. DISTINCT  
10. ORDER BY  
11. Haut de la page  

> [!WARNING]
> La séquence précédente est généralement celle qui est appliquée. Toutefois, dans certains cas rares, la séquence peut s’exécuter différemment.
>
> Par exemple, supposons qu’un index cluster est appliqué à un affichage qui exclut certaines lignes de la table, et que la liste de colonnes SELECT dans l’affichage utilise une clause CONVERT qui convertit un type de données *varchar* en *integer*. Dans ce cas, la clause CONVERT peut s’exécuter avant la clause WHERE. Ce cas se produit rarement. Il y a souvent un moyen de modifier votre affichage pour éviter tout changement de la séquence, si cela est important dans votre cas. 

## <a name="permissions"></a>Autorisations  
 La sélection de données requiert l'autorisation **SELECT** sur la table ou la vue, qui pourrait être héritée d'une étendue supérieure telle que l'autorisation **SELECT** sur le schéma ou l'autorisation **CONTROL** sur la table. La sélection peut également nécessiter l’appartenance au rôle de base de données fixe **db_datareader** ou **db_owner**, ou au rôle de serveur fixe **sysadmin**. La création d’une nouvelle table à l’aide de **SELECT INTO** requiert également l’autorisation **CREATE TABLE** et l’autorisation **ALTER SCHEMA** sur le schéma qui possède la nouvelle table.  
  
## <a name="examples"></a>Exemples :   
Les exemples suivants utilisent la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].
  
### <a name="a-using-select-to-retrieve-rows-and-columns"></a>R. Utilisation de SELECT pour extraire des lignes et des colonnes  
 Cette section présente trois exemples de code. Le premier exemple de code retourne toutes les lignes (aucune clause WHERE n’est définie) et toutes les colonnes (en utilisant `*`) de la table `DimEmployee`.  
  
```sql  
SELECT *  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
 L’exemple suivant donne le même résultat, mais en utilisant des alias de table.  
  
```sql  
SELECT e.*  
FROM DimEmployee AS e  
ORDER BY LastName;  
```  
  
 Cet exemple retourne toutes les lignes (aucune clause WHERE n’est définie) et un sous-ensemble des colonnes (`FirstName`, `LastName`, `StartDate`) de la table `DimEmployee` dans la base de données `AdventureWorksPDW2012`. L’en-tête de la troisième colonne est renommé `FirstDay`.  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
ORDER BY LastName;  
```  
  
 Cet exemple retourne uniquement les lignes de la table `DimEmployee` qui ont une valeur `EndDate` non NULL et une valeur `MaritalStatus` égale à « M » (marié).  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
WHERE EndDate IS NOT NULL   
AND MaritalStatus = 'M'  
ORDER BY LastName;  
```  
  
### <a name="b-using-select-with-column-headings-and-calculations"></a>B. Utilisation de SELECT avec des en-têtes de colonne et des calculs  
 L’exemple suivant retourne toutes les lignes de la table `DimEmployee`, et calcule le salaire brut de chaque employé sur la base de la valeur `BaseRate` et d’une semaine de 40 heures de travail.  
  
```sql  
SELECT FirstName, LastName, BaseRate, BaseRate * 40 AS GrossPay  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
### <a name="c-using-distinct-with-select"></a>C. Utilisation de DISTINCT avec SELECT  
 L’exemple suivant utilise `DISTINCT` pour générer une liste de tous les titres uniques figurant dans la table `DimEmployee`.  
  
```sql  
SELECT DISTINCT Title  
FROM DimEmployee  
ORDER BY Title;  
```  
  
### <a name="d-using-group-by"></a>D. Utilisation de GROUP BY  
 L’exemple suivant calcule le montant total des ventes réalisées par jour.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
 Comme la clause `GROUP BY` est utilisée, une seule ligne contenant la somme de toutes les ventes est retournée pour chaque jour.  
  
### <a name="e-using-group-by-with-multiple-groups"></a>E. Utilisation de GROUP BY avec plusieurs groupes  
 L’exemple suivant calcule le prix moyen et la somme des ventes sur Internet pour chaque jour, en regroupant les résultats en fonction de la date de commande et de la clé de promotion.  
  
```sql  

SELECT OrderDateKey, PromotionKey, AVG(SalesAmount) AS AvgSales, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey, PromotionKey  
ORDER BY OrderDateKey;   
```  
  
### <a name="f-using-group-by-and-where"></a>F. Utilisation de GROUP BY et WHERE  
 L’exemple suivant regroupe les résultats après avoir récupéré uniquement les lignes dont les dates de commande sont postérieures au 1er août 2002.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
WHERE OrderDateKey > '20020801'  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="g-using-group-by-with-an-expression"></a>G. Utilisation de GROUP BY avec une expression  
 L'exemple suivant effectue un regroupement en fonction d'une expression. Vous pouvez spécifier un regroupement en fonction d'une expression à condition qu'elle ne contienne pas de fonction d'agrégation.  
  
```sql  
SELECT SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY (OrderDateKey * 10);  
```  
  
### <a name="h-using-group-by-with-order-by"></a>H. Utilisation de GROUP BY avec ORDER BY  
 L’exemple suivant calcule la somme des ventes par jour, en triant les résultats par date.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-the-having-clause"></a>I. Utilisation de la clause HAVING  
 Cette requête utilise la clause `HAVING` pour limiter les résultats.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
HAVING OrderDateKey > 20010000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)  
 [Indicateurs &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)
  


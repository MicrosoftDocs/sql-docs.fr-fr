---
title: Clause FROM plus JOIN, APPLY, PIVOT (T-SQL)
description: Clause FROM plus JOIN, APPLY, PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d1ac3e525ba2e7f3cf459a7ce0ae309d22263aa1
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172401"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>Clause FROM plus JOIN, APPLY, PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Dans Transact-SQL, la clause FROM est disponible sur les instructions suivantes :

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

La clause FROM est généralement obligatoire sur l’instruction SELECT. L’exception est quand aucune colonne de table n’est listée, et que les seuls éléments listés sont des littéraux, des variables ou des expressions arithmétiques.

Cet article aborde également les mots clés suivants qui peuvent être utilisés sur la clause FROM :

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
\<table_source>  
 Spécifie une table, une vue, une variable de table ou une source de table dérivée, avec ou sans alias, à utiliser dans l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous pouvez utiliser jusqu'à 256 sources de table dans une instruction, bien que cette limite varie en fonction de la mémoire disponible et de la complexité des autres expressions constituant la requête. Les requêtes individuelles peuvent ne pas prendre en charge 256 sources de table.  
  
> [!NOTE]  
>  Les performances des requêtes risquent de baisser si le nombre des tables référencées dans une requête est élevé. Les durées de compilation et d'optimisation sont également affectées par d'autres facteurs. Parmi ceux-ci figurent les index et les vues indexées de chaque \<table_source>, ainsi que la taille de \<select_list> dans l’instruction SELECT.  
  
 L'ordre des sources de table après le mot clé FROM n'a aucune incidence sur le jeu de résultats retourné. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des erreurs lorsque des noms dupliqués apparaissent dans la clause FROM.  
  
 *table_or_view_name*  
 Nom d'une table ou d'une vue.  
  
 Si la table ou la vue existe dans une autre base de données sur la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez un nom complet qui respecte la syntaxe *database*.*schema*.*object_name*.  
  
 Si la table ou la vue existe en dehors de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez un nom en quatre parties sous la forme *linked_server*.*catalog*.*schema*.*object*. Pour plus d’informations, consultez [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Un nom en quatre parties est construit à l’aide de la fonction [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md), car la partie serveur du nom peut également être utilisée pour spécifier la source de la table distante. Quand OPENDATASOURCE est spécifié, *database_name* et *schema_name* peuvent ne pas s’appliquer à toutes les sources de données ; par ailleurs, ils dépendent des fonctionnalités du fournisseur OLE DB qui accède à l’objet distant.  
  
 [AS] *table_alias*  
 Alias de *table_source* utilisé soit par commodité, soit pour distinguer une table ou une vue dans une jointure réflexive ou une sous-requête. Un alias correspond souvent au raccourci du nom de table utilisé pour faire référence aux colonnes spécifiques des tables dans une jointure. Si le même nom de colonne existe dans plusieurs tables de la jointure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exige que le nom de la colonne soit qualifié par un nom de table, un nom de vue ou un alias. Le nom de table ne peut pas être utilisé si un alias est défini.  
  
 Quand une table dérivée, un ensemble de lignes ou une fonction table, ou encore une clause d’opérateur (telle que PIVOT ou UNPIVOT) sont utilisés, l’alias *table_alias* requis à la fin de la clause est le nom de table associé pour toutes les colonnes retournées, notamment les colonnes de regroupement.  
  
 WITH (\<table_hint> )  
 Spécifie que l'optimiseur de requête utilise une stratégie d'optimisation ou de verrouillage avec cette table et pour cette instruction. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
*rowset_function*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Spécifie l'une des fonctions d'ensemble de lignes, par exemple OPENROWSET, qui retourne un objet pouvant être utilisé à la place d'une référence de table. Pour plus d’informations sur une liste de fonctions d’ensemble de lignes, consultez [Fonctions d’ensemble de lignes &#40;Transact-SQL&#41;](../functions/opendatasource-transact-sql.md).  
  
 L'utilisation des fonctions OPENROWSET et OPENQUERY pour spécifier un objet distant dépend des fonctionnalités du fournisseur OLE DB qui accède à l'objet.  
  
 *bulk_column_alias*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
 Alias facultatif qui peut remplacer un nom de colonne dans le jeu de résultats. Les alias de colonne sont autorisés uniquement dans les instructions SELECT qui utilisent la fonction OPENROWSET avec l'option BULK. Quand vous utilisez *bulk_column_alias*, spécifiez un alias pour chaque colonne de table dans le même ordre que les colonnes du fichier.  
  
> [!NOTE]  
>  Cet alias remplace l'attribut NAME dans les éléments COLUMN d'un fichier XML, le cas échéant.  
  
 *user_defined_function*  
 Spécifie une fonction table.  
  
OPENXML \<openxml_clause>  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Fournit une vue de l'ensemble de lignes d'un document XML. Pour plus d’informations, consultez [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 *derived_table*  
 Sous-requête qui récupère les lignes de la base de données. *derived_table* est utilisé comme entrée de la requête externe.  
  
 *derived_table* peut utiliser la fonctionnalité de constructeur de valeurs de table [!INCLUDE[tsql](../../includes/tsql-md.md)] pour spécifier plusieurs lignes. Par exemple : `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. Pour plus d’informations, consultez [Constructeur de valeurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 *column_alias*  
 Alias facultatif qui peut remplacer un nom de colonne dans le jeu de résultats de la table dérivée. Utilisez un alias de colonne pour chaque colonne de la liste de sélection et placez l'intégralité de la liste d'alias de colonnes entre parenthèses.  
  
 *table_or_view_name* FOR SYSTEM_TIME \<system_time>S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et ultérieur, et à [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Spécifie qu’une version spécifique des données est retournée à partir de la table temporelle spécifiée et de la table d’historique associée avec versions gérées par le système  
  
### <a name="tablesample-clause"></a>Clause Tablesample
**S’applique à :** SQL Server, SQL Database Spécifie qu'un exemple de données est retourné à partir de la table. L'exemple peut être approximatif. Cette clause peut être utilisée sur toute table primaire ou jointe dans une instruction SELECT ou UPDATE. TABLESAMPLE ne peut pas être spécifié avec des vues.  
  
> [!NOTE]  
>  Lorsque vous utilisez TABLESAMPLE sur des bases de données mises à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le niveau de compatibilité de la base de données est défini à 110 ou plus, PIVOT n'est pas autorisé dans une requête d'expression de table commune récursive (CTE). Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 SYSTEM  
 Méthode d'échantillonnage dépendante de l'implémentation et spécifiée par les normes ISO. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il s'agit de la seule méthode d'échantillonnage disponible et elle s'applique par défaut. SYSTEM applique une méthode d'échantillonnage basée sur des pages, dans laquelle un jeu de pages aléatoire est choisi pour l'exemple ; toutes les lignes de ces pages sont retournées comme exemple de sous-ensemble.  
  
 *sample_number*  
 Expression numérique constante exacte ou approximative qui représente le pourcentage ou le nombre de lignes. Quand PERCENT est spécifié, l’expression *sample_number* est convertie implicitement en valeur **float** ; sinon, elle est convertie en **bigint**. PERCENT est la valeur par défaut.  
  
 PERCENT  
 Spécifie qu’un pourcentage *sample_number* des lignes de la table doit être récupéré à partir de cette dernière. Lorsque PERCENT est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une approximation du pourcentage spécifié. Quand PERCENT est spécifié, l’expression *sample_number* doit correspondre à une valeur comprise entre 0 et 100.  
  
 ROWS  
 Spécifie qu’un nombre approximatif de lignes équivalent à *sample_number* doit être récupéré. Lorsque ROWS est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une valeur approximative du nombre de lignes indiqué. Quand ROWS est spécifié, l’expression *sample_number* doit correspondre à une valeur entière supérieure à zéro.  
  
 REPEATABLE  
 Indique que l'exemple sélectionné peut être retourné à nouveau. Quand il est spécifié avec la même valeur *repeat_seed*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le même sous-ensemble de lignes tant qu’aucune modification n’est apportée aux lignes de la table. Quand il est spécifié avec une autre valeur *repeat_seed*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne généralement d’autres exemples de lignes de la table. Les actions suivantes effectuées sur la table sont considérées comme des modifications : insertion, mise à jour, suppression, reconstruction ou défragmentation d'index, restauration ou attachement de base de données.  
  
 *repeat_seed*  
 Expression d'un entier de type constante utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour générer un nombre aléatoire. *repeat_seed* est une valeur **bigint**. Si la valeur *repeat_seed* n’est pas spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lui assigne une valeur aléatoire. Pour une valeur *repeat_seed* spécifique, le résultat de l’échantillonnage reste toujours identique si aucune modification n’est apportée à la table. L’expression *repeat_seed* doit correspondre à un entier supérieur à zéro.  
  
### <a name="tablesample-clause"></a>Clause Tablesample
**S’applique à :** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

 Spécifie qu'un exemple de données est retourné à partir de la table. L'exemple peut être approximatif. Cette clause peut être utilisée sur toute table primaire ou jointe dans une instruction SELECT ou UPDATE. TABLESAMPLE ne peut pas être spécifié avec des vues. 

 PERCENT  
 Spécifie qu’un pourcentage *sample_number* des lignes de la table doit être récupéré à partir de cette dernière. Lorsque PERCENT est spécifié, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] retourne une approximation du pourcentage spécifié. Quand PERCENT est spécifié, l’expression *sample_number* doit correspondre à une valeur comprise entre 0 et 100.  


### <a name="joined-table"></a>Table jointe 
Une table jointe est un jeu de résultats correspondant au produit de deux ou plusieurs tables. Pour plusieurs jointures, utilisez des parenthèses afin de modifier l'ordre naturel des jointures.  
  
### <a name="join-type"></a>Type de jointure
Spécifie le type d'opération de jointure.  
  
 INNER  
 Spécifie que toutes les paires correspondantes de lignes sont retournées. Supprime les lignes n'ayant pas de correspondance dans les deux tables. Lorsqu'aucun type de jointure n'est spécifié, cet argument est la valeur par défaut.  
  
 FULL [ OUTER ]  
 Spécifie qu'une ligne de la table de gauche ou de droite qui ne respecte pas la condition de jointure est incluse dans le jeu de résultats et que les colonnes de sortie qui correspondent à l'autre table ont des valeurs NULL. Cela s'ajoute à toutes les lignes généralement retournées par INNER JOIN.  
  
 LEFT [ OUTER ]  
 Spécifie que toutes les lignes de la table de gauche qui ne respectent pas la condition de jointure sont incluses dans le jeu de résultats et que les colonnes de sortie de l'autre table ont des valeurs NULL en plus de toutes les lignes retournées par la jointure interne.  
  
 RIGHT [OUTER]  
 Spécifie que toutes les lignes de la table de droite qui ne respectent pas la condition de jointure sont incluses dans le jeu de résultats et que les colonnes de sortie qui correspondent à l'autre table ont des valeurs NULL en plus de toutes les lignes retournées par la jointure interne.  
  
### <a name="join-hint"></a>Indicateur de jointure  
Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)], spécifie que l’optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un indicateur de jointure (ou un algorithme d’exécution) pour chaque jointure spécifiée dans la clause FROM de la requête. Pour plus d’informations, consultez [Indicateurs de jointure &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
 Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], ces indicateurs de jointure s’appliquent aux jointures INNER sur deux colonnes de distribution incompatibles. Ils contribuent à améliorer les performances de la requête, car ils limitent la quantité de données déplacées pendant le traitement de la requête. Les indicateurs de jointure pris en charge pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] sont les suivants :  
  
 REDUCE  
 Réduit le nombre de lignes de la table à déplacer à droite de la jointure afin que les deux tables de distribution incompatibles deviennent compatibles. L’indicateur REDUCE est également appelé indicateur de semi-jointure.  
  
 REPLICATE  
 Réplique les valeurs contenues dans la colonne de jointure de la table à gauche de la jointure sur tous les nœuds. La table à droite est jointe à la version répliquée de ces colonnes.  
  
 REDISTRIBUTE  
 Force la distribution de deux sources de données sur les colonnes spécifiées dans la clause JOIN. Dans une table distribuée, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] effectue un déplacement aléatoire. Dans une table répliquée, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] effectue un déplacement par suppression. Pour comprendre ces types de déplacements, consultez la section sur les opérations du plan de requête DMS dans la rubrique « Understanding Query Plans » de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Cet indicateur peut améliorer les performances quand le plan de requête utilise un déplacement par diffusion pour résoudre un problème de jointure de distribution incompatible.  
  
 JOIN  
 Indique que l'opération de jointure spécifiée doit avoir lieu entre les sources de table ou les vues spécifiées.  
  
 ON \<search_condition>  
 Spécifie la condition sur laquelle se base la jointure. Celle-ci peut spécifier n'importe quel prédicat, bien que les colonnes et les opérateurs de comparaison soient souvent utilisés, par exemple :  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 Lorsque la condition spécifie des colonnes, celles-ci ne doivent pas nécessairement avoir le même nom ou le même type de données. Toutefois, si les types de données sont différents, ils doivent être compatibles ou pouvoir être convertis implicitement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si les types de données ne peuvent pas être convertis implicitement, la condition doit convertir explicitement le type de données à l'aide de la fonction CONVERT.  
  
 Il se peut que des prédicats n'impliquent qu'une seule des tables jointes dans la clause ON. Ces prédicats peuvent également figurer dans la clause WHERE de la requête. Bien que la position de ces prédicats soit sans importance dans le cas de jointures INNER, ils peuvent aboutir à un résultat différent dans le cas de jointures OUTER. En effet, les prédicats figurant dans la clause ON sont appliqués à la table avant de l'être à la jointure, tandis que la clause WHERE est sémantiquement appliquée au résultat de la jointure.  
  
 Pour plus d’informations sur les prédicats et conditions de recherche, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CROSS JOIN  
 Spécifie le produit croisé de deux tables. Retourne les mêmes lignes comme si aucune clause WHERE n'avait été spécifiée dans une ancienne jointure d'un style différent de SQL-92.  
  
 *left_table_source* { CROSS | OUTER } APPLY *right_table_source*  
 Spécifie que l’argument *right_table_source* de l’opérateur APPLY est évalué par rapport à chaque ligne de *left_table_source*. Cette fonctionnalité est utile quand *right_table_source* contient une fonction table qui utilise des valeurs de colonne de *left_table_source* comme l’un de ses arguments.  
  
 Soit CROSS, soit OUTER doit être spécifié avec APPLY. Quand CROSS est spécifié, aucune ligne n’est générée quand *right_table_source* est évalué par rapport à une ligne spécifique de *left_table_source*. Un jeu de résultats vide est alors retourné.  
  
 Quand OUTER est spécifié, une ligne est générée pour chaque ligne de *left_table_source*, même si *right_table_source* est évalué par rapport à cette ligne ; de plus, un jeu de résultats vide est retourné.  
  
 Pour plus d'informations, consultez la section Notes.  
  
 *left_table_source*  
 Source de table, telle qu'elle est définie dans l'argument précédent. Pour plus d'informations, consultez la section Notes.  
  
 *right_table_source*  
 Source de table, telle qu'elle est définie dans l'argument précédent. Pour plus d'informations, consultez la section Notes.  
  
### <a name="pivot-clause"></a>Clause PIVOT

 *table_source* PIVOT \<pivot_clause>  
 Spécifie que la source *table_source* est croisée dynamiquement sur la colonne *pivot_column*. *table_source* correspond à une table ou une expression de table. La sortie est une table qui contient toutes les colonnes de la source *table_source*, à l’exception des colonnes *pivot_column* et *value_column*. Les colonnes de *table_source* (sauf les colonnes *pivot_column* et *value_column*) sont appelées colonnes de regroupement de l’opérateur PIVOT. Pour plus d’informations sur les opérateurs PIVOT et UNPIVOT, consultez [Utilisation des opérateurs PIVOT et UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
 PIVOT opère un regroupement sur la table d'entrée par rapport aux colonnes de regroupement et retourne une ligne par groupe. En outre, la sortie contient une colonne pour chaque valeur spécifiée dans la liste *column_list* qui s’affiche dans la colonne *pivot_column* de la table *input_table*.  
  
 Pour plus d'informations, consultez les remarques qui suivent.  
  
 *aggregate_function*  
 Fonction d'agrégation système ou définie par l'utilisateur qui accepte une ou plusieurs entrées. Cette fonction doit être indifférente aux valeurs Null. Une fonction d'agrégation indifférente aux valeurs Null ne prend pas en considération les valeurs Null dans le groupe lors de l'évaluation de la valeur d'agrégation.  
  
 La fonction d'agrégation système COUNT(*) n'est pas autorisée.  
  
 *value_column*  
 Colonne de valeur de l'opérateur PIVOT. Quand UNPIVOT est utilisé, *value_column* ne peut pas avoir le même nom qu’une colonne existante dans la source d’entrée *table_source*.  
  
 FOR *pivot_column*  
 Colonne de tableau croisé dynamique de l'opérateur PIVOT. *pivot_column* doit être d’un type de données implicitement ou explicitement convertible en **nvarchar()** . Cette colonne ne peut pas être de type **image** ou **rowversion**.  
  
 Quand UNPIVOT est utilisé, *pivot_column* correspond au nom de la colonne de sortie qui est réduite à partir de *table_source*. *table_source* ne peut pas contenir de colonne portant ce nom.  
  
 IN (*column_list*)  
 Dans la clause PIVOT, répertorie les valeurs dans *pivot_column* qui doivent devenir des noms de colonnes dans la table de sortie. La liste ne peut pas contenir de noms de colonnes déjà existants dans la source d’entrée *table_source* qui est croisée dynamiquement.  
  
 Dans la clause UNPIVOT, répertorie les colonnes dans *table_source* qui doivent être réduites à une seule colonne *pivot_column*.  
  
 *table_alias*  
 Nom d'alias de la table de sortie. *pivot_table_alias* doit être spécifié.  
  
 UNPIVOT \<unpivot_clause>  
 Spécifie que la table d’entrée est réduite de plusieurs colonnes dans *column_list* à une seule colonne appelée *pivot_column*. Pour plus d’informations sur les opérateurs PIVOT et UNPIVOT, consultez [Utilisation des opérateurs PIVOT et UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
AS OF \<date_time>  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Renvoie une table avec un seul enregistrement par ligne contenant les valeurs qui étaient réelles (actuelles) au moment dans le passé spécifié. En interne, une union est effectuée entre la table temporelle et sa table d’historique. Les résultats sont filtrés de manière à retourner les valeurs de la ligne qui étaient valides au moment spécifié par le paramètre *\<date_time>* . La valeur d’une ligne est considérée comme valide si *system_start_time_column_name* a une valeur inférieure ou égale à celle du paramètre *\<date_time>* , et si *system_end_time_column_name* a une valeur supérieure à celle du paramètre *\<date_time>* .   
  
FROM \<start_date_time> TO \<end_date_time>
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et ultérieur et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
Retourne une table avec les valeurs de toutes les versions d’enregistrement qui étaient actives pendant l’intervalle de temps spécifié, sans tenir compte du fait qu’elles soient devenues actives avant la valeur du paramètre *\<start_date_time>* pour l’argument FROM ou qu’elles aient cessé d’être actives après la valeur du paramètre *\<end_date_time>* pour l’argument TO. En interne, une union est effectuée entre la table temporelle et sa table d’historique. Les résultats sont filtrés de manière à renvoyer les valeurs de toutes les versions de ligne qui étaient actives à tout moment de l’intervalle spécifié. Les lignes qui sont devenues actives exactement sur la limite inférieure définie par le point de terminaison FROM sont incluses. Celles qui sont devenus actives exactement sur la limite supérieure définie par le point de terminaison TO ne sont pas incluses.  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Identique à la description de **FROM \<start_date_time> TO \<end_date_time>** ci-dessus, sauf que les lignes qui sont devenues actives au niveau de la limite supérieure définie par le point de terminaison \<end_date_time> sont incluses.  
  
CONTAINED IN (\<start_date_time> , \<end_date_time>)  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Renvoie une table avec les valeurs de toutes les versions d’enregistrement qui ont été ouvertes et fermées dans l’intervalle de temps spécifié défini par les deux valeurs datetime de l’argument CONTAINED IN. Les lignes qui sont devenues actives exactement sur la limite inférieure ou qui ont cessé d’être actives exactement sur la limite supérieure sont incluses.  
  
ALL  
Retourne une table contenant les valeurs de toutes les lignes de la table actuelle et la table d’historique.  
  
## <a name="remarks"></a>Notes  
 La clause FROM prend en charge la syntaxe SQL-92 pour les tables jointes et les tables dérivées. La syntaxe SQL-92 fournit les opérateurs de jointure INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER et CROSS.  
  
 UNION et JOIN à l'intérieur d'une clause FROM sont pris en charge aussi bien dans les vues que dans les tables dérivées et les sous-requêtes.  
  
 Une jointure réflexive est une table qui effectue une jointure avec elle-même. Les insertions ou les mises à jour basées sur une jointure réflexive suivent l'ordre de la clause FROM.  
  
 Sachant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en compte les statistiques de distribution et de cardinalité à partir des serveurs liés qui fournissent les statistiques de distribution des colonnes, l'indicateur de jointure REMOTE n'est pas nécessaire pour forcer l'évaluation d'une jointure à distance. Le processeur de requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en considération les statistiques distantes et détermine l'opportunité ou non d'une stratégie de jointure distante. L'indicateur de jointure REMOTE est utile pour les fournisseurs qui ne produisent pas de statistiques de distribution des colonnes.  
  
## <a name="using-apply"></a>Utilisation de l'opérateur APPLY  
 Les opérandes de gauche et de droite de l'opérateur APPLY sont des expressions de table. Leur principale différence est que *right_table_source* peut utiliser une fonction table qui prend une colonne de *left_table_source* comme l’un des arguments de cette fonction. *left_table_source* peut inclure des fonctions table, mais ne peut pas contenir d’arguments qui représentent des colonnes de *right_table_source*.  
  
L'opérateur APPLY fonctionne de la même façon pour produire la source de table pour la clause FROM :  
  
1.  Évalue *right_table_source* par rapport à chaque ligne de *left_table_source* pour générer des ensembles de lignes.  
  
    Les valeurs dans *right_table_source* dépendent de *left_table_source*. *right_table_source* peut être représenté approximativement de la façon suivante : `TVF(left_table_source.row)`, où `TVF` est une fonction table.  
  
2.  Combine les jeux de résultats générés pour chaque ligne lors de l’évaluation de *right_table_source* avec *left_table_source* en effectuant une opération UNION ALL.  
  
    La liste de colonnes résultant de l’utilisation de l’opérateur APPLY correspond à l’ensemble des colonnes de *left_table_source*, combiné avec la liste des colonnes de *right_table_source*.  
  
## <a name="using-pivot-and-unpivot"></a>Utilisation des opérateurs PIVOT et UNPIVOT  
 *pivot_column* et *value_column* sont des colonnes de regroupement utilisées par l’opérateur PIVOT. PIVOT suit le processus ci-après pour obtenir le jeu de résultats de sortie :  
  
1.  Il effectue une opération GROUP BY sur sa table d’entrée *input_table* par rapport aux colonnes de regroupement et génère une ligne de sortie pour chaque groupe.  
  
     Les colonnes de regroupement dans la ligne de sortie obtiennent les valeurs de colonnes correspondantes pour ce groupe dans *input_table*.  
  
2.  Il génère des valeurs pour les colonnes dans la liste de colonnes pour chaque ligne de sortie en effectuant les opérations suivantes :  
  
    1.  Regroupement supplémentaire des lignes générées dans GROUP BY à l’étape précédente, par rapport à *pivot_column*.  
  
         Sélection d’un sous-groupe qui remplit la condition ci-après, pour chaque colonne de sortie figurant dans *column_list* :  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  *aggregate_function* est évalué par rapport à *value_column* sur ce sous-groupe et son résultat est retourné en tant que valeur *output_column* correspondante. Si le sous-groupe est vide, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une valeur NULL pour *output_column*. Si la fonction d'agrégation est COUNT et si le sous-groupe est vide, la valeur zéro (0) est retournée.  

> [!NOTE]
> Les identificateurs de colonne dans la clause `UNPIVOT` suivent le classement de catalogue. Pour [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], le classement est toujours `SQL_Latin1_General_CP1_CI_AS`. Pour les bases de données partiellement autonomes [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], le classement est toujours `Latin1_General_100_CI_AS_KS_WS_SC`. Si la colonne est combinée avec d’autres colonnes, une clause Collate (`COLLATE DATABASE_DEFAULT`) doit être ajoutée pour éviter les conflits.   
  
 Pour plus d’informations sur les opérateurs PIVOT et UNPIVOT, et obtenir des exemples, consultez [Utilisation des opérateurs PIVOT et UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite les autorisations appropriées pour l'instruction DELETE, SELECT ou UPDATE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-simple-from-clause"></a>R. Utilisation d'une clause FROM simple  
 L'exemple suivant récupère les colonnes `TerritoryID` et `Name` à partir de la table `SalesTerritory` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>B. Utilisation des indicateurs d'optimiseur TABLOCK et HOLDLOCK  
 La transaction partielle suivante illustre le mode de placement d'un verrou de table partagé explicite sur `Employee` et le mode de lecture de l'index. Le verrou est conservé pendant toute la transaction.  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>C. Utilisation de la syntaxe SQL-92 CROSS JOIN  
 L'exemple suivant retourne le produit croisé des deux tables `Employee` et `Department` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La liste de toutes les combinaisons possibles des lignes `BusinessEntityID` et `Department` est retournée.  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>D. Utilisation de la syntaxe SQL-92 FULL OUTER JOIN  
 L'exemple suivant retourne le nom de produit et les commandes correspondantes dans la table `SalesOrderDetail` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Il retourne également les commandes dont les produits ne sont pas répertoriés dans la table `Product`, ainsi que tous les produits dont la commande est différente de celle répertoriée dans la table `Product`.  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>E. Utilisation de la syntaxe SQL-92 LEFT OUTER JOIN  
 L'exemple suivant joint deux tables sur `ProductID` et conserve les lignes sans correspondance de la table de gauche. La table `Product` correspond à la table `SalesOrderDetail` sur les colonnes `ProductID` de chaque table. Tous les produits, qu'ils soient commandés ou non, apparaissent dans le jeu de résultats.  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>F. Utilisation de la syntaxe SQL-92 INNER JOIN  
 L'exemple suivant retourne tous les noms de produits et tous les ID de commandes.  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>G. Utilisation de la syntaxe SQL-92 RIGHT OUTER JOIN  
 L'exemple suivant joint deux tables sur `TerritoryID` et conserve les lignes sans correspondance de la table de droite. La table `SalesTerritory` correspond à la table `SalesPerson` sur la colonne `TerritoryID` de chaque table. Tous les vendeurs apparaissent dans le jeu de résultats, qu'un secteur leur ait été attribué ou non.  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>H. Utilisation des indicateurs de jointure HASH et MERGE  
 L'exemple suivant effectue une jointure sur trois tables parmi les tables `Product`, `ProductVendor` et `Vendor` afin de produire une liste de produits et de leurs fournisseurs. L'optimiseur de requête joint `Product` et `ProductVendor` (`p` et `pv`) à l'aide d'une jointure MERGE. Ensuite, les résultats de la jointure MERGE `Product` et `ProductVendor` (`p` et `pv`) sont joints à l'aide de HASH à la table `Vendor` pour produire (`p` et `pv`) et `v`.  
  
> [!IMPORTANT]  
>  Lorsqu'un indicateur de jointure est spécifié, le mot clé INNER n'est plus facultatif et doit être déclaré de manière explicite pour l'exécution d'une jointure INNER JOIN.  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>I. Utilisation d'une table dérivée  
 L'exemple suivant se sert d'une table dérivée, d'une instruction `SELECT` après la clause `FROM`, pour retourner les prénoms et noms de tous les employés ainsi que leur ville de résidence.  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>J. Utilisation de TABLESAMPLE pour lire des données à partir d'un exemple de lignes dans une table  
 L'exemple suivant utilise `TABLESAMPLE` dans la clause `FROM` pour retourner environ `10` % de toutes les lignes de la table `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>K. Utilisation de l'opérateur APPLY  
L’exemple suivant part du principe que les tables et la fonction table ci-après existent dans la base de données :  

|Nom d’objet|Noms des colonnes|      
|---|---|   
|Departments|DeptID, DivisionID, DeptName, DeptMgrID|      
|EmpMgr|MgrID, EmpID|     
|Employees|EmpID, EmpLastName, EmpFirstName, EmpSalary|  
|GetReports(MgrID)|EmpID, EmpLastName, EmpSalary|     
  
La fonction table `GetReports` retourne la liste de tous les employés qui rendent directement ou indirectement compte au `MgrID` spécifié.  
  
L'exemple utilise `APPLY` pour retourner tous les services et tous les employés qui en font partie. Si un service particulier est dépourvu d'employés, aucune ligne n'est retournée pour celui-ci.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
Si vous souhaitez que la requête génère des lignes pour ces services sans employés, ce qui produira des valeurs NULL pour les colonnes `EmpID`, `EmpLastName` et `EmpSalary`, utilisez `OUTER APPLY` à la place.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>L. Utilisation de l'opérateur CROSS APPLY  
L'exemple suivant récupère un instantané de tous les plans de requête résidant dans la mémoire cache des plans, en interrogeant la vue de gestion dynamique `sys.dm_exec_cached_plans` pour récupérer les descripteurs de plan de tous les plans de requête dans le cache. L'opérateur `CROSS APPLY` est spécifié pour transmettre les descripteurs de plan à `sys.dm_exec_query_plan`. La sortie du plan d'exécution de requêtes XML pour chaque plan actuellement dans la mémoire cache des plans se trouve dans la colonne `query_plan` de la table retournée.  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>M. Utilisation de FOR SYSTEM_TIME  
  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
L’exemple suivant utilise l’argument FOR SYSTEM_TIME AS OF date_time_literal_or_variable pour retourner les lignes de la table qui étaient actives à la date du 1er janvier 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
L’exemple suivant utilise l’argument FOR SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable pour retourner toutes les lignes qui étaient actives pendant la période entre le 1er janvier 2013 et le 1er janvier 2014, limite supérieure exclue.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 L’exemple suivant utilise l’argument FOR SYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variable pour retourner toutes les lignes qui étaient actives pendant la période entre le 1er janvier 2013 et le 1er janvier 2014, limite supérieure incluse.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 L’exemple suivant utilise l’argument FOR SYSTEM_TIME CONTAINED IN ( date_time_literal_or_variable, date_time_literal_or_variable ) pour retourner toutes les lignes qui étaient ouvertes et fermées pendant la période entre le 1er janvier 2013 et le 1er janvier 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 L’exemple suivant utilise une variable, plutôt qu’un littéral, pour fournir les valeurs de date limites pour la requête.  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>N. Utilisation de la syntaxe INNER JOIN  
 L’exemple suivant retourne les colonnes `SalesOrderNumber`, `ProductKey` et `EnglishProductName` des tables `FactInternetSales` et `DimProduct` qui ont la même clé de jointure (`ProductKey`). Les colonnes `SalesOrderNumber` et `EnglishProductName` existent dans l’une des tables uniquement. Vous n’avez donc pas besoin de spécifier l’alias de table avec ces colonnes comme le montre l’exemple ; ces alias sont inclus pour une meilleure lisibilité. Le mot **AS** avant un alias n’est pas obligatoire, mais il est recommandé pour garantir une meilleure lisibilité et le respect de la norme ANSI.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Étant donné que le mot clé `INNER` n’est pas obligatoire pour les jointures internes, cette requête peut également être écrite sous la forme suivante :  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 Vous pouvez aussi utiliser une clause `WHERE` avec cette requête pour limiter les résultats retournés. Cet exemple limite les résultats aux valeurs de `SalesOrderNumber` supérieures à « SO5000 » :  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>O. Utilisation de la syntaxe LEFT OUTER JOIN et RIGHT OUTER JOIN  
 L’exemple suivant joint les tables `FactInternetSales` et `DimProduct` sur les colonnes `ProductKey`. La syntaxe de jointure externe gauche (LEFT OUTER JOIN) conserve les lignes sans correspondance de la table de gauche (`FactInternetSales`). Comme la table `FactInternetSales` ne contient pas de valeurs `ProductKey` qui ne correspondent pas dans la table `DimProduct`, cette requête retourne les mêmes lignes que dans le premier exemple de jointure interne ci-dessus.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Cette requête peut aussi s’écrire sans le mot clé `OUTER`.  
  
 La syntaxe de jointure externe droite (RIGHT OUTER JOIN) conserve les lignes sans correspondance de la table de droite. L’exemple suivant retourne les mêmes lignes que l’exemple de jointure externe gauche ci-dessus.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 La requête suivante utilise la table `DimSalesTerritory` en tant que table de gauche dans une jointure externe gauche. Elle récupère les valeurs `SalesOrderNumber` à partir de la table `FactInternetSales`. Si aucune commande n’est enregistrée pour un `SalesTerritoryKey` spécifique, la requête retourne une valeur `SalesOrderNumber` NULL pour cette ligne. Cette requête est triée sur la colonne `SalesOrderNumber` pour que les éventuelles valeurs NULL de cette colonne s’affichent en premier dans les résultats.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Cette requête peut être réécrite avec une jointure externe droite pour récupérer les mêmes résultats :  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>P. Utilisation de la syntaxe FULL OUTER JOIN  
 L’exemple suivant montre une jointure externe entière, qui retourne toutes les lignes des deux tables jointes, mais qui retourne NULL pour les valeurs sans correspondance dans l’autre table.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Cette requête peut aussi s’écrire sans le mot clé `OUTER`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>Q. Utilisation de la syntaxe CROSS JOIN  
 L’exemple suivant retourne le produit croisé des tables `FactInternetSales` et `DimSalesTerritory`. La liste de toutes les combinaisons possibles de `SalesOrderNumber` et `SalesTerritoryKey` est retournée. Notez l’absence de la clause `ON` dans la requête de jointure croisée.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>R. Utilisation d'une table dérivée  
 L’exemple suivant utilise une table dérivée (une instruction `SELECT` après la clause `FROM`) pour retourner les colonnes `CustomerKey` et `LastName` de tous les clients dans la table `DimCustomer` dont la valeur `BirthDate` est postérieure au 1er janvier 1970 et dont le nom de famille est « Smith ».  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>S. Exemple d’indicateur de jointure REDUCE  
 L’exemple suivant utilise l’indicateur de jointure `REDUCE` pour modifier le traitement de la table dérivée dans la requête. Quand vous utilisez l’indicateur de jointure `REDUCE` dans cette requête, la valeur `fis.ProductKey` est projetée, répliquée et rendue unique, puis jointe à `DimProduct` lors de la lecture aléatoire de `DimProduct` sur `ProductKey`. La table dérivée qui en résulte est distribuée sur `fis.ProductKey`.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. Exemple d’indicateur de jointure REPLICATE  
 L’exemple suivant montre la même requête que l’exemple précédent, à ceci près qu’un indicateur de jointure `REPLICATE` est utilisé à la place de l’indicateur de jointure `REDUCE`. Quand vous utilisez l’indicateur `REPLICATE`, les valeurs dans la colonne (jointure) `ProductKey` de la table `FactInternetSales` sont répliquées vers tous les nœuds. La table `DimProduct` est jointe à la version répliquée de ces valeurs.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>U. Utilisation de l’indicateur REDISTRIBUTE pour garantir un déplacement aléatoire dans une jointure de distribution incompatible  
 La requête suivante utilise l’indicateur de requête REDISTRIBUTE sur une jointure de distribution incompatible. Cela garantit que l’optimiseur de requête utilise un déplacement aléatoire dans le plan de requête. Cela empêche également que le plan de requête utilise un déplacement par diffusion pour déplacer une table distribuée vers une table répliquée.  
  
 Dans l’exemple suivant, l’indicateur REDISTRIBUTE force l’utilisation d’un déplacement aléatoire sur la table FactInternetSales, car ProductKey est la colonne de distribution pour DimProduct et n’est pas la colonne de distribution pour FactInternetSales.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. Utilisation de TABLESAMPLE pour lire des données à partir d'un exemple de lignes dans une table  
 L'exemple suivant utilise `TABLESAMPLE` dans la clause `FROM` pour retourner environ `10` % de toutes les lignes de la table `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>Voir aussi  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)

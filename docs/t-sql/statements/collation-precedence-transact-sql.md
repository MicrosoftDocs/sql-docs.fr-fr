---
description: Priorité de classement
title: Priorité de classement | Microsoft Docs
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
- coercible-default collation label
- precedence [SQL Server], collations
- collation sensitive
- collations [SQL Server], precedence
- explicit collation label [SQL Server]
- implicit collation label
- no-collation label
- collation insensitive
- operators [Transact-SQL], collations
- collation labels
- collation coercion rules
- rules [SQL Server], collations
ms.assetid: 58c4e64b-5634-4c29-aa22-33193282dd27
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c06bf7aa0ec45150f657e5822d1d5b1387ef18df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098463"
---
# <a name="collation-precedence"></a>Priorité de classement
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La priorité de classement, également appelée règles de contraintes de classements, détermine :  
  
-   le classement du résultat final d'une expression évaluée en une chaîne de caractères ;  
  
-   le classement utilisé par certains opérateurs, tels que LIKE et IN, qui utilisent des entrées de chaînes de caractères, mais ne renvoient pas une chaîne de caractères.  
  
Les règles de priorité de classement s’appliquent uniquement aux types de données de chaînes de caractères : **char**, **varchar**, **text**, **nchar**, **nvarchar** et **ntext**. Les objets comportant d'autres types de données n'interviennent pas dans les évaluations de classement.  
  
## <a name="collation-labels"></a>Étiquettes de classement  
Le tableau suivant répertorie et décrit les quatre catégories de classement des objets. Le nom de chaque catégorie est identifié par une étiquette de classement.  
  
|Étiquette de classement|Types d’objets|  
|---------------------|----------------------|  
|Contrainte par défaut|Toute variable de chaîne de caractères [!INCLUDE[tsql](../../includes/tsql-md.md)], tout paramètre, littéral, résultat d'une fonction de catalogue intégrée, ou toute fonction intégrée qui ne prend pas les entrées de type chaîne, mais produit un résultat de type chaîne.<br /><br /> Si l'objet est déclaré dans une fonction définie par l'utilisateur, une procédure stockée ou un déclencheur, il se voit attribuer le classement par défaut de la base de données dans lequel la fonction, la procédure stockée ou le déclencheur est créé. Si l'objet est déclaré dans un traitement, le classement par défaut de la base de données actuelle lui est attribué pour la connexion.|  
|X implicite|Référence de colonne. Le classement de l'expression (X) provient du classement défini pour la colonne dans la table ou la vue.<br /><br /> Même si la colonne est explicitement affectée à un classement par une clause COLLATE de l'instruction CREATE TABLE ou CREATE VIEW, la référence de la colonne est classée comme étant implicite.|  
|X explicite|Expression explicitement convertie en un classement spécifique (X) à l'aide d'une clause COLLATE dans l'expression.|  
|Sans classement|Indique que la valeur d'une expression est le résultat d'une opération entre deux chaînes dont l'étiquette de classement implicite génère des classements conflictuels. Le résultat de l'expression est défini comme étant dépourvu d'un classement.|  
  
## <a name="collation-rules"></a>Règles de classement  
L'étiquette de classement d'une expression simple qui fait référence à un seul objet de chaîne de caractère représente l'étiquette de classement de l'objet en référence.  
  
L'étiquette de classement d'une expression complexe qui fait référence à deux expressions d'opérandes dotées de la même étiquette de classement constitue l'étiquette de classement des expressions d'opérandes.  
  
L'étiquette de classement du résultat final d'une expression complexe qui fait référence à deux expressions d'opérandes comportant des classements différents obéit aux règles suivantes :  
  
-   L'étiquette Explicite est prioritaire par rapport à l'étiquette Implicite. L'étiquette Implicite est prioritaire par rapport à l'étiquette Contrainte par défaut.  
  
     Explicite > Implicite > Contrainte par défaut  
  
-   La combinaison de deux expressions explicites affectées à des classements différents génère une erreur.  
  
     X explicite + Y explicite = Erreur  
  
-   La combinaison de deux expressions implicites affectées à des classements différents produit un résultat dépourvu de classement :  
  
     X implicite + Y implicite = Sans classement  
  
-   La combinaison d'une expression sans classement avec une expression d'une étiquette, à l'exception d'un classement explicite (voir règle suivante), produit un résultat comportant l'étiquette Sans classement (No-collation) :  
  
     Sans classement + autre classement = Sans classement  
  
-   La combinaison d'une expression sans classement avec une expression dotée d'un classement explicite produit une expression affectée d'une étiquette explicite :  
  
     Sans classement + X explicite = Explicite  
  
Le tableau suivant récapitule les différentes règles.  
  
|Étiquette de contrainte d'opérande|X explicite|X implicite|Contrainte par défaut|Sans classement|  
|----------------------------|----------------|----------------|------------------------|-------------------|  
|**Y explicite**|Génère une erreur|Le résultat est Y explicite|Le résultat est Y explicite|Le résultat est Y explicite|  
|**Y implicite**|Le résultat est X explicite|Le résultat est Sans classement|Le résultat est Y implicite|Le résultat est Sans classement|  
|**Contrainte par défaut**|Le résultat est X explicite|Le résultat est X implicite|Le résultat est Contrainte par défaut|Le résultat est Sans classement|  
|**Sans classement**|Le résultat est X explicite|Le résultat est Sans classement|Le résultat est Sans classement|Le résultat est Sans classement|  
  
Les règles supplémentaires suivantes s'appliquent également à la priorité de classement :  
  
-   Une expression explicite ne peut pas avoir plusieurs clauses COLLATE. Par exemple, la clause `WHERE` suivante n'est pas valide car une clause `COLLATE` est spécifiée pour une expression qui est déjà explicite :  
  
     `WHERE ColumnA = ( 'abc' COLLATE French_CI_AS) COLLATE French_CS_AS`  
  
-   Les conversions de pages de codes pour les types de données **text** ne sont pas autorisées. Vous ne pouvez pas effectuer un cast d’une expression de type **texte** d’un classement à l’autre si les pages de codes de ces derniers sont différentes. L'opérateur d'affectation ne peut pas attribuer de valeurs si le classement de l'opérande texte de droite utilise une page de codes différente de l'opérande texte de gauche.  
  
La priorité de classement est déterminée après la conversion du type de données. L'opérande qui détermine le classement résultant peut être différent de l'opérande qui fournit le type de données du résultat final. Par exemple, étudiez le traitement ci-dessous :  
  
```sql  
CREATE TABLE TestTab  
   (PrimaryKey int PRIMARY KEY,  
    CharCol char(10) COLLATE French_CI_AS  
   )  
  
SELECT *  
FROM TestTab  
WHERE CharCol LIKE N'abc'  
```  
  
Le type de données Unicode de l'expression simple `N'abc'` est prioritaire. Par conséquent, l'expression résultante a le type de données Unicode affecté à `N'abc'`. Cependant, l'expression `CharCol` a l'étiquette de classement Implicite, alors que `N'abc'` est associé à une étiquette de contrainte inférieure de Contrainte par défaut. Le classement `French_CI_AS` de `CharCol` sera dès lors utilisé.  
  
### <a name="examples-of-collation-rules"></a>Exemples de règles de classement  
 Les exemples suivants illustrent le fonctionnement des règles de classement. Pour exécuter les exemples, créez la table test suivante.  
  
```sql  
USE tempdb;  
GO  
  
CREATE TABLE TestTab (  
   id int,   
   GreekCol nvarchar(10) collate greek_ci_as,   
   LatinCol nvarchar(10) collate latin1_general_cs_as  
   )  
INSERT TestTab VALUES (1, N'A', N'a');  
GO  
```  
  
#### <a name="collation-conflict-and-error"></a>Conflit et erreur de classement  
 Dans la requête suivante, le prédicat présente un conflit de classement et génère une erreur.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 448, Level 16, State 9, Line 2  
Cannot resolve collation conflict between 'Latin1_General_CS_AS' and 'Greek_CI_AS' in equal to operation.  
```  
  
#### <a name="explicit-label-vs-implicit-label"></a>Étiquettes Explicite et Implicite  
 Dans la requête suivante, le prédicat est analysé dans le classement `greek_ci_as`, car l'expression de droite comporte l'étiquette Explicite. Celle-ci est prioritaire par rapport à l'étiquette Implicite de l'expression de gauche.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol COLLATE greek_ci_as;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
id          GreekCol             LatinCol  
----------- -------------------- --------------------  
          1 A                    a  
  
(1 row affected)  
```  
  
#### <a name="no-collation-labels"></a>Étiquette sans classement  
Dans les requêtes suivantes, les expressions `CASE` ont l'étiquette Sans classement. Elles ne peuvent donc pas apparaître dans la liste de sélection, ni être traitées par les opérateurs qui respectent le classement. Toutefois, les expressions peuvent être traitées par les opérateurs qui ne respectent pas le classement.  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END)   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 451, Level 16, State 1, Line 1  
Cannot resolve collation conflict for column 1 in SELECT statement.  
```  
  
```sql  
SELECT PATINDEX((CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END), 'a')  
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 446, Level 16, State 9, Server LEIH2, Line 1  
Cannot resolve collation conflict for patindex operation.  
```  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END) COLLATE Latin1_General_CI_AS   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------  
a  
  
(1 row affected)  
```  
  
## <a name="collation-sensitive-and-collation-insensitive"></a>Sensibilité au classement  
Les opérateurs et les fonctions respectent ou non le classement.  
  
Respect du classement  
Le respect du classement signifie que la spécification d'un opérande sans classement génère une erreur de compilation. Le résultat de l'expression ne peut être de type sans classement.  
  
Non-respect du classement  
Le non-respect du classement signifie que les opérandes et le résultat peuvent être sans classement.  
  
### <a name="operators-and-collation"></a>Opérateurs et classement  
Les opérateurs de comparaison et les opérateurs MAX, MIN, BETWEEN, LIKE et IN respectent le classement. La chaîne utilisée par les opérateurs se voit attribuer l'étiquette de classement de l'opérande comportant le degré de priorité le plus élevé. L’instruction UNION respecte également le classement. Tous les opérandes de chaînes et le résultat final se voient attribuer le classement de l’opérande doté du degré de priorité le plus élevé. La priorité de classement de l’opérande UNION et du résultat est évaluée par colonne.  
  
L'opérateur d'affectation ne respecte pas le classement et l'expression de droite est affectée au classement de gauche.  
  
L'opérateur de concaténation de chaînes respecte le classement, les deux opérandes de chaînes et le résultat se voient attribuer l'étiquette de classement de l'opérande comportant la priorité de classement la plus élevée. Les instructions UNION ALL et CASE ne respectent pas le classement. Tous les opérandes de chaînes et résultats finaux se voient attribuer l’étiquette de classement de l’opérande doté du degré de priorité le plus élevé. La priorité de classement des opérandes UNION ALL et du résultat est évaluée par colonne.  
  
### <a name="functions-and-collation"></a>Fonctions et classement  
Les fonctions CAST, CONVERT et COLLATE respectent le classement pour les types de données **char**, **varchar** et **text**. Si l'entrée et la sortie des fonctions CAST et CONVERT sont des chaînes de caractères, la chaîne résultante a l'étiquette de classement de la chaîne d'entrée. Si l'entrée n'est pas une chaîne de caractères, la chaîne résultante est une contrainte par défaut et se voit attribuer le classement de la base de données actuelle pour la connexion, ou de celle contenant la fonction définie par l'utilisateur, la procédure stockée ou le déclencheur dans lequel la fonction CAST ou CONVERT est référencée.  
  
 Pour les fonctions intégrées qui renvoient une chaîne, mais qui n'acceptent pas une chaîne en entrée, la chaîne résultante est une contrainte par défaut et se voit attribuer le classement de la base de données actuelle ou celui de la base de données contenant la fonction définie par l'utilisateur, la procédure stockée ou le déclencheur dans lequel la fonction est référencée.  
  
 Les fonctions suivantes respectent le classement et leurs chaînes résultantes ont l'étiquette de classement de la chaîne d'entrée :  

:::row:::
    :::column:::
        CHARINDEX
    :::column-end:::
    :::column:::
        REPLACE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DIFFERENCE
    :::column-end:::
    :::column:::
        REVERSE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ISNUMERIC
    :::column-end:::
    :::column:::
        RIGHT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEFT
    :::column-end:::
    :::column:::
        SOUNDEX
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEN
    :::column-end:::
    :::column:::
        STUFF
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LOWER
    :::column-end:::
    :::column:::
        SUBSTRING
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        PATINDEX
    :::column-end:::
    :::column:::
        UPPER
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Voir aussi  
 [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md)   
 [Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  

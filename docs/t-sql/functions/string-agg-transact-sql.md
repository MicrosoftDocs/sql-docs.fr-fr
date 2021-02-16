---
description: STRING_AGG (Transact-SQL)
title: STRING_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- STRING_AGG
- STRING_AGG_TSQL
helpviewer_keywords:
- STRING_AGG function
ms.assetid: 8860ef3f-142f-4cca-aa64-87a123e91206
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6c0eaef945ae4234987e6849f305d7b26cbb9374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346606"
---
# <a name="string_agg-transact-sql"></a>STRING_AGG (Transact-SQL)

<!--[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]-->

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Concatène les valeurs des expressions de chaîne et place les valeurs de séparateur entre elles. Le séparateur n’est pas ajouté à la fin de la chaîne. 
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=   
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )   
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type. Les expressions sont converties en types `NVARCHAR` ou `VARCHAR` durant la concaténation. Les types autres que chaîne sont convertis en type `NVARCHAR`.

*separator*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de type `NVARCHAR` ou `VARCHAR` qui est utilisée comme séparateur pour les chaînes concaténées. Ce peut être un littéral ou une variable. 

<order_clause>   
Spécifiez éventuellement l’ordre des résultats concaténés à l’aide de la clause `WITHIN GROUP` :

```syntaxsql
WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```   
<order_by_expression_list>   
 
  Liste d’[expressions](../../t-sql/language-elements/expressions-transact-sql.md) non constantes qui peuvent être utilisées pour le tri des résultats. Un seul `order_by_expression` est autorisé par requête. L’ordre de tri par défaut est croissant.   
  
## <a name="return-types"></a>Types de retour

Le type de retour dépend du premier argument (expression). Si l’argument d’entrée est de type chaîne (`NVARCHAR`, `VARCHAR`), le type de résultat sera identique au type d’entrée. Le tableau suivant répertorie les conversions automatiques :  

|Type d’expression d’entrée |Résultats | 
|-------|-------|
|NVARCHAR(MAX) |NVARCHAR(MAX) |
|VARCHAR(MAX) |VARCHAR(MAX) |
|NVARCHAR(1...4000) |NVARCHAR(4000) |
|VARCHAR(1...8000) |VARCHAR(8000) |
|int, bigint, smallint, tinyint, numeric, float, real, bit, decimal, smallmoney, money, datetime, datetime2, |NVARCHAR(4000) |

## <a name="remarks"></a>Remarques

`STRING_AGG` est une fonction d’agrégation qui accepte toutes les expressions à partir des lignes et les concatène en une seule chaîne. Les valeurs d’expression sont implicitement converties en types chaîne, puis concaténées. La conversion implicite en chaînes respecte les règles existantes de conversion de type de données. Pour plus d’informations sur les conversions de type de données, consultez [CAST et CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md). 

Si l’expression d’entrée est de type `VARCHAR`, le séparateur ne peut pas être de type `NVARCHAR`. 

Les valeurs NULL sont ignorées et le séparateur correspondant n’est pas ajouté. Pour renvoyer un espace réservé pour les valeurs NULL, utilisez la fonction `ISNULL` comme illustré dans l’exemple B.

`STRING_AGG` est disponible dans n’importe quel niveau de compatibilité.

## <a name="examples"></a>Exemples

### <a name="a-generate-list-of-names-separated-in-new-lines"></a>R. Générer une liste de noms séparés sur plusieurs lignes

L’exemple suivant génère une liste de noms dans une cellule de résultat unique, séparés par des retours chariot.
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG (CONVERT(NVARCHAR(max),FirstName), CHAR(13)) AS csv 
FROM Person.Person;  
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|csv | 
|--- |
|Syed <br />Catherine <br />Kim <br />Kim <br />Kim <br />Hazem <br />... | 

Les valeurs `NULL` trouvées dans les cellules `name` ne sont pas renvoyées dans le résultat.   

> [!NOTE]  
> Si vous utilisez l’éditeur de requête [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’option **Résultats dans des grilles** ne peut pas implémenter le retour chariot. Basculez vers **Results to Text** pour afficher correctement le jeu de résultats.       
> Les résultats dans du texte sont tronqués à 256 caractères par défaut. Pour augmenter cette limite, modifiez l’option **Nombre maximal de caractères affichés dans chaque colonne**.

### <a name="b-generate-list-of-names-separated-with-comma-without-null-values"></a>B. Générer une liste de noms séparés par des virgules sans valeurs NULL

L’exemple suivant remplace les valeurs NULL par « N/A » et renvoie les noms séparés par des virgules dans une cellule de résultat unique.  
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), ISNULL(FirstName,'N/A')), ',') AS csv 
FROM Person.Person; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Les résultats sont affichés tronqués.

|csv | 
|--- |
|Syed,Catherine,Kim,Kim,Kim,Hazem,Sam,Humberto,Gustavo,Pilar,Pilar, ...|  

### <a name="c-generate-comma-separated-values"></a>C. Générer des valeurs séparées par des virgules

```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), CONCAT(FirstName, ' ', LastName, '(', ModifiedDate, ')')), CHAR(13)) AS names 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Les résultats sont affichés tronqués.

|noms |
|--- |
|Ken Sánchez (8 février 2003 12h00) <br />Terri Duffy (24 février 2002 12h00) <br />Roberto Tamburello (5 décembre 2001 12h00) <br />Rob Walters (29 décembre 2001 12h00) <br />... |

> [!NOTE]  
> Si vous utilisez l’éditeur de requête de Management Studio, l’option **Results to Grid** ne peut pas implémenter le retour chariot. Basculez vers **Results to Text** pour afficher correctement le jeu de résultats.

### <a name="d-return-news-articles-with-related-tags"></a>D. Retourner des articles d’actualité avec les balises associées

Les articles et leurs balises sont séparés dans différentes tables. Le développeur souhaite renvoyer une ligne pour chaque article avec toutes les balises associées. À l’aide de la requête suivante :

```sql
SELECT a.articleId, title, STRING_AGG (tag, ',') as tags
FROM dbo.Article AS a
LEFT JOIN dbo.ArticleTag AS t
    ON a.ArticleId = t.ArticleId
GROUP BY a.articleId, title;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|articleId |title |tags |
|--- |--- |--- |
|172 |Les sondages prévoient des résultats serrés aux élections |politique,sondages,conseil municipal |
|176 |La nouvelle autoroute devrait réduire les embouteillages |NULL |
|177 |Les chiens restent plus populaires que les chats |sondages,animaux|

> [!NOTE]
> La clause `GROUP BY` est obligatoire si la fonction `STRING_AGG` n’est pas le seul élément de la liste `SELECT`.

### <a name="e-generate-list-of-emails-per-towns"></a>E. Générer une liste d’adresses e-mail par ville

La requête suivante recherche les adresses e-mail des employés et les regroupe par ville :

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Les résultats sont affichés tronqués.

|City |emails |
|--- |--- |
|Ballard|paige28@adventure-works.com;joshua24@adventure-works.com;javier12@adventure-works.com;...|
|Baltimore|gilbert9@adventure-works.com|
|Barstow|kristen4@adventure-works.com|
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com|
|Baytown|kelvin15@adventure-works.com|
|Beaverton|billy6@adventure-works.com;dalton35@adventure-works.com;lawrence1@adventure-works.com;...|
|Bell Gardens|christy8@adventure-works.com
|Bellevue|min0@adventure-works.com;gigi0@adventure-works.com;terry18@adventure-works.com;...|
|Bellflower|philip0@adventure-works.com;emma34@adventure-works.com;jorge8@adventure-works.com;...|
|Bellingham|christopher23@adventure-works.com;frederick7@adventure-works.com;omar0@adventure-works.com;...|

Les adresses e-mail renvoyées dans la colonne emails peuvent être utilisées directement pour envoyer des e-mails à un groupe de personnes travaillant dans certaines villes particulières. 

### <a name="f-generate-a-sorted-list-of-emails-per-towns"></a>F. Générer une liste ordonnée d’adresses e-mail par ville   
Similaire à l’exemple précédent, la requête suivante recherche les adresses e-mail des employés, les regroupe par ville, puis les trie par ordre alphabétique :   

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') WITHIN GROUP (ORDER BY EmailAddress ASC) AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Les résultats sont affichés tronqués.

|City |emails |
|--- |--- |
|Barstow|kristen4@adventure-works.com
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com
|Braintree|mindy20@adventure-works.com
|Bell Gardens|christy8@adventure-works.com
|Byron|louis37@adventure-works.com
|Bordeaux|ranjit0@adventure-works.com
|Carnation|don0@adventure-works.com;douglas0@adventure-works.com;george0@adventure-works.com;...|
|Boulogne-Billancourt|allen12@adventure-works.com;bethany15@adventure-works.com;carl5@adventure-works.com;...|
|Berkshire|barbara41@adventure-works.com;brenda4@adventure-works.com;carrie14@adventure-works.com;...|
|Berks|adriana6@adventure-works.com;alisha13@adventure-works.com;arthur19@adventure-works.com;...|

## <a name="see-also"></a>Voir aussi
 
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  


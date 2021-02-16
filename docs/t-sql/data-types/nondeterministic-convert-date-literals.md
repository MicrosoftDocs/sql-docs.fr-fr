---
description: Conversion non déterministe de chaînes de date littérale en valeurs DATE
title: Conversion non déterministe des littéraux de date | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 45540a538982f75033f35ad759782317983dc958
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071813"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>Conversion non déterministe de chaînes de date littérale en valeurs DATE

Soyez prudent quand vous autorisez la conversion de vos chaînes de type CHARACTER en types de données DATE. La raison en est que ces conversions sont souvent _non déterministes_.

Vous contrôlez ces conversions non déterministes en prenant en considération les paramètres [SET LANGUAGE](../statements/set-language-transact-sql.md) et [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md).



## <a name="set-language-example-month-name-in-polish"></a>Exemple pour SET LANGUAGE : nom du mois en polonais

- `SET LANGUAGE Polish;`

Une chaîne de caractères peut être le nom d’un mois. Mais quel est le nom en anglais , en polonais, en croate ou dans une autre langue ? Et la session de l’utilisateur sera-t-elle définie pour la langue (LANGUAGE) correspondante appropriée ?

Par exemple, considérez le mot _listopad_, qui est le nom d’un mois. Le mois qu’il désigne dépend de la langue que le système SQL estime être utilisée :
- Si c’est du polonais, _listopad_ est traduit en mois 11 (_November_ en anglais).
- Si c’est du croate, _listopad_ est traduit en mois 10 (_October_ en anglais).

#### <a name="code-example-of-set-language"></a>Exemple de code de SET LANGUAGE

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
***/
```



## <a name="set-dateformat-example"></a>Exemple SET DATEFORMAT

- `SET DATEFORMAT dmy;`

Le format **dmy** (jma) précédent indique qu’un exemple de chaîne de date « 01-03-2018 » serait interprétée comme signifiant _le premier jour du mois de mars de l’année 2018_.

Si au lieu de cela, le format **mdy** a été spécifié, la même chaîne « 01-03-2018 » signifierait _le troisième jour du mois de janvier 2018_.

Et si le format **ymd** (amj) a été spécifié, aucune garantie ne peut être donnée quant au résultat. La valeur numérique « 2018 » est trop grande pour être un jour.
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>Pays spécifiques

Au Japon et en Chine, le format DATEFORMAT **ymd** (amj) est utilisé. Les parties du format sont dans un ordre significatif, qui va de la plus grande unité à la plus petite. Ce format est donc trié correctement. Ce format est considéré comme étant le format _international_. Il est international, car les quatre chiffres de l’année ne sont pas ambigus, et qu’aucun pays sur terre n’utilise le format archaïque **ydm** (ajm).

Dans d’autres pays, comme l’Allemagne et la France, le format DATEFORMAT est **dmy** (jma), ce qui signifie **« jj-mm-aaaa »**. Le format **dmy** (jma) ne se trie pas correctement, mais c’est une séquence significative allant de la plus petite unité à la plus grande.

Les États-Unis et les États fédérés de Micronésie sont les seuls pays à utiliser **mdy** (mja), qui ne se trie pas. La séquence mixte du format correspond à un modèle de discours parlé dans les dates énoncées verbalement.

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>Exemple de code de SET DATEFORMAT : *mdy* (mja) et *dmy* (jma)

L’exemple de code Transact-SQL suivant utilise la même chaîne de caractères de date avec trois valeurs différentes pour DATEFORMAT. Une exécution du code produit la sortie présentée dans le commentaire :

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
***/
```

Dans l’exemple de code précédent, le dernier exemple montre une non-correspondance entre le format **ymd** (amj) et la chaîne d’entrée. Le troisième nœud de la chaîne d’entrée représente une valeur numérique qui est trop grande pour être un jour. Microsoft ne garantit pas la valeur de la sortie dans le cas de ces non-correspondances.

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT offre des codes explicites pour le contrôle _déterministe_ des formats de date

Notre article documentant CAST et CONVERT liste les codes explicites que vous pouvez utiliser avec la fonction CONVERT pour contrôler _de façon déterministe_ les conversions de date. L’article concerné a chaque mois un nombre de visualisations de page parmi les plus élevés.

- [CAST et CONVERT (Transact-SQL) : styles de date et d’heure](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST et CONVERT (Transact-SQL) : certaines conversions de date/heure sont non déterministes](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>Niveau de compatibilité 90 et supérieur

Dans SQL Server 2000, le niveau de compatibilité était de 80. Pour les paramètres de niveau 80 ou inférieurs, les conversions de date implicites étaient déterministes.

À compter de SQL Server 2005 et de son niveau de compatibilité de 90, les conversions de date implicites sont devenues non déterministes. Les conversions de dates sont devenues dépendantes de SET LANGUAGE et SET DATEFORMAT, à partir du niveau 90.

#### <a name="unicode"></a>Unicode

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
La conversion de données caractères non-Unicode entre les classements est également considérée comme non déterministe.



## <a name="see-also"></a>Voir aussi

- [Définir une langue de session](../../relational-databases/collations/set-a-session-language.md)
- [Types de données et fonctions de date et d'heure (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->


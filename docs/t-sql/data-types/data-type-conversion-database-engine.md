---
description: Conversion de types de données (moteur de base de données)
title: Conversion de types de données (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- converting data types [SQL Server]
- CONVERT function
- data types [SQL Server], converting
- implicit data type conversions
- explicit data type conversions [SQL Server]
- converting data types [SQL Server], about converting data types
ms.assetid: ffacf45e-a488-48d0-9bb0-dcc7fd365299
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d88c7b479f6a4d9cbd7bca4091690c79b3ba564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194998"
---
# <a name="data-type-conversion-database-engine"></a>Conversion de types de données (moteur de base de données)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les types de données peuvent être convertis dans les scénarios suivants :
-   Lorsque les données d'un objet sont déplacées dans les données d'un autre objet, sont comparées ou combinées à ces dernières, les données d'un objet doivent être converties du type de données d'un objet en type de données de l'autre.  
-   Quand les données d’une colonne de résultats, d’un code de retour ou d’un paramètre de sortie [!INCLUDE[tsql](../../includes/tsql-md.md)] sont déplacées dans une variable de programme, elles doivent être converties du type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers le type de données de la variable.  
  
Lors de la conversion entre une variable d'application et une colonne du jeu de résultats, un code de retour, un paramètre ou un marqueur de paramètre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les conversions de types de données acceptées sont définies par l'API de base de données.
  
## <a name="implicit-and-explicit-conversion"></a>Conversions implicites et explicites
Les types de données peuvent être convertis implicitement ou explicitement.
  
Les conversions implicites sont invisibles pour l'utilisateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit automatiquement les données d'un type de données en un autre. Par exemple, quand un **smallint** est comparé à un **int**, le **smallint** est implicitement converti en **int** avant de poursuivre la comparaison.
  
**GETDATE()** convertit implicitement la date en style 0. **SYSDATETIME()** convertit implicitement la date en style 21.
  
Les conversions explicites utilisent les fonctions CAST ou CONVERT.
  
Les fonctions [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) convertissent une valeur (une variable locale, une colonne ou une autre expression) d’un type de données en un autre. Par exemple, la fonction `CAST` convertit la valeur numérique de `$157.27` en une chaîne de caractères `'157.27'` :
  
```sql
CAST ( $157.27 AS VARCHAR(10) )  
```  
  
Utilisez CAST au lieu de CONVERT si vous souhaitez que le code de programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] soit compatible avec la norme ISO. Utilisez la fonction CONVERT et non la fonction CAST pour bénéficier de la fonctionnalité style de la fonction CONVERT.
  
L'illustration ci-dessous reprend toutes les conversions de types de données explicites et implicites autorisées pour les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournis par le système. Ces types sont notamment **xml**, **bigint** et **sql_variant**. Aucune conversion implicite d’attribution de valeur n’est effectuée à partir du type de données **sql_variant**, mais une conversion implicite vers **sql_variant** existe.
  
![Table de conversion de type de données](../../t-sql/data-types/media/lrdatahd.png "Table de conversion de type de données")

Le graphique ci-dessus illustre toutes les conversions implicites et explicites autorisées dans SQL Server, mais il n’indique pas le type de données résultant de la conversion. Quand SQL Server effectue une conversion explicite, l’instruction elle-même détermine le type de données résultant. Pour les conversions implicites,ce sont les instructions d’assignation, comme la définition de la valeur d’une variable ou l’insertion d’une valeur dans une colonne, qui donnent le type de données défini par la déclaration de variable ou la définition de colonne. Pour les opérateurs de comparaison ou d’autres expressions, le type de données résultant dépend des règles de priorité des types de données.

Par exemple, le script suivant définit une variable de type `varchar`, assigne une valeur de type `int` à la variable, puis sélectionne une concaténation de la variable avec une chaîne.

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.'
```

La valeur `int` de `1` étant convertie en `varchar`, l’instruction `SELECT` retourne la valeur `1 is a string.`.

L’exemple suivant illustre un script similaire avec à la place une variable `int` :

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.'
```

Dans ce cas, l’instruction `SELECT` génère l’erreur suivante :

`Msg 245, Level 16, State 1, Line 3`
`Conversion failed when converting the varchar value ' is not a string.' to data type int.`

Afin d’évaluer l’expression `@notastring + ' is not a string.'`, SQL Server suit les règles de priorité des types de données pour effectuer la conversion implicite avant le calcul du résultat de l’expression. Étant donné que le type `int` a une priorité plus élevée que `varchar`, SQL Server tente de convertir la chaîne en entier et échoue parce que cette chaîne ne peut pas être convertie en entier. Si l’expression fournit une chaîne pouvant être convertie, l’instruction réussit, comme dans l’exemple suivant :

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

La chaîne `1` pouvant être convertie en valeur entière `1` dans ce cas, l’instruction `SELECT` retourne la valeur `2`. Notez que l’opérateur `+` devient une addition plutôt qu’une concaténation quand les types de données fournis sont des entiers.

## <a name="data-type-conversion-behaviors"></a>Comportements de conversion de types de données

Lors de la conversion du type de données d'un objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un autre, certaines conversions de types de données implicites et explicites ne sont pas prises en charge. Ainsi, une valeur **nchar** ne peut pas être convertie en valeur **image**. Une valeur **nchar** peut uniquement être convertie en **binary** avec une conversion explicite ; une conversion implicite en **binary** n’est pas prise en charge. Cependant, une valeur **nchar** peut être convertie explicitement ou implicitement en **nvarchar**.
  
Les rubriques suivantes décrivent les comportements de conversion propres aux types de données correspondants :
  
 - [binary et varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)  
 - [datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)  
 - [money et smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
 - [bit &#40;Transact-SQL&#41;](../../t-sql/data-types/bit-transact-sql.md)  
 - [datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
 - [smalldatetime &#40;Transact-SQL&#41;](../../t-sql/data-types/smalldatetime-transact-sql.md)  
 - [char et varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
 - [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)  
 - [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
 - [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)  
 - [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)  
 - [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md)  
 - [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)  
 - [int, bigint, smallint et tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
 - [uniqueidentifier &#40;Transact-SQL&#41;](../../t-sql/data-types/uniqueidentifier-transact-sql.md)  
  
###  <a name="converting-data-types-by-using-ole-automation-stored-procedures"></a>Conversion de types de données à l'aide des procédures stockées OLE Automation  
Étant donné que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les types de données [!INCLUDE[tsql](../../includes/tsql-md.md)] et qu'OLE Automation utilise les types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], les procédures stockées OLE Automation doivent convertir les données qu'elles s'échangent.
  
Le tableau suivant décrit la conversion des types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].
  
|Type de données SQL Server|Type de données Visual Basic|  
|--------------------------|----------------------------|  
|**char**, **varchar**, **text**, **nvarchar**, **ntext**|**Chaîne**|  
|**decimal**, **numeric**|**Chaîne**|  
|**bit**|**Booléen**|  
|**binary**, **varbinary**, **image**|Tableau de type **Byte()** unidimensionnel|  
|**int**|**Long**|  
|**smallint**|**Integer**|  
|**tinyint**|**Byte**|  
|**float**|**Double**|  
|**real**|**Unique**|  
|**money**, **smallmoney**|**Devise**|  
|**datetime**, **smalldatetime**|**Date**|  
|Tout type qui prend la valeur NULL|**Variant** ayant la valeur Null|  
  
Toutes les valeurs uniques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont converties en une valeur unique [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], à l’exception des valeurs **binary**, **varbinary** et **image**. Ces valeurs sont converties en tableau de type **Byte()** unidimensionnel dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Ce tableau possède une plage **Byte(** 0 à _length_ 1 **)** où *length* représente le nombre d’octets dans les valeurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **binary**, **varbinary** ou **image**.
  
Il s'agit des conversions des types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] en types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
|Type de données Visual Basic|Type de données SQL Server|  
|----------------------------|--------------------------|  
|**Long**, **Integer**, **Byte**, **Boolean**, **Object**|**int**|  
|**Double**, **Single**|**float**|  
|**Devise**|**money**|  
|**Date**|**datetime**|  
|**String** avec 4 000 caractères au maximum|**varchar**/**nvarchar**|  
|**String** avec plus de 4 000 caractères|**text**/**ntext**|  
|Tableau de type **Byte()** unidimensionnel avec 8 000 octets au maximum|**varbinary**|  
|Tableau de type **Byte()** unidimensionnel avec plus de 8 000 octets|**image**|  
  
## <a name="see-also"></a>Voir aussi
[Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)
  

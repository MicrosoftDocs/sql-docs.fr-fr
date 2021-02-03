---
description: char et varchar (Transact-SQL)
title: char et varchar (Transact-SQL)
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- varchar
- varchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fixed-length data types [SQL Server]
- character data type
- char data type
- varchar(max) data type
- variable-length data types [SQL Server]
- varchar data type
- utf8
ms.assetid: 282cd982-f4fb-4b22-b2df-9e8478f13f6a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a54d7e0abbbb15379f83a13fc0e604b6d087326d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236385"
---
# <a name="char-and-varchar-transact-sql"></a>char et varchar (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les types de données de caractères qui sont soit de taille fixe, **char**, soit de taille variable, **varchar**. À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], quand un classement prenant en charge UTF-8 est utilisé, ces types de données stockent la plage complète des données caractères [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) et utilisent le codage de caractères [UTF-8](https://www.wikipedia.org/wiki/UTF-8). Si un classement non-UTF-8 est spécifié, ces types de données stockent uniquement un sous-ensemble de caractères pris en charge par la page de codes correspondante de ce classement.

## <a name="arguments"></a>Arguments

**char** [ ( *n* ) ] Données de type chaîne de taille fixe. *n* définit la taille de chaîne en octets et doit être une valeur comprise entre 1 et 8 000. Pour l’encodage de jeux de caractères sur un octet, par exemple *Latin*, la taille de stockage est *n* octets et le nombre de caractères pouvant être stockés est également *n*. Pour les jeux de caractères codés sur plusieurs octets, la taille de stockage est toujours *n* octets, mais le nombre de caractères pouvant être stockés peut être inférieur à *n*. Le synonyme ISO de **char** est **character**. Pour plus d’informations sur les jeux de caractères, consultez [Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

**varchar** [ ( *n* | **max** ) ] Données de type chaîne de taille variable. Utilisez *n* pour définir la taille de chaîne en octets et peut être une valeur comprise entre 1 et 8 000 ou utilisez **max** pour indiquer une taille de contrainte de colonne maximale de 2^31-1 octets (2 Go). Pour l’encodage de jeux de caractères sur un octet, par exemple *Latin*, la taille de stockage est *n* octets + 2 octets et le nombre de caractères pouvant être stockés est également *n*. Pour les jeux de caractères codés sur plusieurs octets, la taille de stockage est toujours *n* octets + 2 octets, mais le nombre de caractères pouvant être stockés peut être inférieur à *n*. Les synonymes ISO de **varchar** sont **charvarying** ou **charactervarying**. Pour plus d’informations sur les jeux de caractères, consultez [Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

## <a name="remarks"></a>Notes

Contrairement à l’idée fausse qui circule sur [CHAR(*n*) et VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), *n* ne définit pas le nombre de caractères. Dans [CHAR(*n*) et VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), *n* définit la longueur de chaîne en **octets** (0-8 000). *n* ne définit jamais des nombres de caractères pouvant être stockés. Cette définition est similaire à celle de [NCHAR(*n*) et de NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
En fait, lors de l’utilisation de l’encodage sur un octet, la taille de stockage de CHAR et VARCHAR est de *n* octets et le nombre de caractères est également de *n*, d’où cette idée fausse. Toutefois, pour l’encodage multioctet, par exemple [UTF-8](https://www.wikipedia.org/wiki/UTF-8), les plages Unicode plus élevées (128-1 114 111) génèrent un caractère utilisant deux octets ou plus. Par exemple, dans une colonne définie en tant que NCHAR(10), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] peut stocker 10 caractères qui utilisent l’encodage sur un octet (plage Unicode 0-127), mais moins de 10 caractères lors de l’utilisation de l’encodage multioctet (plage Unicode 128-1 114 111). Pour plus d’informations sur le stockage Unicode et les plages de caractères, consultez [Différences de stockage entre UTF-8 et UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).

Quand la valeur de *n* n’est spécifiée ni dans une définition de données ni dans une instruction de déclaration de variable, la longueur par défaut est 1. Si la valeur de *n* n’est pas précisée avec les fonctions CAST et CONVERT, la longueur par défaut est 30.

Les objets qui utilisent **char** ou **varchar** reçoivent le classement par défaut de la base de données, sauf si un classement spécifique est affecté à l’aide de la clause COLLATE. Le classement contrôle la page de codes utilisée pour stocker les données de caractères.

Les encodages multioctets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent :

- Les jeux de caractères codés sur deux octets (DBCS) pour certaines langues d’Extrême-Orient à l’aide des pages de codes 936 et 950 (chinois), 932 (japonais) ou 949 (coréen).
- UTF-8 avec la page de codes 65001. **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Si vous avez de sites qui prennent en charge plusieurs langues :

- À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], envisagez d’utiliser un classement UTF-8 prenant en charge Unicode pour limiter les problèmes de conversion de caractères.
- Si vous utilisez une version antérieure de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], envisagez d’utiliser les types de données Unicode **nchar** ou **nvarchar** afin de limiter les problèmes de conversion de caractères.

Si vous utilisez **char** ou **varchar**, nous vous recommandons de procéder comme suit :

- Utilisez **char** quand les tailles des entrées de données de la colonne sont cohérentes.
- Utilisez **varchar** quand les tailles des entrées de données de la colonne varient considérablement.
- Utilisez **varchar(max)** quand les tailles des entrées de données de la colonne varient considérablement et que la longueur de chaîne peut dépasser 8 000 octets.

Si SET ANSI_PADDING a la valeur OFF lors de l’exécution de CREATE TABLE ou d’ALTER TABLE, une colonne de type **char** définie comme Null est gérée comme une colonne de type **varchar**.

> [!WARNING]
> Chaque colonne varchar(max) ou nvarchar(max) non Null demande 24 octets d’allocation fixe supplémentaire calculée par rapport à la limite de 8 060 octets par ligne pendant une opération de tri. Cela peut produire une limite implicite du nombre de colonnes varchar(max) ou nvarchar(max) non Null pouvant être créées dans une table.
Aucune erreur spéciale n'est fournie quand la table est créée (mis à part l’avertissement habituel indiquant que la taille maximale de ligne dépasse la taille maximale autorisée de 8 060 octets) ou quand les données sont insérées. Cette grande taille de ligne peut provoquer des erreurs (comme l’erreur 512) au cours des opérations normales, telles que la mise à jour de la clé d’index cluster ou le tri de l’ensemble des colonnes, ce qui ne se produit que pendant l’exécution d’une opération.

## <a name="converting-character-data"></a><a name="_character"></a> Conversion de données caractères

Lorsque des expressions de caractères sont converties en type caractère de taille différente, les valeurs trop longues pour le nouveau type de données sont tronquées. Le type **uniqueidentifier** est considéré comme un type caractères pour la conversion d’une expression de type caractères. Il est donc soumis aux règles de troncation pour la conversion en un type caractères. Consultez la section Exemples qui suit.

Quand une expression de caractères est convertie en une expression de caractères de taille ou de type de données différent (par exemple, de **char(5)** en **varchar(5)** , ou **char(20)** en **char(15)** ), le classement de la valeur d’entrée est affecté à la valeur convertie. Si une expression de type non caractère est convertie en un type de données caractère, le classement par défaut de la base de données active est affecté à la valeur convertie. Dans les deux cas, vous pouvez affecter un classement spécifique à l’aide de la clause [COLLATE](../../t-sql/statements/collations.md).

> [!NOTE]
> Les traductions de pages de codes sont prises en charge pour les types de données **char** et **varchar**, mais pas pour **text**. Comme dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la perte de données lors de la traduction d’une page de codes n’est pas signalée.

Les expressions de type caractères qui sont converties en un type de données **numeric** approximatif peuvent inclure une notation exponentielle facultative. Cette notation est un e minuscule ou majuscule, suivi d’un signe facultatif plus (+) ou moins (-) et d’un nombre.

Les expressions de caractères converties en un type de données **numeric** exact doivent être composées de chiffres, d’une virgule décimale et éventuellement d’un plus (+) ou d’un moins (-). Les espaces à gauche sont ignorés. Les séparateurs virgule (séparateurs de milliers en notation anglaise comme dans 123,456.00) ne sont pas autorisés dans la chaîne.

Les expressions de caractères converties en types de données **money** ou **smallmoney** peuvent également comprendre une virgule décimale et le symbole du dollar ($). Les séparateurs virgule (séparateurs de milliers en notation anglaise comme dans $123,456.00) sont autorisés.

Quand une chaîne vide est convertie en **int**, sa valeur devient ```0```. Quand une chaîne vide est convertie en date, sa valeur devient la [valeur par défaut de la date](date-transact-sql.md), en l’occurrence ```1900-01-01```.

## <a name="examples"></a>Exemples

### <a name="a-showing-the-default-value-of-n-when-used-in-variable-declaration"></a>R. Affichage de la valeur par défaut de n utilisée dans une déclaration de variable

L’exemple suivant montre que la valeur par défaut de *n* est 1 pour les types de données `char` et `varchar` quand ils sont utilisés dans une déclaration de variable.

```sql
DECLARE @myVariable AS VARCHAR = 'abc';
DECLARE @myNextVariable AS CHAR = 'abc';
--The following returns 1
SELECT DATALENGTH(@myVariable), DATALENGTH(@myNextVariable);
GO
```

### <a name="b-showing-the-default-value-of-n-when-varchar-is-used-with-cast-and-convert"></a>B. Affichage de la valeur par défaut de n quand varchar est utilisé dans CAST et CONVERT

L’exemple suivant montre que la valeur par défaut de *n* est 30 quand les types de données `char` ou `varchar` sont utilisés avec les fonctions `CAST` et `CONVERT`.

```sql
DECLARE @myVariable AS VARCHAR(40);
SET @myVariable = 'This string is longer than thirty characters';
SELECT CAST(@myVariable AS VARCHAR);
SELECT DATALENGTH(CAST(@myVariable AS VARCHAR)) AS 'VarcharDefaultLength';
SELECT CONVERT(CHAR, @myVariable);
SELECT DATALENGTH(CONVERT(CHAR, @myVariable)) AS 'VarcharDefaultLength';
```

### <a name="c-converting-data-for-display-purposes"></a>C. Conversion de données à des fins d'affichage

L'exemple suivant convertit deux colonnes en types caractères et applique un style qui applique un format spécifique aux données affichées. Un type **money** est converti en données caractères et le style 1 est appliqué, ce qui affiche les valeurs avec une virgule tous les trois chiffres à gauche de la virgule décimale et deux chiffres à droite de la virgule décimale. Un type **datetime** est converti en données caractères et le style 3 est appliqué, ce qui affiche les données au format jj/mm/aa. Dans la clause WHERE, un type **money** est converti en un type caractère pour effectuer une opération de comparaison de chaînes.

```sql
USE AdventureWorks2012;
GO
SELECT BusinessEntityID,
   SalesYTD,
   CONVERT (VARCHAR(12),SalesYTD,1) AS MoneyDisplayStyle1,
   GETDATE() AS CurrentDate,
   CONVERT(VARCHAR(12), GETDATE(), 3) AS DateDisplayStyle3
FROM Sales.SalesPerson
WHERE CAST(SalesYTD AS VARCHAR(20) ) LIKE '1%';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
BusinessEntityID SalesYTD              DisplayFormat CurrentDate             DisplayDateFormat
---------------- --------------------- ------------- ----------------------- -----------------
278              1453719.4653          1,453,719.47  2011-05-07 14:29:01.193 07/05/11
280              1352577.1325          1,352,577.13  2011-05-07 14:29:01.193 07/05/11
283              1573012.9383          1,573,012.94  2011-05-07 14:29:01.193 07/05/11
284              1576562.1966          1,576,562.20  2011-05-07 14:29:01.193 07/05/11
285              172524.4512           172,524.45    2011-05-07 14:29:01.193 07/05/11
286              1421810.9242          1,421,810.92  2011-05-07 14:29:01.193 07/05/11
288              1827066.7118          1,827,066.71  2011-05-07 14:29:01.193 07/05/11
```

### <a name="d-converting-uniqueidentifer-data"></a>D. Conversion de données Uniqueidentifer

L'exemple suivant convertit une valeur `uniqueidentifier` en type de données `char`.

```sql
DECLARE @myid uniqueidentifier = NEWID();
SELECT CONVERT(CHAR(255), @myid) AS 'char';
```

L'exemple suivant illustre la troncation des données lorsque la valeur est trop longue pour le type de données vers lequel la conversion est effectuée. Étant donné que le type **uniqueidentifier** est limité à 36 caractères, les caractères qui dépassent cette longueur sont tronqués.

```sql
DECLARE @ID NVARCHAR(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
String                                       TruncatedValue
-------------------------------------------- ------------------------------------
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0

(1 row(s) affected)
```

## <a name="see-also"></a>Voir aussi

[nchar et nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)

[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)

[COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md)

[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)

[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)

[Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)

[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)

[Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)

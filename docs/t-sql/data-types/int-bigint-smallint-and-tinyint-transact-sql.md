---
title: int, bigint, smallint et tinyint (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour les types de données int, bigint, smallint et tinyint. Ces types de données sont utilisés pour représenter des données de type integer.
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bigint_TSQL
- smallint
- bigint
- smallint_TSQL
- tinyint_TSQL
- int_TSQL
- int
- tinyint
dev_langs:
- TSQL
helpviewer_keywords:
- exact numeric data [SQL Server]
- numeric data
- tinyint data type
- int data type
- smallint data type
ms.assetid: 9bda5b0b-2380-4931-a1c8-f362fdefa99b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23242d075a453f244601ed573ed779d160d810bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165137"
---
# <a name="int-bigint-smallint-and-tinyint-transact-sql"></a>int, bigint, smallint et tinyint (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Types de données représentant des valeurs numériques exactes qui utilisent des entiers. Pour économiser de l’espace dans la base de données, utilisez le plus petit type de données qui peut contenir de manière fiable toutes les valeurs possibles. Par exemple, tinyint peut suffire pour l’âge d’une personne, car personne ne vit plus de 255 ans. En revanche, tinyint ne serait pas suffisant pour dater un bâtiment, car il peut durer plus de 255 ans.
  
|Type de données|Plage|Stockage|  
|---|---|---|
|**bigint**|-2^63 (-9 223 372 036 854 775 808) à 2^63-1 (9 223 372 036 854 775 807)|Huit octets|  
|**int**|-2^31 (-2 147 483 648) à 2^31-1 (2 147 483 647)|Quatre octets|  
|**smallint**|-2^15 (-32 768) à 2^15-1 (32 767)|Deux octets|  
|**tinyint**|0 à 255|Un octet|  
  
## <a name="remarks"></a>Notes  
Le type de données **int** est le type de données integer principal dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le type de données **bigint** est destiné à être utilisé quand les valeurs entières sont susceptibles de ne pas appartenir à la plage prise en charge par le type de données **int**.
  
**bigint** s’insère entre **smallmoney** et **int** dans le graphique de priorité des types de données.
  
Les fonctions retournent **bigint** uniquement si l’expression de paramètre est un type de données **bigint**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne promeut pas automatiquement les autres types de données integer (**tinyint**, **smallint** et **int**) en **bigint**.
  
> [!CAUTION]  
>  Quand vous utilisez les opérateurs arithmétiques +, -, \*, / ou % pour réaliser une conversion implicite ou explicite de valeurs constantes **int**, **smallint**, **tinyint** ou **bigint** en types de données **float**, **real**, **decimal** ou **numeric**, les règles appliquées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour calculer le type de données et la précision des résultats des expressions varient selon que la requête est automatiquement paramétrable ou non.  
>   
>  Par conséquent, des expressions similaires dans les requêtes peuvent parfois produire des résultats différents. Quand une requête n’est pas automatiquement paramétrable, la valeur constante est d’abord convertie en type de données **numeric**, dont la précision est juste suffisante pour contenir la valeur de la constante, avant la conversion dans le type de données spécifié. Par exemple, la valeur constante 1 est convertie en **numeric (1, 0)** et la valeur constante 250 est convertie en **numeric (3, 0)** .  
>   
>  Quand une requête est automatiquement paramétrable, la valeur constante est toujours convertie en **numeric (10, 0)** avant la conversion dans le type de données final. Lorsque l'opérateur / est impliqué, outre la précision du type de résultat, la valeur de résultat peut aussi différer parmi des requêtes similaires. Par exemple, la valeur du résultat d’une requête automatiquement paramétrable qui comprend l’expression `SELECT CAST (1.0 / 7 AS float)` diffère de la valeur du résultat de la même requête non paramétrable automatiquement, car le résultat de la requête automatiquement paramétrable est tronqué afin qu’il puisse être contenu dans le type de données **numeric (10, 0)** .  
  
## <a name="converting-integer-data"></a>Conversion de données integer
Lorsque les entiers sont convertis implicitement en un type de données character, si l'entier est trop grand pour le champ de caractère, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche le caractère ASCII 42, l'astérisque (*).
  
Les constantes entières supérieures à 2 147 483 647 sont converties en type de données **decimal**, et non **bigint**. L’exemple suivant montre que, quand la valeur de seuil est dépassée, le type de données du résultat passe de **int** à **decimal**.
  
```sql
SELECT 2147483647 / 2 AS Result1, 2147483649 / 2 AS Result2 ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result1      Result2  
1073741823   1073741824.500000  
```  
  
## <a name="examples"></a>Exemples  
L’exemple suivant crée une table à l’aide des types de données **bigint**, **int**, **smallint** et **tinyint**. Les valeurs sont insérées dans chaque colonne et retournées dans l'instruction SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyBigIntColumn BIGINT  
,MyIntColumn  INT
,MySmallIntColumn SMALLINT
,MyTinyIntColumn TINYINT
);  
  
GO  
  
INSERT INTO dbo.MyTable VALUES (9223372036854775807, 2147483647,32767,255);  
 GO  
SELECT MyBigIntColumn, MyIntColumn, MySmallIntColumn, MyTinyIntColumn  
FROM dbo.MyTable;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
MyBigIntColumn       MyIntColumn MySmallIntColumn MyTinyIntColumn  
-------------------- ----------- ---------------- ---------------  
9223372036854775807  2147483647  32767            255  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  

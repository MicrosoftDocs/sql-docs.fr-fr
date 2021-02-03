---
title: decimal et numeric (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour les types de données decimal et numeric. Decimal et numeric sont synonymes pour les types de données numériques ayant une précision et une échelle fixes.
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- decimal
- decimal_TSQL
- numeric
- numeric_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decimal data type
- decimal data type, about decimal data type
- numeric data type
- numeric data type, about numeric data type
ms.assetid: 9d862a90-e6b7-4692-8605-92358dccccdf
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4aee37a054da84a247b34ee272b877e28d2bb023
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161269"
---
# <a name="decimal-and-numeric-transact-sql"></a>decimal et numeric (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Types de données numériques ayant une précision et une échelle fixes. Les types decimal et numeric sont synonymes et peuvent être utilisés indifféremment.
  
## <a name="arguments"></a>Arguments
**decimal**[ **(** _p_[ **,** _s_] **)** ] et **numeric**[ **(** _p_[ **,** _s_] **)** ]  
Valeurs de précision et d'échelle fixes. Lorsque la précision maximale est utilisée, les valeurs valides sont comprises entre - 10^38 +1 et 10^38 - 1. Les synonymes ISO de **decimal** sont **dec** et **dec(** _p_, _s_ **)** . **numeric** est fonctionnellement identique à **decimal**.
  
p (précision)  
Nombre total maximal de chiffres décimaux à stocker. Ce nombre inclut le côté à gauche et le côté à droite de la virgule. La précision doit être une valeur comprise entre 1 et la précision maximale de 38. La précision par défaut est 18.
  
> [!NOTE]  
>  Informatica prend uniquement en charge 16 chiffres significatifs, quelles que soient la précision et l’échelle spécifiées.  
  
*s* (échelle)  
Nombre de chiffres décimaux stockés à droite de la virgule. Ce nombre est soustrait de *p* afin de déterminer le nombre maximal de chiffres à gauche de la virgule décimale. La mise à l’échelle doit être une valeur comprise entre 0 et *p* et peut uniquement être spécifiée si la précision est indiquée. La valeur par défaut de l’échelle est de 0, par conséquent 0 <= *s* \<= *p*. Les tailles de stockage maximales varient en fonction de la précision.
  
|Precision|Taille de stockage (octets)|  
|---|---|
|1-9|5|  
|10-19|9|  
|20-28|13|  
|29-38|17|  
  
> [!NOTE]  
>  Informatica (connecté via le connecteur Informatica SQL Server PDW) prend uniquement en charge 16 chiffres significatifs, quelles que soient la précision et l’échelle spécifiées.  
  
## <a name="converting-decimal-and-numeric-data"></a>Conversion de données decimal et numeric
Pour les types de données **decimal** et **numeric**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère chaque combinaison de précision et d’échelle comme un type de données différent. Par exemple, **decimal(5,5)** et **decimal(5,0)** sont considérés comme des types de données différents.
  
Dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], une constante avec une virgule décimale est automatiquement convertie en valeur de donnée **numeric**, en utilisant la précision et l’échelle requises minimales. Par exemple, la constante 12,345 est convertie en valeur **numeric** avec une précision de 5 et une échelle de 3.
  
La conversion de **decimal** ou **numeric** en **float** ou **real** peut entraîner une certaine perte de précision. La conversion de **int**, **smallint**, **tinyint**, **float**, **real**, **money** ou **smallmoney** en **decimal** ou **numeric** peut entraîner un dépassement de capacité.
  
Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les arrondis lors de la conversion d’un nombre en valeur **decimal** ou **numeric** avec une précision et une échelle plus faibles. À l’inverse, si l'option SET ARITHABORT a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déclenche une erreur lors du dépassement de capacité. Perdre uniquement la précision et l'échelle ne suffit pas à engendrer une erreur.
  
Dans les versions antérieures à [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)], la conversion des valeurs **float** en valeurs **decimal** ou **numeric** est limitée à des valeurs d’une précision de 17 chiffres uniquement. Toutes les valeurs **float** inférieures à 5E-18 (quand elles sont définies avec la notation scientifique 5E-18 ou la notation décimale 0.0000000000000000050000000000000005) sont arrondies à 0. Cette limitation n’existe plus dans [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)].
  
## <a name="examples"></a>Exemples  
L’exemple suivant crée une table à l’aide des types de données **decimal** et **numeric**.  Les valeurs sont insérées dans chaque colonne. Les résultats sont retournés à l’aide d’une instruction SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyDecimalColumn DECIMAL(5,2)  
,MyNumericColumn NUMERIC(10,5)
  
);  
  
GO  
INSERT INTO dbo.MyTable VALUES (123, 12345.12);  
GO  
SELECT MyDecimalColumn, MyNumericColumn  
FROM dbo.MyTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
MyDecimalColumn                         MyNumericColumn  
--------------------------------------- ---------------------------------------  
123.00                                  12345.12000  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  

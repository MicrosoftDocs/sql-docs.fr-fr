---
description: LEN (Transact-SQL)
title: LEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/03/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEN
- LEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- LEN function
- characters [SQL Server], number of
- number of characters
ms.assetid: fa20fee4-884d-4301-891a-c03e901345ae
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4586ea73d3e64d60ac3f013e46fa43e75c30802
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204690"
---
# <a name="len-transact-sql"></a>LEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne le nombre de caractères de l’expression de type chaîne spécifiée, à l’exception des espaces de fin.  
  
> [!NOTE]  
> Pour retourner le nombre d’octets utilisés pour représenter une expression, utilisez la fonction [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
LEN ( string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *string_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de chaîne à évaluer. *string_expression* peut être une constante, une variable ou une colonne de données binaires ou caractères.  
  
## <a name="return-types"></a>Types de retour  
 **bigint** si *expression* est du type **varchar(max)** , **nvarchar(max)** ou **varbinary(max)**  ; sinon, **int**.  
  
 Si vous utilisez des classements SC, la valeur entière retournée compte les paires de substitution UTF-16 comme un caractère unique. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="remarks"></a>Notes  
LEN exclut les espaces de fin. Si ceci pose problème, envisagez d’utiliser la fonction [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md) qui ne les supprime pas. En cas de traitement d’une chaîne unicode, DATALENGTH retourne un nombre qui peut être différent du nombre de caractères. L’exemple suivant illustre les fonctions LEN et DATALENGTH avec un espace à droite.  
  
```sql  
  DECLARE @v1 VARCHAR(40),  
    @v2 NVARCHAR(40);  
SELECT   
@v1 = 'Test of 22 characters ',   
@v2 = 'Test of 22 characters ';  
SELECT LEN(@v1) AS [VARCHAR LEN] , DATALENGTH(@v1) AS [VARCHAR DATALENGTH];  
SELECT LEN(@v2) AS [NVARCHAR LEN], DATALENGTH(@v2) AS [NVARCHAR DATALENGTH];  
```  

> [!NOTE]
> Utilisez [LEN](../../t-sql/functions/len-transact-sql.md) pour retourner le nombre de caractères encodés dans une expression de chaîne donnée et [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) pour retourner la taille en octets d’une expression de chaîne donnée. Ces sorties peuvent différer selon le type de données et le type d’encodage utilisé dans la colonne. Pour plus d’informations sur les différences de stockage entre les différents types d’encodage, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).

## <a name="examples"></a>Exemples  
 L'exemple suivant sélectionne le nombre de caractères et les données figurant dans `FirstName` pour les personnes résidant en `Australia`. Cet exemple utilise la base de données AdventureWorks.  
  
```sql  
SELECT LEN(FirstName) AS Length, FirstName, LastName   
FROM Sales.vIndividualCustomer  
WHERE CountryRegionName = 'Australia';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant retourne le nombre de caractères dans la colonne `FirstName` et les prénoms et noms des employés situés dans `Australia`.  
  
```sql  
USE AdventureWorks2016  
GO  
SELECT DISTINCT LEN(FirstName) AS FNameLength, FirstName, LastName   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.DimGeography AS g   
    ON e.SalesTerritoryKey = g.SalesTerritoryKey   
WHERE EnglishCountryRegionName = 'Australia';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
FNameLength  FirstName  LastName  
-----------  ---------  ---------------  
4            Lynn       Tsoflias
```  
  
## <a name="see-also"></a>Voir aussi  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [CHARINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/charindex-transact-sql.md)  
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)   
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
  
  

---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: c2d152ce66276575de5fb299cda2e54ab0c37a43
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102329"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Spécifie le comportement, compatible avec ISO, des opérateurs de comparaison Égal à (=) et Différent de (<>), lorsqu'ils sont utilisés avec des valeurs Null dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Syntaxe pour [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] et [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Syntaxe pour [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] et [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
Quand ANSI_NULLS a la valeur ON, une instruction SELECT qui utilise la clause WHERE *nom_colonne* = **NULL** ne retourne aucune ligne, même si *nom_colonne* comporte des valeurs Null. Une instruction SELECT qui utilise la clause WHERE *nom_colonne* <> **NULL** ne retourne aucune ligne, même si *nom_colonne* comporte des valeurs non Null.  
  
Quand ANSI_NULLS a la valeur OFF, les opérateurs de comparaison Égal à (=) et Différent de (<>) ne sont pas conformes à la norme ISO. Une instruction SELECT qui utilise la clause WHERE *nom_colonne* = **NULL** retourne les lignes qui ont des valeurs Null dans *nom_colonne*. Une instruction SELECT qui utilise la clause WHERE *nom_colonne* <> **NULL** retourne les lignes qui ont des valeurs non Null dans la colonne. De plus, une instruction SELECT utilisant WHERE *nom_colonne* <> *valeur_XYZ* retourne toutes les lignes qui ne contiennent ni *valeur_XYZ*, ni la valeur Null.  
  
Quand ANSI_NULLS a la valeur ON, toutes les comparaisons effectuées sur une valeur Null retournent la valeur UNKNOWN. Lorsque la valeur de SET ANSI_NULLS a la valeur OFF, les comparaisons de toutes les données par rapport à une valeur Null sont vraies (TRUE) si la valeur des données est Null. Si SET ANSI_NULLS n'est pas spécifié, le paramètre de l'option ANSI_NULLS de la base de données actuelle s'applique. Pour plus d’informations sur l’option de base de données ANSI_NULLS, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  

Le tableau suivant montre comment la valeur de ANSI_NULLS affecte les résultats de différentes expressions booléennes utilisant des valeurs Null et non-Null.  
  
|Expression booléenne|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|UNKNOWN|TRUE|  
|1 = NULL|UNKNOWN|FALSE|  
|NULL <> NULL|UNKNOWN|FALSE|  
|1 <> NULL|UNKNOWN|TRUE|  
|NULL > NULL|UNKNOWN|UNKNOWN|  
|1 > NULL|UNKNOWN|UNKNOWN|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

SET ANSI_NULLS ON n'a d'influence sur une comparaison que si l'un des opérandes de la comparaison est une variable Null ou une valeur Null littérale. Si les deux termes de la comparaison sont des colonnes ou des expressions composées, le paramètre n'a pas d'incidence sur la comparaison.  
  
Pour qu'un script s'exécute correctement, quelle que soit la valeur de l'option de base de données ANSI_NULLS ou du paramètre de SET ANSI_NULLS, utilisez IS NULL et IS NOT NULL dans des comparaisons susceptibles de contenir des valeurs Null.  
  
Lors de l'exécution de requêtes distribuées, l'option ANSI_NULLS doit être activée (ON).  
  
ANSI_NULLS doit également avoir la valeur ON lors de la création ou de la modification d'index dans des colonnes calculées ou des vues indexées. Si SET ANSI_NULLS est désactivée (OFF), les instructions CREATE, UPDATE, INSERT et DELETE appliquées à des tables comportant des index sur des colonnes calculées ou des vues indexées échouent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur qui répertorie toutes les options SET non conformes aux valeurs requises. En outre, lors de l’exécution d’une instruction SELECT, si SET ANSI_NULLS a la valeur OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore les valeurs d’index dans les vues ou les colonnes calculées et résout l’opération select comme si ces index n’existaient pas.  
  
> [!NOTE]  
> ANSI_NULLS est l'une des sept options SET qui doivent avoir les valeurs requises lors du traitement des index dans des colonnes calculées et des vues indexées. Les options `ANSI_PADDING`, `ANSI_WARNINGS`, `ARITHABORT`, `QUOTED_IDENTIFIER` et `CONCAT_NULL_YIELDS_NULL` doivent également être définies sur ON, et `NUMERIC_ROUNDABORT` doit être définie sur OFF.  
  
 Le pilote ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et le fournisseur OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribuent automatiquement la valeur ON à ANSI_NULLS lors de la connexion. Cette valeur peut être configurée dans les sources de données ou les attributs de connexion ODBC, ainsi que dans les propriétés de connexion OLE DB définies dans l'application avant la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur par défaut de SET ANSI_NULLS est OFF.  
  
Quand ANSI_DEFAULTS est ON, ANSI_NULLS est activé.  
  
La valeur d’ANSI_NULLS est définie au moment de l’exécution, pas au moment de l’analyse.  
  
Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante :
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous utilise les opérateurs de comparaison Égal à (`=`) et Différent de (`<>`) pour effectuer une comparaison avec des valeurs `NULL` et non NULL dans une table. Il montre également que l’option `IS NULL` n’est pas influencée par le paramètre `SET ANSI_NULLS`.  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

Définissez à présent ANSI_NULLS avec la valeur ON et effectuez un test.

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

Définissez à présent ANSI_NULLS avec la valeur OFF et effectuez un test.  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [&#60;&#62; &#40;Not Equal To&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  

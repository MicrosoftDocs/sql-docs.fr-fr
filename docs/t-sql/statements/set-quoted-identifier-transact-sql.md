---
description: SET QUOTED_IDENTIFIER (Transact-SQL)
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 232c287564593eee0deab4ae37dc13c36d2d689b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100746"
---
# <a name="set-quoted_identifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Force [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à suivre les règles ISO se rapportant aux guillemets qui délimitent les identificateurs et les chaînes littérales. Les identificateurs entre guillemets doubles peuvent être des mots clés réservés [!INCLUDE[tsql](../../includes/tsql-md.md)] ou ils peuvent contenir des caractères généralement interdits dans les conventions de syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] concernant les identificateurs.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and serverless SQL pool in Azure Synapse Analytics

SET QUOTED_IDENTIFIER { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET QUOTED_IDENTIFIER ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
Quand `SET QUOTED_IDENTIFIER` a la valeur ON (par défaut), les identificateurs peuvent être encadrés par des guillemets (" ") et les littéraux par des apostrophes (' '). Toutes les chaînes délimitées par des guillemets doubles sont considérées comme des identificateurs d'objet. Par conséquent, les identificateurs entre guillemets n'ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Ils peuvent être des mots clés réservés et contenir des caractères généralement interdits dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les guillemets doubles ne peuvent pas servir à délimiter des expressions littérales ; seuls des guillemets simples peuvent encadrer des chaînes littérales. Si un guillemet simple (') fait partie d’une chaîne littérale, il peut être représenté par deux guillemets simples accolés ("). `SET QUOTED_IDENTIFIER` doit avoir la valeur ON si des mots clés réservés sont utilisés pour des noms d’objets dans la base de données.

Quand `SET QUOTED_IDENTIFIER` a la valeur OFF, les identificateurs ne peuvent pas figurer entre des guillemets et doivent respecter toutes les règles de [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md). Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles. Si une chaîne littérale est délimitée par des guillemets doubles, des apostrophes (guillemets simples) peuvent y être imbriquées.

> [!NOTE]
> QUOTED_IDENTIFIER n’affecte pas les identificateurs délimités entre crochets ([]).

`SET QUOTED_IDENTIFIER` doit avoir la valeur ON lors de la création ou de la modification d’index dans des colonnes calculées ou des vues indexées. Si `SET QUOTED_IDENTIFIER` a la valeur OFF, les instructions CREATE, UPDATE, INSERT et DELETE échouent dans des tables avec des index sur des colonnes calculées ou dans des tables avec des vues indexées. Pour plus d’informations sur les paramètres d’option SET requis dans des vues indexées et des index sur des colonnes calculées, consultez [Remarques sur l’utilisation des instructions SET](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).

`SET QUOTED_IDENTIFIER` doit avoir la valeur ON quand vous créez un index filtré.

`SET QUOTED_IDENTIFIER` doit avoir la valeur ON quand vous appelez les méthodes ayant le type de données XML.

Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribuent automatiquement la valeur ON à QUOTED_IDENTIFIER lors de la connexion. Cette option peut être configurée dans les sources de données et les attributs de connexion ODBC, ainsi que dans les propriétés de connexion OLE DB. Dans le cas d'une connexion depuis une application DB-Library, SET QUOTED_IDENTIFIER a la valeur OFF par défaut.

Lors de la création d'une table, l'option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table, même si elle a été affectée de la valeur OFF au moment de sa création.

Lors de la création d'une procédure stockée, les options de SET QUOTED_IDENTIFIER et SET ANSI_NULLS sont capturées puis réutilisées lors d'appels ultérieurs de cette procédure.

La valeur de SET QUOTED_IDENTIFIER ne change pas lorsqu'elle est exécutée dans une procédure stockée.

Quand `SET ANSI_DEFAULTS` a la valeur ON, QUOTED_IDENTIFIER a également la valeur ON.

`SET QUOTED_IDENTIFIER` correspond aussi au paramètre QUOTED_IDENTIFIER de [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

`SET QUOTED_IDENTIFIER` prend effet au moment de l’analyse [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il affecte uniquement l’analyse, et n’affecte ni l’optimisation ni l’exécution des requêtes.

Pour un lot ad hoc de plus haut niveau, l’analyse commence en utilisant le paramètre actuel de la session défini pour QUOTED_IDENTIFIER. Lors de l’analyse du lot, toute occurrence de `SET QUOTED_IDENTIFIER` change le comportement de l’analyse à partir de ce point, et enregistre ce paramètre pour la session. Par conséquent, une fois le lot analysé et exécuté, le paramètre QUOTED_IDENTIFER de la session est défini en fonction de la dernière occurrence de `SET QUOTED_IDENTIFIER` dans le lot.

Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] statique d’une procédure stockée est analysée à l’aide du paramètre QUOTED_IDENTIFIER en vigueur pour le lot qui a créé ou modifié la procédure stockée. `SET QUOTED_IDENTIFIER` n’a aucun effet quand il apparaît dans le corps d’une procédure stockée sous forme de code [!INCLUDE[tsql](../../includes/tsql-md.md)] statique.

Pour un lot imbriqué à l’aide de `sp_executesql` ou `exec()`, l’analyse commence en utilisant le paramètre QUOTED_IDENTIFIER de la session. Si le lot imbriqué se trouve à l’intérieur d’une procédure stockée, l’analyse commence en utilisant le paramètre QUOTED_IDENTIFIER de la procédure stockée. Lors de l’analyse du lot imbriqué, toute occurrence de `SET QUOTED_IDENTIFIER` change le comportement de l’analyse à partir de ce point, mais le paramètre QUOTED_IDENTIFIER de la session n’est pas mis à jour.

Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante :

```sql
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';
IF ( (256 & @@OPTIONS) = 256 ) 
SET @QUOTED_IDENTIFIER = 'ON';

SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;
```

## <a name="permissions"></a>Autorisations
Nécessite l’appartenance au rôle `PUBLIC`.

## <a name="examples"></a>Exemples

### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>R. Utilisation de l'option QUOTED IDENTIFIER et de noms d'objets avec des mots réservés

L'exemple suivant montre que l'option `SET QUOTED_IDENTIFIER` doit être `ON`, et les mots clés figurant dans les noms de tables doivent être placés entre guillemets doubles pour pouvoir créer et utiliser des objets dont les noms sont des mots clés réservés.

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- Create statement fails.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Create statement succeeds.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SELECT "identity","order"
FROM "select"
ORDER BY "order";
GO

DROP TABLE "SELECT";
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>B. Utilisation de QUOTED IDENTIFIER avec des guillemets simples et doubles

 L'exemple suivant montre comment utiliser les guillemets simples et doubles dans des chaînes, lorsque `SET QUOTED_IDENTIFIER` a la valeur `ON` et `OFF`.

```sql
SET QUOTED_IDENTIFIER OFF;
GO

USE AdventureWorks2012;
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME = 'Test')
    DROP TABLE dbo.Test;
GO
USE AdventureWorks2012;
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;
GO

-- Literal strings can be in single or double quotation marks.
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Strings inside double quotation marks are now treated
-- as object names, so they cannot be used for literals.
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');
GO

-- Object identifiers do not have to be in double quotation marks
-- if they are not reserved keywords.
SELECT ID, String
FROM dbo.Test;
GO

DROP TABLE dbo.Test;
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
 ID          String
 ----------- ------------------------------
 1           'Text in single quotes'
 2           'Text in single quotes'
 3           Text with 2 '' single quotes
 4           "Text in double quotes"
 5           "Text in double quotes"
 6           Text with 2 "" double quotes
 7           Text with a single ' quote
 ```

## <a name="see-also"></a>Voir aussi
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)    
[CREATE DEFAULT](../../t-sql/statements/create-default-transact-sql.md)    
[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)    
[CREATE RULE](../../t-sql/statements/create-rule-transact-sql.md)    
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)    
[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)    
[Types de données](../../t-sql/data-types/data-types-transact-sql.md)    
[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)    
[SELECT](../../t-sql/queries/select-transact-sql.md)    
[Instructions SET](../../t-sql/statements/set-statements-transact-sql.md)    
[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)    
[sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)    
[Identificateurs de base de données](../../relational-databases/databases/database-identifiers.md)

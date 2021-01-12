---
description: SET ANSI_NULL_DFLT_ON (Transact-SQL)
title: SET ANSI_NULL_DFLT_ON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ANSI_NULL_DFLT_ON
- ANSI_NULL_DFLT_ON_TSQL
- SET ANSI_NULL_DFLT_ON
- SET_ANSI_NULL_DFLT_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULL_DFLT_ON statement
- ANSI_NULL_DFLT_ON option
- default nullability
- null values [SQL Server], overriding
- overriding default nullability
ms.assetid: 8c925924-a466-4c8b-aeb2-7e0d341f32db
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39854417675cc4a7ee767c27918ae16c070c9ef5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100765"
---
# <a name="set-ansi_null_dflt_on-transact-sql"></a>SET ANSI_NULL_DFLT_ON (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie le comportement de la session de manière à supplanter l’acceptation par défaut des valeurs NULL dans les nouvelles colonnes lorsque l’option **ANSI null default** de la base de données a la valeur **false**. Pour plus d’informations sur la configuration de la valeur de l’option **ANSI null default**, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

SET ANSI_NULL_DFLT_ON {ON | OFF}
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_ON ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 Ce paramètre affecte l'acceptation des valeurs NULL dans une nouvelle colonne, uniquement si celle-ci n'est pas spécifiée dans les instructions CREATE TABLE et ALTER TABLE. Lorsque l'option SET ANSI_NULL_DFLT_ON est activée (ON), les nouvelles colonnes créées à l'aide de l'instruction ALTER TABLE ou CREATE TABLE autorisent les valeurs NULL si cet état n'est pas explicitement spécifié pour ces dernières. SET ANSI_NULL_DFLT_ON n'a aucun effet sur les colonnes créées avec une valeur NULL ou NOT NULL explicite.  
  
 SET ANSI_NULL_DFLT_OFF et SET ANSI_NULL_DFLT_ON ne peuvent pas être activées (valeur ON) simultanément. Si l'une des options a la valeur ON, l'autre a automatiquement la valeur OFF. Par conséquent, SET ANSI_NULL_DFLT_OFF ou SET ANSI_NULL_DFLT_ON peut avoir la valeur ON ou les deux peuvent avoir la valeur OFF. Si l'une des deux options a la valeur ON, ce paramètre (SET ANSI_NULL_DFLT_OFF ou SET ANSI_NULL_DFLT_ON) prend effet. Si la valeur des deux options est OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la valeur de la colonne **is_ansi_null_default_on** dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Pour garantir un meilleur fonctionnement des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisés dans des bases de données gérant différemment l'acceptation de valeurs NULL, il est préférable de spécifier NULL ou NOT NULL dans les instructions CREATE TABLE et ALTER TABLE.  
  
 Le pilote ODBC du client natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le fournisseur OLE DB du client natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affectent automatiquement la valeur ON à ANSI_NULL_DFLT_ON lors de la connexion. Dans le cas d'applications DB-Library, SET ANSI_NULL_DFLT_ON prend par défaut la valeur OFF.  
  
 Lorsque SET ANSI_DEFAULTS est défini sur ON, l'option SET ANSI_NULL_DFLT_ON est activée.  
  
 La valeur de SET ANSI_NULL_DFLT_ON est définie lors de l'exécution, et non pas durant l'analyse.  
  
 Le paramètre de SET ANSI_NULL_DFLT_ON ne s'applique pas lorsque les tables sont créées à l'aide de l'instruction SELECT INTO.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_ON VARCHAR(3) = 'OFF';  
IF ( (1024 & @@OPTIONS) = 1024 ) SET @ANSI_NULL_DFLT_ON = 'ON';  
SELECT @ANSI_NULL_DFLT_ON AS ANSI_NULL_DFLT_ON;  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant illustre les effets de `SET ANSI_NULL_DFLT_ON` quand la valeur de l’option de base de données **ANSI null default** est ON et OFF, respectivement.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON  
-- has an effect when the 'ANSI null default' for the database is false.  
-- Set the 'ANSI null default' database option to false by executing  
-- ALTER DATABASE.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t2.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL insert should succeed.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t3.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t3 (a TINYINT);  
GO  
-- NULL insert should fail.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON   
-- has no effect when the 'ANSI null default' for the database is true.  
-- Set the 'ANSI null default' database option to true.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON  
GO  
  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t5.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t6.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t6 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1,t2,t3,t4,t5,t6;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SET ANSI_NULL_DFLT_OFF &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)  
  
  

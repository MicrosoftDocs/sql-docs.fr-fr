---
description: SET ANSI_NULL_DFLT_OFF (Transact-SQL)
title: SET ANSI_NULL_DFLT_OFF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_NULL_DFLT_OFF_TSQL
- ANSI_NULL_DFLT_OFF
- SET ANSI_NULL_DFLT_OFF
- SET_ANSI_NULL_DFLT_OFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- default nullability
- ANSI_NULL_DFLT_OFF option
- null values [SQL Server], overriding
- overriding default nullability
- SET ANSI_NULL_DFLT_OFF statement
ms.assetid: 8ed5c512-f5de-4741-a18a-de85a3041295
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2386dbb9873b6f93b755db4dbb8dcd883dc27d6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190529"
---
# <a name="set-ansi_null_dflt_off-transact-sql"></a>SET ANSI_NULL_DFLT_OFF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie le comportement de la session de manière à supplanter l’acceptation par défaut des valeurs NULL dans les nouvelles colonnes quand l’option ANSI null default de la base de données a la valeur **true**. Pour plus d’informations sur la configuration de la valeur de l’option ANSI null default, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET ANSI_NULL_DFLT_OFF { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_OFF OFF
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 Ce paramètre affecte l'acceptation des valeurs NULL dans une nouvelle colonne, uniquement si celle-ci n'est pas spécifiée dans les instructions CREATE TABLE et ALTER TABLE. Si l'option SET ANSI_NULL_DFLT_OFF est activée (ON), les nouvelles colonnes créées à l'aide de l'instruction ALTER TABLE ou CREATE TABLE sont de type NOT NULL par défaut, si l'acceptation des valeurs NULL n'est pas explicitement spécifiée pour ces dernières. SET ANSI_NULL_DFLT_OFF n’a aucun effet sur les colonnes créées avec une valeur NULL ou NOT NULL explicite.  
  
 SET ANSI_NULL_DFLT_OFF et SET ANSI_NULL_DFLT_ON ne peuvent pas être activées (valeur ON) simultanément. Si l'une des options a la valeur ON, l'autre a automatiquement la valeur OFF. Par conséquent, SET ANSI_NULL_DFLT_OFF ou SET ANSI_NULL_DFLT_ON peut avoir la valeur ON, ou les deux options peuvent avoir la valeur OFF. Si l'une des deux options a la valeur ON, ce paramètre (SET ANSI_NULL_DFLT_OFF ou SET ANSI_NULL_DFLT_ON) prend effet. Si la valeur des deux options est OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la valeur de la colonne is_ansi_null_default_on dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Pour garantir un meilleur fonctionnement des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisés dans des bases de données gérant différemment l’acceptation de valeurs NULL, il est préférable de toujours spécifier NULL ou NOT NULL dans les instructions CREATE TABLE et ALTER TABLE.  
  
 La valeur de SET ANSI_NULL_DFLT_OFF est définie lors de l'exécution et non durant l'analyse.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_OFF VARCHAR(3) = 'OFF';  
IF ( (2048 & @@OPTIONS) = 2048 ) SET @ANSI_NULL_DFLT_OFF = 'ON';  
SELECT @ANSI_NULL_DFLT_OFF AS ANSI_NULL_DFLT_OFF;  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre les effets de `SET ANSI_NULL_DFLT_OFF` lorsque la valeur de l'option de base de données ANSI null default est ON et OFF, respectivement.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Set the 'ANSI null default' database option to true by executing   
-- ALTER DATABASE.  
GO  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT);  
GO  
-- NULL INSERT should succeed.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t2.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL INSERT should fail.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t3.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t3 (a TINYINT) ;  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- This illustrates the effect of having both the database  
-- option and SET option disabled.  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t5.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL insert should fail.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t6.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t6 (a TINYINT);   
GO   
-- NULL insert should fail.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1, t2, t3, t4, t5, t6;  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)  
  
  

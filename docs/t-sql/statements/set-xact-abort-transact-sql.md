---
description: SET XACT_ABORT (Transact-SQL)
title: SET XACT_ABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- XACT_ABORT_TSQL
- XACT_ABORT
- SET XACT_ABORT
- SET_XACT_ABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- XACT_ABORT option
- automatic transaction roll backs
- transactions [SQL Server], rolling back
- rolling back transactions, SET XACT_ABORT
- roll back transactions [SQL Server]
- SET XACT_ABORT statement
ms.assetid: cbcaa433-58f2-4dc3-a077-27273bef65b5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9e67626f2e5ac088d0313e4c325db433850194a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206925"
---
# <a name="set-xact_abort-transact-sql"></a>SET XACT_ABORT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> L’instruction **THROW** respecte **SET XACT_ABORT**, contrairement à **RAISERROR**. Les nouvelles applications doivent utiliser **THROW** au lieu de **RAISERROR**.

Indique si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restaure automatiquement la transaction en cours lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] déclenche une erreur d'exécution.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
SET XACT_ABORT { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes

Lorsque SET XACT_ABORT est défini sur ON et qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] génère une erreur d'exécution, la transaction est interrompue et annulée dans son intégralité.

Si SET XACT_ABORT est défini sur OFF, dans certains cas seulement, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui a généré l'erreur est annulée et le traitement de la transaction se poursuit. Selon la gravité de l'erreur, la transaction peut être quand même annulée dans son intégralité lorsque SET XACT_ABORT est défini sur OFF. OFF est le paramètre par défaut dans une instruction T-SQL, tandis que ON est le paramètre par défaut dans un déclencheur.

Les erreurs de compilation, comme les erreurs de syntaxe, ne sont pas affectées par l'option SET XACT_ABORT.

XACT_ABORT doit être défini sur ON pour les instructions de modification des données dans une transaction implicite ou explicite avec la plupart des fournisseurs OLE DB, y compris [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le seul cas où cette option n'est pas obligatoire, c'est lorsque le fournisseur prend en charge les transactions imbriquées.

Lorsque ANSI_WARNINGS=OFF, des violations d'autorisations peuvent provoquer l'abandon de transactions.

L'option SET XACT_ABORT est définie lors de l'exécution, et non pas durant l'analyse.

Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.

```sql
DECLARE @XACT_ABORT VARCHAR(3) = 'OFF';
IF ( (16384 & @@OPTIONS) = 16384 ) SET @XACT_ABORT = 'ON';
SELECT @XACT_ABORT AS XACT_ABORT;

```

## <a name="examples"></a>Exemples

L'exemple suivant provoque une erreur de violation de clé étrangère dans une transaction comportant d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dans le premier jeu d'instructions, l'erreur est générée mais les autres instructions sont correctement exécutées et la transaction est validée. Dans le deuxième jeu d'instructions, `SET XACT_ABORT` est défini sur `ON`. L'erreur met alors fin à l'exécution du traitement d'instructions et la transaction est annulée.

```sql
IF OBJECT_ID(N't2', N'U') IS NOT NULL
    DROP TABLE t2;
GO
IF OBJECT_ID(N't1', N'U') IS NOT NULL
    DROP TABLE t1;
GO  
CREATE TABLE t1
    (a INT NOT NULL PRIMARY KEY);
CREATE TABLE t2
    (a INT NOT NULL REFERENCES t1(a));
GO
INSERT INTO t1 VALUES (1);
INSERT INTO t1 VALUES (3);
INSERT INTO t1 VALUES (4);
INSERT INTO t1 VALUES (6);
GO
SET XACT_ABORT OFF;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (1);
INSERT INTO t2 VALUES (2); -- Foreign key error.
INSERT INTO t2 VALUES (3);
COMMIT TRANSACTION;
GO
SET XACT_ABORT ON;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (4);
INSERT INTO t2 VALUES (5); -- Foreign key error.
INSERT INTO t2 VALUES (6);
COMMIT TRANSACTION;
GO
-- SELECT shows only keys 1 and 3 added.
-- Key 2 insert failed and was rolled back, but
-- XACT_ABORT was OFF and rest of transaction
-- succeeded.
-- Key 5 insert error with XACT_ABORT ON caused
-- all of the second transaction to roll back.
SELECT *
 FROM t2;
GO
```

## <a name="see-also"></a>Voir aussi

- [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)
- [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)
- [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)
- [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
- [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
- [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  

---
description: ALTER MASTER KEY (Transact-SQL)
title: ALTER MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: fasttrack-edit
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER MASTER KEY
- ALTER_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REGENERATE phrase
- ALTER MASTER KEY statement
- decryption [SQL Server], Database Master Key
- FORCE option
- encryption [SQL Server], Database Master Key
- database master key [SQL Server], modifying
- cryptography [SQL Server], Database Master Key
- modifying Database Master Key
- service master key [SQL Server], modifying
- DROP ENCRYPTION BY SERVICE MASTER KEY phrase
ms.assetid: 8ac501c3-4280-4d5b-b58a-1524fa715b50
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 677de39f127ea8efc853746168eea1d3be7e4eb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204173"
---
# <a name="alter-master-key-transact-sql"></a>ALTER MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Permet de modifier les propriétés de la clé principale d'une base de données.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure SQL Database
-- Note: DROP ENCRYPTION BY SERVICE MASTER KEY is not supported on Azure SQL Database.

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Analytics Platform System

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD ='password'

<encryption_option> ::=
    ADD ENCRYPTION BY SERVICE MASTER KEY
    |
    DROP ENCRYPTION BY SERVICE MASTER KEY
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

PASSWORD ='*password*' Spécifie un mot de passe avec lequel chiffrer ou déchiffrer la clé principale de la base de données. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="remarks"></a>Notes

L'option REGENERATE permet de créer de nouveau la clé principale de base de données et toutes les clés qu'elle protège. Les clés sont déchiffrées au préalable au moyen de l'ancienne clé principale, puis chiffrées au moyen de la nouvelle clé principale. Cette opération gourmande en ressources doit être planifiée au cours d'une période de faible demande, à moins que la clé principale ait été compromise.

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] utilise l’algorithme de chiffrement AES pour protéger la clé principale du service (SMK) et la clé principale de base de données (DMK). AES est un algorithme de chiffrement plus récent que 3DES, qui était utilisé dans les versions antérieures. Au terme de la mise à niveau d'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , les clés SMK et DMK doivent être régénérées pour mettre à niveau les clés principales vers AES. Pour plus d’informations sur la régénération de la clé SMK, consultez l’article [ALTER SERVICE MASTER KEY](../../t-sql/statements/alter-service-master-key-transact-sql.md).

Lorsque l'option FORCE est utilisée, la régénération de clé continue même si la clé principale n'est pas disponible ou si le serveur ne peut pas déchiffrer toutes les clés privées chiffrées. Si la clé principale ne peut pas être ouverte, utilisez l’instruction [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md) pour restaurer la clé principale à partir d’une sauvegarde. Utilisez l'option FORCE seulement si la clé principale ne peut pas être récupérée ou si le déchiffrement échoue. Les données chiffrées uniquement à l'aide d'une clé irrécupérable sont perdues.

L'option DROP ENCRYPTION BY SERVICE MASTER KEY permet de supprimer le chiffrement de la clé principale de base de données au moyen de la clé principale du service.

ADD ENCRYPTION BY SERVICE MASTER KEY entraîne le chiffrement d'une copie de la clé principale au moyen de la clé principale du service et le stockage de cette copie dans la base de données en cours et dans master.

## <a name="permissions"></a>Autorisations

Requiert l'autorisation CONTROL sur la base de données. Si la clé principale de base de données a été chiffrée à l'aide d'un mot de passe, ce mot de passe doit également être connu.

## <a name="examples"></a>Exemples

Dans l'exemple ci-dessous, une nouvelle clé principale de base de données est créée pour `AdventureWorks` et les clés placées sous cette clé dans la hiérarchie de chiffrement sont chiffrées de nouveau.

```sql
USE AdventureWorks2012;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Dans l’exemple suivant, une clé principale de base de données est créée pour `AdventureWorksPDW2012` et les clés placées sous cette clé dans la hiérarchie de chiffrement sont de nouveau chiffrées.

```sql
USE master;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE MASTER KEY](../../t-sql/statements/create-master-key-transact-sql.md)
- [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md)
- [CLOSE MASTER KEY](../../t-sql/statements/close-master-key-transact-sql.md)
- [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md)
- [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md)
- [DROP MASTER KEY](../../t-sql/statements/drop-master-key-transact-sql.md)
- [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Attacher et détacher une base de données](../../relational-databases/databases/database-detach-and-attach-sql-server.md)

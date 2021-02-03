---
description: CREATE MASTER KEY (Transact-SQL)
title: CREATE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_MASTER_KEY_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- CREATE MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], Database Master Key
- database master key [SQL Server]
- CREATE MASTER KEY statement
- cryptography [SQL Server], Database Master Key
- database master key [SQL Server], creating
ms.assetid: 1710a305-1a4f-48ec-836c-11ffd0356d76
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b77069fd4665ea72988f3eba5aee6c790c6c181e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188601"
---
# <a name="create-master-key-transact-sql"></a>CREATE MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crée une clé principale de base de données dans la base de données MASTER.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE MASTER KEY [ ENCRYPTION BY PASSWORD ='password' ]
[ ; ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

PASSWORD ='*password*' est le mot de passe utilisé pour chiffrer la clé principale dans la base de données. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *password* est facultatif dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

## <a name="remarks"></a>Notes

La clé principale de base de données est une clé symétrique qui permet de protéger les clés privées des certificats et des clés asymétriques présentes dans la base de données. Lors de sa création, la clé principale est chiffrée à l'aide de l'algorithme AES_256 et d'un mot de passe fourni par l'utilisateur. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], l'algorithme triple DES est utilisé. Pour permettre le déchiffrement automatique de la clé principale, une copie de la clé est chiffrée à l'aide de la clé principale de service et stockée dans la base de données et dans master. En général, la copie stockée dans master est mise à jour sans avertissement chaque fois que la clé principale est modifiée. Ce comportement par défaut peut être changé à l’aide de l’option DROP ENCRYPTION BY SERVICE MASTER KEY d’[ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Une clé principale qui n’est pas chiffrée par la clé principale de service doit être ouverte à l’aide de l’instruction [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md) et d’un mot de passe.

La colonne is_master_key_encrypted_by_server de l'affichage catalogue sys.databases dans master indique si la clé principale de base de données est chiffrée au moyen de la clé principale de service.

Des informations sur la clé principale de base de données sont consultables dans l'affichage catalogue sys.symmetric_keys.

Pour SQL Server et Parallel Data Warehouse, la clé principale est généralement protégée par la clé principale de service et au moins un mot de passe. Dans le cas où la base de données est physiquement déplacée vers un autre serveur (copie des journaux de transaction, restauration de sauvegarde, etc.), la base de données contient une copie de la clé principale chiffrée par la clé principale de service du serveur d’origine (sauf si ce chiffrement a été explicitement supprimé en utilisant `ALTER MASTER KEY DDL`) et une copie de cette clé chiffrée par chaque mot de passe spécifié pendant les opérations `CREATE MASTER KEY` ou `ALTER MASTER KEY DDL` consécutives. Pour récupérer la clé principale et toutes les données chiffrées à l’aide de la clé principale comme racine de la hiérarchie de clés après le déplacement de la base de données, l’utilisateur doit utiliser l’instruction `OPEN MASTER KEY` à l’aide de l’un des mots de passe utilisés pour protéger la clé principale, restaurer une sauvegarde de la clé principale ou restaurer une sauvegarde de la clé principale de service d’origine sur le nouveau serveur.

Pour [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], la protection par mot de passe n’est pas considérée comme un mécanisme de sécurité pour empêcher un scénario de perte de données dans les cas où la base de données peut être déplacée d’un serveur vers un autre, car la protection de la clé principale de service sur la clé principale est gérée par la plateforme Microsoft Azure. Par conséquent, le mot de passe de la clé principale est facultatif dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

> [!IMPORTANT]
> Vous devez sauvegarder la clé principale à l’aide de l’instruction [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md) et stocker la sauvegarde en lieu sûr, en dehors de votre lieu de travail.

La clés principale du service et les clés principales de base de données sont protégées à l'aide de l'algorithme AES-256.

## <a name="permissions"></a>Autorisations

Requiert l'autorisation CONTROL sur la base de données.

## <a name="examples"></a>Exemples

Utilisez l’exemple suivant pour créer une clé principale de base de données dans la base de données `master`. La clé est chiffrée à l'aide du mot de passe `23987hxJ#KL95234nl0zBe`.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';
GO
```

## <a name="see-also"></a>Voir aussi

- [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)
- [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)
- [DROP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-master-key-transact-sql.md)
- [CLOSE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-master-key-transact-sql.md)
- [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)

---
description: OPEN MASTER KEY (Transact-SQL)
title: OPEN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN MASTER KEY DECRYPTION BY PASSWORD
- OPEN_MASTER_KEY_DECRYPTION_BY_PASSWORD_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- OPEN_MASTER_KEY_TSQL
- MASTER KEY DECRYPTION
- OPEN MASTER KEY
- MASTER_KEY_DECRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- opening Database Master Keys
- encryption [SQL Server], Database Master Key
- cryptography [SQL Server], Database Master Key
- master key decryption
- OPEN MASTER KEY statement
- database master key [SQL Server], opening
ms.assetid: 1674753e-ca1e-4913-9ba4-b442e7106121
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e9acddda3ac18a3ae82b9d09f6ecc1ee1fc793c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199354"
---
# <a name="open-master-key-transact-sql"></a>OPEN MASTER KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ouvre la clé principale de base de données de la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'password'   
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 '*password*'  
 Mot de passe utilisé pour le chiffrement de la clé principale de la base de données.  
  
## <a name="remarks"></a>Remarques  
 Si cette clé a été chiffrée avec la clé principale du service, elle est automatiquement ouverte lorsqu'elle doit être utilisée pour du chiffrement ou du déchiffrement. Dans ce cas, il n’est pas nécessaire d’utiliser l’instruction **OPEN MASTER KEY**.  
  
 Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés.  
  
 Vous pouvez exclure la clé principale d'une base de données spécifique de la gestion de clés automatique en utilisant l'instruction ALTER MASTER KEY avec l'option DROP ENCRYPTION BY SERVICE MASTER KEY. Après quoi, vous devez ouvrir explicitement la clé principale de la base de données avec un mot de passe.  
  
 Si une transaction dans laquelle la clé principale de la base de données a été explicitement ouverte est annulée, la clé reste ouverte.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ouvre la clé principale de la base de données `AdventureWorks2012`, chiffrée avec un mot de passe.  
  
```sql  
USE AdventureWorks2012;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant ouvre la clé principale de base de données, qui a été chiffrée avec un mot de passe.  
  
```sql  
USE master;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
CLOSE MASTER KEY;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CLOSE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-master-key-transact-sql.md)   
 [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md)   
 [RESTORE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [DROP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-master-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  


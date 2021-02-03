---
description: ALTER DATABASE ENCRYPTION KEY (Transact-SQL)
title: ALTER DATABASE ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.date: 04/16/2018
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_ENCRYPTION_KEY_TSQL
- ALTER DATABASE ENCRYPTION
- ALTER_DATABASE_ENCRYPTION_TSQL
- ALTER DATABASE ENCRYPTION KEY
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key, alter
- ALTER DATABASE ENCRYPTION KEY
ms.assetid: f88dac4b-efe0-47ed-9808-972a4381377e
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 5519ff16020ab6e02220c32d72df4a349619a036
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205787"
---
# <a name="alter-database-encryption-key-transact-sql"></a>ALTER DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  Modifie une clé et un certificat de chiffrement permettant de chiffrer une base de données de façon transparente. Pour plus d’informations sur le chiffrement transparent de bases de données, consultez [Chiffrement transparent des données &#40;TDE, Transparent Data Encryption&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER DATABASE ENCRYPTION KEY  
      REGENERATE WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   |  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER DATABASE ENCRYPTION KEY  
    {  
      {  
        REGENERATE WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
        [ ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name ]  
      }  
      |  
      ENCRYPTION BY SERVER   CERTIFICATE Encryptor_Name    
    }  
[ ; ]  
```  
 
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 REGENERATE WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY }  
 Spécifie l'algorithme de chiffrement utilisé pour la clé de chiffrement.  
  
 ENCRYPTION BY SERVER CERTIFICATE *Encryptor_Name*  
 Spécifie le nom du certificat utilisé pour chiffrer la clé de chiffrement de base de données.  
  
 ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
 Spécifie le nom de la clé asymétrique utilisée pour chiffrer la clé de chiffrement de base de données.  
  
## <a name="remarks"></a>Notes  
 Le certificat ou la clé asymétrique utilisé pour chiffrer la clé de chiffrement de base de données doit se trouver dans la base de données système principale.  
  
 Quand le propriétaire de base de données (dbo) est modifié, la clé de chiffrement de base de données n’a pas besoin d’être régénérée.
  
 Lorsque la clé de chiffrement d'une base de données a été modifiée deux fois, une sauvegarde de fichier journal doit être effectuée pour rendre possible une nouvelle modification de la clé de chiffrement.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur la base de données et l'autorisation VIEW DEFINITION sur le certificat ou la clé asymétrique permettant de chiffrer la clé de chiffrement de base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie la clé de chiffrement de base de données pour qu'elle utilise l'algorithme `AES_256`.  
  
```sql  
-- Uses AdventureWorks  
  
ALTER DATABASE ENCRYPTION KEY  
REGENERATE WITH ALGORITHM = AES_256;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
 [Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
 [sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
  
  


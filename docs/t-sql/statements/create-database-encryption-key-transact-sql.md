---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: af62041c92ca78b52204aac7d736465d5d01012b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270386"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 Crée une clé de chiffrement permettant de chiffrer une base de données de façon transparente. Pour plus d’informations sur le chiffrement transparent de bases de données, consultez [Chiffrement transparent des données &#40;TDE, Transparent Data Encryption&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY  }  
Spécifie l'algorithme de chiffrement utilisé pour la clé de chiffrement.

> [!WARNING]
> Depuis SQL Server 2016, tous les algorithmes autres que AES_128, AES_192 et AES_256 sont dépréciés. Pour utiliser des algorithmes plus anciens (ce qui n’est pas recommandé), vous devez affecter le niveau de compatibilité 120 ou un niveau inférieur à la base de données.  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
Spécifie le nom du chiffreur utilisé pour chiffrer la clé de chiffrement de base de données.  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
Spécifie le nom de la clé asymétrique utilisée pour chiffrer la clé de chiffrement de base de données. Afin de chiffrer la clé de chiffrement de base de données avec une clé asymétrique, la clé asymétrique doit résider sur un fournisseur de gestion de clés extensible.  
  
## <a name="remarks"></a>Notes  
Une clé de chiffrement de base de données est nécessaire avant qu’une base de données puisse être chiffrée en utilisant le *chiffrement transparent des données* (TDE). Lorsqu'une base de données est chiffrée de façon transparente, elle l'est entièrement au niveau des fichiers, sans aucune modification de code spéciale. Le certificat ou la clé asymétrique utilisé pour chiffrer la clé de chiffrement de base de données doit se trouver dans la base de données système principale.  
  
Les instructions de chiffrement de base de données ne sont autorisées que sur les bases de données utilisateur.  
  
La clé de chiffrement de base de données ne peut pas être exportée de la base de données. Elle est uniquement disponible pour le système, les utilisateurs qui disposent d'autorisations de débogage sur le serveur et les utilisateurs qui ont accès aux certificats qui chiffrent et déchiffrent la clé de chiffrement de base de données.  
  
La clé de chiffrement de base de données n'a pas besoin d'être régénérée lorsqu'un propriétaire de base de données (dbo) est modifié.  
  
Une clé de chiffrement de base de données est automatiquement créée pour une base de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Vous n’avez pas besoin de créer une clé à l’aide de l’instruction CREATE DATABASE ENCRYPTION KEY.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'autorisation CONTROL sur la base de données et l'autorisation VIEW DEFINITION sur le certificat ou la clé asymétrique permettant de chiffrer la clé de chiffrement de base de données.  
  
## <a name="examples"></a>Exemples  
Pour obtenir des exemples supplémentaires utilisant TDE, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md), [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) et [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
Dans l'exemple suivant, une clé de chiffrement de base de données est créée à l'aide de l'algorithme `AES_256`, et protège la clé privée à l'aide d'un certificat nommé `MyServerCert`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
[SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    

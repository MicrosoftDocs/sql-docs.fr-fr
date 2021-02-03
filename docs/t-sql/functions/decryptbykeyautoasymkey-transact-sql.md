---
description: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
title: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOASYMKEY_TSQL
- DECRYPTBYKEYAUTOASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOASYMSKEY function
ms.assetid: 5521d4cf-740c-4ede-98b6-4ba90b84e32d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 66764a72445cc161abe878f2713ec122d5627067
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207010"
---
# <a name="decryptbykeyautoasymkey-transact-sql"></a>DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fonction déchiffre les données chiffrées. Pour ce faire, elle commence par déchiffrer une clé symétrique avec une clé asymétrique distincte, puis déchiffre les données chiffrées avec la clé symétrique extraite à la première étape.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DecryptByKeyAutoAsymKey ( akey_ID , akey_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *akey_ID*  
ID de la clé asymétrique servant à chiffrer la clé symétrique. *akey_ID* a le type de données **int**.  
  
 *akey_password*  
Mot de passe protégeant la clé asymétrique. *akey_password* peut avoir une valeur NULL si la clé principale de base de données protège la clé privée asymétrique. *akey_password* a le type de données **nvarchar**.  
  
 *ciphertext* Données chiffrées avec la clé. *ciphertext* a le type de données **varbinary**.  
  
 @ciphertext  
Variable de type **varbinary** contenant des données chiffrées avec la clé symétrique.  
  
 *add_authenticator*  
Indique si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Doit correspondre à la valeur passée à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durant le processus de chiffrement des données. *add_authenticator* a la valeur 1 si le processus de chiffrement utilise un authentificateur. *add_authenticator* a le type de données **int**.  
  
 @add_authenticator  
Variable indiquant si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Doit correspondre à la valeur passée à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durant le processus de chiffrement des données. *\@add_authenticator* a le type de données **int**.
  
 *authenticator*  
Données utilisées comme base pour la génération de l’authentificateur. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* a le type de données **sysname**.  
  
 @authenticator  
Variable contenant des données à partir desquelles un authentificateur est généré. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* a le type de données **sysname**.  
  
@add_authenticator  
Variable indiquant si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Doit correspondre à la valeur passée à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durant le processus de chiffrement des données. *\@add_authenticator* a le type de données **int**.  

*authenticator*  
Données utilisées comme base pour la génération de l’authentificateur. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* a le type de données **sysname**.

@authenticator  
Variable contenant des données à partir desquelles un authentificateur est généré. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* a le type de données **sysname**.  

## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
`DECRYPTBYKEYAUTOASYMKEY` combine les fonctionnalités d’`OPEN SYMMETRIC KEY` et de `DECRYPTBYKEY`. Dans une même opération, il déchiffre d’abord une clé symétrique, puis déchiffre le texte chiffré avec cette clé.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation `VIEW DEFINITION` sur la clé symétrique et `CONTROL` sur la clé asymétrique.  
  
## <a name="examples"></a>Exemples
L’exemple suivant montre comment `DECRYPTBYKEYAUTOASYMKEY` peut simplifier le code de déchiffrement. Ce code doit s’exécuter sur une base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] qui n’a pas encore de clé principale de base de données.  

```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE ASYMMETRIC KEY SSN_AKey   
    WITH ALGORITHM = RSA_2048 ;   
GO  
CREATE SYMMETRIC KEY SSN_Key_02 WITH ALGORITHM = DES  
    ENCRYPTION BY ASYMMETRIC KEY SSN_AKey;  
GO  
--  
--Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber2 varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber2  
    = EncryptByKey(Key_GUID('SSN_Key_02'), NationalIDNumber);  
GO  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key.  
--2. Decrypt the data.  
--3. Close the symmetric key.  
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
SELECT NationalIDNumber, EncryptedNationalIDNumber2    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--OPTION TWO, using DecryptByKeyAutoAsymKey()  
SELECT NationalIDNumber, EncryptedNationalIDNumber2   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoAsymKey ( AsymKey_ID('SSN_AKey') , NULL ,EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

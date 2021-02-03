---
description: DECRYPTBYKEYAUTOCERT (Transact-SQL)
title: DECRYPTBYKEYAUTOCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOCERT
- DECRYPTBYKEYAUTOCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOCERT function
ms.assetid: 6b45fa2e-ffaa-46f7-86ff-5624596eda4a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 11cc7a88ad012554fac63b08030e31686ef1cc8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207209"
---
# <a name="decryptbykeyautocert-transact-sql"></a>DECRYPTBYKEYAUTOCERT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fonction déchiffre les données avec une clé symétrique. Cette clé symétrique est automatiquement déchiffrée avec un certificat.  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DecryptByKeyAutoCert ( cert_ID , cert_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *cert_ID*  
ID du certificat utilisé pour protéger la clé symétrique. *cert_ID* a le type de données **int**.  
  
*cert_password*  
Mot de passe utilisé pour chiffrer la clé privée du certificat. Peut avoir une valeur `NULL` si la clé principale de la base de données protège la clé privée. *cert_password* a le type de données **nvarchar**.  

'*ciphertext*'  
Chaîne de données chiffrées avec la clé. *ciphertext* a le type de données **varbinary**.  

@ciphertext  
Variable de type **varbinary** contenant des données chiffrées avec la clé.  

*add_authenticator*  
Indique si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Doit correspondre à la valeur passée à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durant le processus de chiffrement des données. *add_authenticator* a la valeur 1 si le processus de chiffrement utilise un authentificateur. *add_authenticator* a le type de données **int**.  
  
@add_authenticator  
Variable indiquant si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Doit correspondre à la valeur passée à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durant le processus de chiffrement des données. *\@add_authenticator* a le type de données **int**.  
  
*authenticator*  
Données utilisées comme base pour la génération de l’authentificateur. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* a le type de données **sysname**.  
  
@authenticator  
Variable contenant des données à partir desquelles un authentificateur est généré. Doit correspondre à la valeur fournie à [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* a le type de données **sysname**.  
  
## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
`DECRYPTBYKEYAUTOCERT` combine les fonctionnalités d’`OPEN SYMMETRIC KEY` et de `DECRYPTBYKEY`. Dans une même opération, il déchiffre d’abord une clé symétrique, puis déchiffre le texte chiffré avec cette clé.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation `VIEW DEFINITION` sur la clé symétrique et l’autorisation `CONTROL` sur le certificat.   
  
## <a name="examples"></a>Exemples  
L’exemple suivant montre comment `DECRYPTBYKEYAUTOCERT` peut simplifier le code de déchiffrement. Ce code doit s’exécuter sur une base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] qui n’a pas encore de clé principale de base de données.  
  
```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE CERTIFICATE HumanResources037   
   WITH SUBJECT = 'Sammamish HR',   
   EXPIRY_DATE = '10/31/2009';  
CREATE SYMMETRIC KEY SSN_Key_01 WITH ALGORITHM = DES  
    ENCRYPTION BY CERTIFICATE HumanResources037;  
GO  
----Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037 ;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
--  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key  
--2. Decrypt the data  
--3. Close the symmetric key  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
SELECT NationalIDNumber, EncryptedNationalIDNumber    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_01;  
--  
--OPTION TWO, using DecryptByKeyAutoCert()  
SELECT NationalIDNumber, EncryptedNationalIDNumber   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoCert ( cert_ID('HumanResources037') , NULL ,EncryptedNationalIDNumber))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

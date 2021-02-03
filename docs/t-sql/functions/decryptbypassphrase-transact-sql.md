---
description: DECRYPTBYPASSPHRASE (Transact-SQL)
title: DECRYPTBYPASSPHRASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYPASSPHRASE
- DECRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], DECRYPTBYPASSPHRASE function
- DECRYPTBYPASSPHRASE function
ms.assetid: ca34b5cd-07b3-4dca-b66a-ed8c6a826c95
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 81e6e954a597e3ea6b7e2b6e983e063c4a1bf8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207235"
---
# <a name="decryptbypassphrase-transact-sql"></a>DECRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction déchiffre les données chiffrées à l’origine avec une phrase secrète.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DecryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *passphrase*  
Phrase secrète utilisée pour générer la clé de déchiffrement.  
  
 @passphrase  
Variable de type **char**, **nchar**, **nvarchar** ou **varchar** contenant la phrase secrète utilisée pour générer la clé de déchiffrement.  
  
'*ciphertext*'  
Chaîne de données chiffrées avec la clé. *ciphertext* a le type de données **varbinary**.  
 
@ciphertext  
Variable de type **varbinary** contenant des données chiffrées avec la clé. La variable *\@ciphertext* a une taille maximale de 8 000 octets.  
  
*add_authenticator*  
Indique si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. *add_authenticator* a la valeur 1 si le processus de chiffrement utilise un authentificateur. *add_authenticator* a le type de données **int**.  
  
@add_authenticator  
Variable indiquant si le processus de chiffrement d’origine comprend et chiffre un authentificateur avec le texte en clair. Is *\@add_authenticator* a la valeur 1 si le processus de chiffrement utilise un authentificateur. *\@add_authenticator* a le type de données **int**.  

*authenticator*  
Données utilisées comme base pour la génération de l’authentificateur. *authenticator* a le type de données **sysname**.  
  
@authenticator  
Variable contenant les données utilisées comme base pour la génération des authentificateurs. *\@authenticator* a le type de données **sysname**.  
  
## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
`DECRYPTBYPASSPHRASE` ne nécessite pas d’autorisations pour son exécution. `DECRYPTBYPASSPHRASE` retourne NULL si elle reçoit la phrase secrète ou les informations de l’authentificateur incorrectes.  
  
`DECRYPTBYPASSPHRASE` utilise la phrase secrète pour générer une clé de déchiffrement. Cette clé de déchiffrement n’est pas conservée.  
  
Si un authentificateur a été inclus au moment du chiffrement du texte chiffré, `DECRYPTBYPASSPHRASE` doit recevoir ce même authentificateur pour le processus de déchiffrement. Si la valeur de l’authentificateur fournie pour le processus de déchiffrement ne correspond pas à celle initialement utilisée pour chiffrer les données, l’opération `DECRYPTBYPASSPHRASE` échoue.  
  
## <a name="examples"></a>Exemples  
L’exemple suivant déchiffre l’enregistrement mis à jour dans [EncryptByPassPhrase](../../t-sql/functions/encryptbypassphrase-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
-- Get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
= 'A little learning is a dangerous thing!';  
  
-- Decrypt the encrypted record.  
SELECT CardNumber, CardNumber_EncryptedbyPassphrase   
    AS 'Encrypted card number', CONVERT(varchar,  
    DecryptByPassphrase(@PassphraseEnteredByUser, CardNumber_EncryptedbyPassphrase, 1   
    , CONVERT(varbinary, CreditCardID)))  
    AS 'Decrypted card number' FROM Sales.CreditCard   
    WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ENCRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
  
  

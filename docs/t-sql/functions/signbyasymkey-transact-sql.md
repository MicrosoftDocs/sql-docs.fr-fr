---
description: SIGNBYASYMKEY (Transact-SQL)
title: SIGNBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SIGNBYASYMKEY_TSQL
- SIGNBYASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- text signing [SQL Server]
- encryption [SQL Server], asymmetric keys
- signing text [SQL Server]
- SIGNBYASYMKEY function
- asymmetric keys [SQL Server], SIGNBYASYMKEY function
- cryptography [SQL Server], asymmetric keys
- clear text signing
ms.assetid: b1c46159-cc76-4205-a841-8f4a71742f80
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 786c196f985a8e9328af23fba6ba3857a1fb27bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212176"
---
# <a name="signbyasymkey-transact-sql"></a>SIGNBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Signe du texte en clair avec une clé asymétrique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SignByAsymKey( Asym_Key_ID , @plaintext [ , 'password' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Asym_Key_ID*  
 ID d'une clé asymétrique de la base de données actuelle. *Asym_Key_ID* est de type **int**.  
  
 **\@plaintext**  
 Variable de type **nvarchar**, **char**, **varchar** ou **nchar** dont les données seront signées avec la clé asymétrique.  
  
 *mot de passe*  
 Mot de passe au moyen duquel la clé privée est protégée. *password* est de type **nvarchar(128)**.  
  
## <a name="return-types"></a>Types de retour  
 **varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Remarques  
 Nécessite l'autorisation CONTROL sur la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une table, `SignedData04`, dans laquelle doivent être stockés le texte en clair et sa signature. Ensuite, il insère un enregistrement dans la table, signé avec la clé asymétrique `PrimeKey`, qui est d'abord déchiffré avec le mot de passe `'pGFD4bb925DGvbd2439587y'`.  
  
```sql  
-- Create a table in which to store the data  
CREATE TABLE [SignedData04](Description NVARCHAR(max), Data NVARCHAR(max), DataSignature VARBINARY(8000));  
GO  
-- Store data together with its signature  
DECLARE @clear_text_data NVARCHAR(max);  
set @clear_text_data = N'Important numbers 2, 3, 5, 7, 11, 13, 17,   
      19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79,  
      83, 89, 97';  
INSERT INTO [SignedData04]   
    VALUES( N'data encrypted by asymmetric key ''PrimeKey''',  
    @clear_text_data, SignByAsymKey( AsymKey_Id( 'PrimeKey' ),  
    @clear_text_data, N'pGFD4bb925DGvbd2439587y' ));  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)   
 [VERIFYSIGNEDBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/verifysignedbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

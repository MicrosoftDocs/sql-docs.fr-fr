---
description: DECRYPTBYASYMKEY (Transact-SQL)
title: DECRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYASYMKEY
- DECRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], DECRYPTBYASYMKEY function
- DECRYPTBYASYMKEY function
- decryption [SQL Server], asymmetric keys
ms.assetid: d9ebcd30-f01c-4cfe-b95e-ffe6ea13788b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c37631b5be6d68f69249507de4e8f96556731eeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195754"
---
# <a name="decryptbyasymkey-transact-sql"></a>DECRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction utilise une clé asymétrique pour déchiffrer les données chiffrées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
DecryptByAsymKey (Asym_Key_ID , { 'ciphertext' | @ciphertext }   
    [ , 'Asym_Key_Password' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Asym_Key_ID*  
ID d’une clé asymétrique dans la base de données. *Asym_Key_ID* a le type de données **int**.  
  
 *ciphertext*  
Chaîne de données chiffrées avec la clé asymétrique.  
  
 @ciphertext  
Variable de type **varbinary** contenant des données chiffrées avec la clé asymétrique.  
  
 *Asym_Key_Password*  
Mot de passe utilisé pour chiffrer la clé asymétrique dans la base de données.  
  
## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
Le chiffrement/déchiffrement à clé asymétrique a un coût élevé par rapport au chiffrement/déchiffrement à clé symétrique. Pour les jeux de données volumineux (par exemple, des données utilisateur stockées dans des tables), nous recommandons aux développeurs d’éviter le chiffrement/déchiffrement à clé symétrique.  
  
## <a name="permissions"></a>Autorisations  
`DECRYPTBYASYMKEY` nécessite l’autorisation CONTROL sur la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
Cet exemple déchiffre le texte chiffré à l’origine avec la clé asymétrique `JanainaAsymKey02`. Cette clé asymétrique était stockée dans `AdventureWorks2012.ProtectedData04`. L’exemple déchiffrait les données retournées avec la clé asymétrique `JanainaAsymKey02`. L’exemple utilisait le mot de passe `pGFD4bb925DGvbd2439587y` pour déchiffrer cette clé asymétrique. L’exemple convertissait le texte en clair retourné en type **nvarchar**.  
  
```sql
SELECT CONVERT(NVARCHAR(max),  
    DecryptByAsymKey( AsymKey_Id('JanainaAsymKey02'),   
    ProtectedData, N'pGFD4bb925DGvbd2439587y' ))   
AS DecryptedData   
FROM [AdventureWorks2012].[Sales].[ProtectedData04]   
WHERE Description = N'encrypted by asym key''JanainaAsymKey02''';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ENCRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

---
title: ENCRYPTBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENCRYPTBYCERT
- ENCRYPTBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], encryption
- encryption [SQL Server], certificates
- ENCRYPTBYCERT function
ms.assetid: ab66441f-e2d2-4e3a-bcae-bcc09e12f3c1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 801d2af4bc83b974761c1bd11982e0943460cf92
ms.sourcegitcommit: 768f046107642f72693514f51bf2cbd00f58f58a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87112989"
---
# <a name="encryptbycert-transact-sql"></a>ENCRYPTBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Chiffre des données au moyen de la clé publique d'un certificat.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
EncryptByCert ( certificate_ID , { 'cleartext' | @cleartext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_certificate\_ID_  
ID d'un certificat de la base de données. **int**.  
  
_cleartext_  
Chaîne de données chiffrées avec le certificat.  
  
**\@cleartext**  
Variable de l’un des types suivants, dont les données seront chiffrées avec la clé publique du certificat :

* **nvarchar** 
* **char**
* **varchar**
* **binary** 
* **varbinary**
* **nchar**
  
## <a name="return-types"></a>Types de retour  
**varbinary** d’une taille maximale de 8 000 octets.  
  
## <a name="remarks"></a>Notes  
Cette fonction chiffre des données à l’aide de la clé publique du certificat. Seule la clé privée correspondante peut déchiffrer le texte chiffré. Ces transformations asymétriques sont coûteuses par rapport au chiffrement/déchiffrement avec une clé symétrique. Par conséquent, le chiffrement asymétrique n’est pas recommandé avec des ensembles de données volumineux.
  
## <a name="examples"></a>Exemples  
Cet exemple chiffre le texte en clair stocké dans `@cleartext` avec le certificat `JanainaCert02`. Les données chiffrées sont insérées dans la table `ProtectedData04`.  
  
```  
INSERT INTO [AdventureWorks2012].[ProtectedData04]   
    VALUES ( N'Data encrypted by certificate ''Shipping04''',  
    EncryptByCert(Cert_ID('JanainaCert02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[DECRYPTBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbycert-transact-sql.md)   
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
[ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
[DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
[BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

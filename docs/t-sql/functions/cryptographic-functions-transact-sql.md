---
description: Fonctions de chiffrement (Transact-SQL)
title: Fonctions de chiffrement (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], cryptographic
- crypto functions
- cryptography [SQL Server], functions
- decryption [SQL Server], functions
- security functions
- encryption [SQL Server], functions
ms.assetid: 0be5626b-5a25-4d8c-9f44-7abbfccf816c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a4e0de6fbf1181c57681b8bd3f554575cb2ab92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184135"
---
# <a name="cryptographic-functions-transact-sql"></a>Fonctions de chiffrement (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Ces fonctions prennent en charge la signature numérique, la validation des signatures numériques, le chiffrement et le déchiffrement.
  
## <a name="symmetric-encryption-and-decryption"></a>Chiffrement et déchiffrement symétriques

:::row:::
    :::column:::
        [ENCRYPTBYKEY](../../t-sql/functions/encryptbykey-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYKEY](../../t-sql/functions/decryptbykey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ENCRYPTBYPASSPHRASE](../../t-sql/functions/encryptbypassphrase-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYPASSPHRASE](../../t-sql/functions/decryptbypassphrase-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [KEY_ID](../../t-sql/functions/key-id-transact-sql.md)
    :::column-end:::
    :::column:::
        [KEY_GUID](../../t-sql/functions/key-guid-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [DECRYPTBYKEYAUTOASYMKEY](../../t-sql/functions/decryptbykeyautoasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [KEY_NAME](../../t-sql/functions/key-name-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SYMKEYPROPERTY](../../t-sql/functions/symkeyproperty-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="asymmetric-encryption-and-decryption"></a>Chiffrement et déchiffrement asymétriques
  
:::row:::
    :::column:::
        [ENCRYPTBYASYMKEY](../../t-sql/functions/encryptbyasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYASYMKEY](../../t-sql/functions/decryptbyasymkey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ENCRYPTBYCERT](../../t-sql/functions/encryptbycert-transact-sql.md)
    :::column-end:::
    :::column:::
        [DECRYPTBYCERT](../../t-sql/functions/decryptbycert-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [ASYMKEYPROPERTY](../../t-sql/functions/asymkeyproperty-transact-sql.md)
    :::column-end:::
    :::column:::
        [ASYMKEY_ID](../../t-sql/functions/asymkey-id-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="signing-and-signature-verification"></a>Signature et vérification de signature

:::row:::
    :::column:::
        [SIGNBYASYMKEY](../../t-sql/functions/signbyasymkey-transact-sql.md)
    :::column-end:::
    :::column:::
        [VERIFYSIGNEDBYASMKEY](../../t-sql/functions/verifysignedbyasymkey-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SIGNBYCERT](../../t-sql/functions/signbycert-transact-sql.md)
    :::column-end:::
    :::column:::
        [VERIGYSIGNEDBYCERT](../../t-sql/functions/verifysignedbycert-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [IS_OBJECTSIGNED](../../t-sql/functions/is-objectsigned-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;
  
## <a name="symmetric-decryption-with-automatic-key-handling"></a>Déchiffrement symétrique avec gestion de clé automatique

:::row:::
    :::column:::
        [DecryptByKeyAutoCert](../../t-sql/functions/decryptbykeyautocert-transact-sql.md)
    :::column-end:::
:::row-end:::
 
&nbsp;

## <a name="encryption-hashing"></a>Hachage de chiffrement

:::row:::
    :::column:::
        [HASHBYTES](../../t-sql/functions/hashbytes-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="certificate-copying"></a>Copie de certificat 

:::row:::
    :::column:::
        [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)
    :::column-end:::
    :::column:::
        [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="see-also"></a>Voir aussi
[Fonctions](../../t-sql/functions/functions.md)  
[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
[Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
[Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
  
  

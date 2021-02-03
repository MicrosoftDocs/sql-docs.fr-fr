---
description: ALTER COLUMN ENCRYPTION KEY (Transact-SQL)
title: ALTER COLUMN ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER COLUMN ENCRYPTION
- ALTER_COLUMN_ENCRYPTION_TSQL
- ALTER COLUMN ENCRYPTION KEY
- ALTER_COLUMN_ENCRYPTION_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column encryption key, alter
- ALTER COLUMN ENCRYPTION KEY statement
ms.assetid: c79a220d-e178-4091-a330-c924cc0f0ae0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: b9aaf24adad7cc1451b32173bedefc30fbff60eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191045"
---
# <a name="alter-column-encryption-key-transact-sql"></a>ALTER COLUMN ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Modifie une clé de chiffrement de colonne dans une base de données, en ajoutant ou en supprimant une valeur chiffrée. Une clé de chiffrement de colonne peut avoir jusqu’à deux valeurs, ce qui permet de permuter la clé principale de colonne correspondante. Une clé de chiffrement de colonne est utilisée lors du chiffrement des colonnes avec [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) ou [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Avant d’ajouter une valeur de clé de chiffrement de colonne, vous devez définir la clé principale de colonne qui a été utilisée pour chiffrer la valeur à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de l’instruction [CREATE MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER COLUMN ENCRYPTION KEY key_name   
    [ ADD | DROP ] VALUE   
    (  
        COLUMN_MASTER_KEY = column_master_key_name   
        [, ALGORITHM = 'algorithm_name' , ENCRYPTED_VALUE =  varbinary_literal ]   
    ) [;]  
```  

## <a name="arguments"></a>Arguments
 *key_name*  
 Clé de chiffrement de colonne que vous changez.  
  
 *column_master_key_name*  
 Spécifie le nom de la clé principale de colonne (CMK) utilisée pour chiffrer la clé de chiffrement de colonne (CEK).  
  
 *algorithm_name*  
 Nom de l’algorithme de chiffrement utilisé pour chiffrer la valeur. L’algorithme des fournisseurs de système doit être **RSA_OAEP**. Cet argument n’est pas valide lors de la suppression d’une valeur de clé de chiffrement de colonne.  
  
 *varbinary_literal*  
 Objet blob de la clé CEK chiffré avec la clé de chiffrement principale spécifiée. Cet argument n’est pas valide lors de la suppression d’une valeur de clé de chiffrement de colonne.  
  
> [!WARNING]  
>  Ne passez jamais des valeurs de clé CEK en texte clair dans cette instruction. Cela compromet l’avantage de cette fonctionnalité.  

## <a name="remarks"></a>Notes
En général, une clé de chiffrement de colonne est créée avec une seule valeur chiffrée. Quand une clé principale de colonne doit être permutée (c’est-à-dire quand la clé principale de colonne actuelle doit être remplacée par la nouvelle clé principale de colonne), vous pouvez ajouter une nouvelle valeur de la clé de chiffrement de colonne, chiffrée avec la nouvelle clé principale de colonne. Ce workflow vous permet de garantir que les applications clientes peuvent accéder aux données chiffrées avec la clé de chiffrement de colonne, tandis que la nouvelle clé principale de colonne est mise à la disposition des applications clientes. Un pilote avec Always Encrypted dans une application cliente qui n’a pas accès à la nouvelle clé principale peut utiliser la valeur de la clé de chiffrement de colonne chiffrée avec l’ancienne clé principale de colonne pour accéder aux données sensibles. Les algorithmes de chiffrement pris en charge par Always Encrypted exigent que la valeur de texte en clair comporte 256 bits. 
 
Nous vous recommandons d’utiliser des outils, tels que SQL Server Management Studio (SSMS) ou PowerShell, pour la rotation des clés principales de colonne. Consultez [Effectuer la rotation de clés Always Encrypted avec SQL Server Management Studio](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-ssms.md) et [Effectuer la rotation de clés Always Encrypted avec PowerShell](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md).

Une valeur chiffrée doit être générée à l’aide d’un fournisseur de magasin de clés qui encapsule le magasin de clés contenant la clé principale de colonne.  

 Les clés principales de colonne sont permutées pour les raisons suivantes :
- Les réglementations de conformité peuvent exiger que les clés soient permutées régulièrement.
- Une clé principale de colonne est compromise et doit être permutée pour des raisons de sécurité.
- Pour activer ou désactiver le partage de clés de chiffrement de colonne avec une enclave sécurisée côté serveur. Par exemple, si votre clé principale de colonne actuelle ne prend pas en charge les calculs d’enclave (n’a pas été définie avec la propriété ENCLAVE_COMPUTATIONS) et que vous souhaitez activer les calculs d’enclave sur les colonnes protégées avec une clé de chiffrement de colonne que chiffre votre clé principale de colonne, vous devez remplacer la clé principale de colonne par la nouvelle clé avec la propriété ENCLAVE_COMPUTATIONS. [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) et [Gérer les clés pour Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md).

Pour afficher des informations sur les clés de chiffrement de colonne, utilisez [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md), [sys.column_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) et [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Exige l’autorisation **ALTER ANY COLUMN ENCRYPTION KEY** sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-column-encryption-key-value"></a>R. Ajout d’une valeur de clé de chiffrement de colonne  
 L’exemple suivant modifie une clé de chiffrement de colonne nommée `MyCEK`.  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
ADD VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK2,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E00740075007300650072002F006D0079002F0064006500650063006200660034006100340031003000380034006200350033003200360066003200630062006200350030003600380065003900620061003000320030003600610037003800310066001DDA6134C3B73A90D349C8905782DD819B428162CF5B051639BA46EC69A7C8C8F81591A92C395711493B25DCBCCC57836E5B9F17A0713E840721D098F3F8E023ABCDFE2F6D8CC4339FC8F88630ED9EBADA5CA8EEAFA84164C1095B12AE161EABC1DF778C07F07D413AF1ED900F578FC00894BEE705EAC60F4A5090BBE09885D2EFE1C915F7B4C581D9CE3FDAB78ACF4829F85752E9FC985DEB8773889EE4A1945BD554724803A6F5DC0A2CD5EFE001ABED8D61E8449E4FAA9E4DD392DA8D292ECC6EB149E843E395CDE0F98D04940A28C4B05F747149B34A0BAEC04FFF3E304C84AF1FF81225E615B5F94E334378A0A888EF88F4E79F66CB377E3C21964AACB5049C08435FE84EEEF39D20A665C17E04898914A85B3DE23D56575EBC682D154F4F15C37723E04974DB370180A9A579BC84F6BC9B5E7C223E5CBEE721E57EE07EFDCC0A3257BBEBF9ADFFB00DBF7EF682EC1C4C47451438F90B4CF8DA709940F72CFDC91C6EB4E37B4ED7E2385B1FF71B28A1D2669FBEB18EA89F9D391D2FDDEA0ED362E6A591AC64EF4AE31CA8766C259ECB77D01A7F5C36B8418F91C1BEADDD4491C80F0016B66421B4B788C55127135DA2FA625FB7FD195FB40D90A6C67328602ECAF3EC4F5894BFD84A99EB4753BE0D22E0D4DE6A0ADFEDC80EB1B556749B4A8AD00E73B329C95827AB91C0256347E85E3C5FD6726D0E1FE82C925D3DF4A9  
);  
GO  
  
```  
  
### <a name="b-dropping-a-column-encryption-key-value"></a>B. Suppression d’une valeur de clé de chiffrement de colonne  
 L’exemple suivant modifie une clé de chiffrement de colonne nommée `MyCEK` en supprimant une valeur.  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
DROP VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK  
);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
 [DROP COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Always Encrypted &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)   
 [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Gérer des clés pour Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
  
  

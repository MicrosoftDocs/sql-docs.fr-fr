---
description: OPEN SYMMETRIC KEY (Transact-SQL)
title: OPEN SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN SYMMETRIC KEY
- OPEN_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], opening
- OPEN SYMMETRIC KEY statement
ms.assetid: ff019a7c-c373-46c7-ac43-ffb7e2ee60b3
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 8939563a1c5cfc2c9ca44527493fdf120d9b3897
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421874"
---
# <a name="open-symmetric-key-transact-sql"></a>OPEN SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Déchiffre une clé symétrique et la met à disposition pour l'utiliser.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
OPEN SYMMETRIC KEY Key_name DECRYPTION BY <decryption_mechanism>  
  
<decryption_mechanism> ::=  
    CERTIFICATE certificate_name [ WITH PASSWORD = 'password' ]  
    |  
    ASYMMETRIC KEY asym_key_name [ WITH PASSWORD = 'password' ]  
    |  
    SYMMETRIC KEY decrypting_Key_name  
    |  
    PASSWORD = 'decryption_password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *KEY_NAME*  
 Nom de la clé symétrique à ouvrir.  
  
 CERTIFICATE *certificate_name*  
 Nom du certificat dont la clé privée sera utilisée pour déchiffrer la clé symétrique.  
  
 ASYMMETRIC KEY *asym_key_name*  
 Nom de la clé asymétrique dont la clé privée sera utilisée pour déchiffrer la clé symétrique.  
  
 WITH PASSWORD ='*password*'  
 Mot de passe utilisé pour chiffrer la clé privée du certificat ou la clé asymétrique.  
  
 SYMMETRIC KEY *decrypting_key_name*  
 Nom de la clé symétrique qui sera utilisée pour déchiffrer la clé symétrique en cours d'ouverture.  
  
 PASSWORD ='*password*'  
 Mot de passe utilisé pour protéger la clé symétrique.  
  
## <a name="remarks"></a>Remarques  
 Les clés symétriques ouvertes sont liées à la session et non au contexte de sécurité. Une clé ouverte est disponible tant qu'elle n'a pas été explicitement fermée ou que la session n'a pas été arrêtée. Si vous ouvrez une clé symétrique puis vous changez de contexte, la clé reste ouverte et disponible dans le contexte de substitution. Des informations relatives aux clés symétriques ouvertes sont consultables dans la vue de catalogue [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md).  
  
 Si la clé symétrique a été chiffrée avec une autre clé, celle-ci doit être ouverte d'abord.  
  
 Si la clé symétrique est déjà ouverte, la requête est **NO_OP**.  
  
 Si le mot de passe, le certificat ou la clé fournie pour déchiffrer la clé symétrique est incorrecte, la requête échoue.  
  
 Les clés symétriques créées à partir d'un fournisseur de chiffrement ne peuvent pas être ouvertes. Les opérations de chiffrement et déchiffrement utilisant ce type de clé symétrique s’exécutent sans l’instruction **OPEN** car le fournisseur de chiffrement ouvre et ferme la clé.  
  
## <a name="permissions"></a>Autorisations  
 L'appelant doit disposer d'une autorisation sur la clé, et l'autorisation VIEW DEFINITION ne doit pas lui avoir été refusée sur la clé. D'autres conditions existent qui dépendent du mode de déchiffrement :  
  
-   DECRYPTION BY CERTIFICATE : autorisation CONTROL sur le certificat et connaissance du mot de passe qui chiffre la clé privée.  
  
-   DECRYPTION BY ASYMMETRIC KEY : autorisation CONTROL sur la clé asymétrique et connaissance du mot de passe qui chiffre la clé privée.  
  
-   DECRYPTION BY PASSWORD : connaissance de l'un des mots de passe utilisés pour chiffrer la clé symétrique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-opening-a-symmetric-key-by-using-a-certificate"></a>R. Ouverture d'une clé symétrique à l'aide d'un certificat  
 L'exemple suivant ouvre la clé symétrique `SymKeyMarketing3` et la déchiffre à l'aide de la clé privée du certificat `MarketingCert9`.  
  
```sql  
USE AdventureWorks2012;  
OPEN SYMMETRIC KEY SymKeyMarketing3   
    DECRYPTION BY CERTIFICATE MarketingCert9;  
GO  
```  
  
### <a name="b-opening-a-symmetric-key-by-using-another-symmetric-key"></a>B. Ouverture d'une clé symétrique à l'aide d'une autre clé symétrique  
 L'exemple suivant ouvre la clé symétrique `MarketingKey11` et la déchiffre à l'aide de la clé symétrique `HarnpadoungsatayaSE3`.  
  
```sql  
USE AdventureWorks2012;  
-- First open the symmetric key that you want for decryption.  
OPEN SYMMETRIC KEY HarnpadoungsatayaSE3   
    DECRYPTION BY CERTIFICATE sariyaCert01;  
-- Use the key that is already open to decrypt MarketingKey11.  
OPEN SYMMETRIC KEY MarketingKey11   
    DECRYPTION BY SYMMETRIC KEY HarnpadoungsatayaSE3;  
GO   
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  

---
description: ALTER SYMMETRIC KEY (Transact-SQL)
title: ALTER SYMMETRIC KEY (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SYMMETRIC KEY
- ALTER_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], modifying
- cryptography [SQL Server], symmetric keys
- ALTER SYMMETRIC KEY statement
ms.assetid: d3c776a4-7d71-4e6f-84fc-1db47400c465
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: e233b2a262edf8866e2080c9718a5bbcd29411b6
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421894"
---
# <a name="alter-symmetric-key-transact-sql"></a>ALTER SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Permet de modifier les propriétés d'une clé symétrique.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER SYMMETRIC KEY Key_name <alter_option>  
  
<alter_option> ::=  
   ADD ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
   |   
   DROP ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
<encrypting_mechanism> ::=  
   CERTIFICATE certificate_name  
   |  
   PASSWORD = 'password'  
   |  
   SYMMETRIC KEY Symmetric_Key_Name  
   |  
   ASYMMETRIC KEY Asym_Key_Name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *KEY_NAME*  
 Nom sous lequel la clé symétrique à modifier est connue dans la base de données.  
  
 ADD ENCRYPTION BY  
 Permet d'ajouter un chiffrement en utilisant la méthode spécifiée.  
  
 DROP ENCRYPTION BY  
 Permet de supprimer le chiffrement utilisant la méthode spécifiée. Vous ne pouvez pas supprimer tous les chiffrements d'une clé symétrique.  
  
 CERTIFICATE *Certificate_name*  
 Spécifie le certificat utilisé pour chiffrer la clé symétrique. Ce certificat doit déjà exister dans la base de données.  
  
 PASSWORD **='** _password_ **'**  
 Spécifie le mot de passe utilisé pour chiffrer la clé symétrique. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 SYMMETRIC KEY *Symmetric_Key_Name*  
 Spécifie la clé symétrique utilisée pour chiffrer la clé symétrique en cours de modification. Cette clé symétrique doit déjà exister dans la base de données et doit être ouverte.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Spécifie la clé asymétrique utilisée pour chiffrer la clé symétrique en cours de modification. Cette clé asymétrique doit déjà exister dans la base de données.  
  
## <a name="remarks"></a>Notes  
  
> [!CAUTION]  
>  Lorsqu'une clé symétrique est chiffrée à l'aide d'un mot de passe à la place de la clé publique de la clé principale de base de données, l'algorithme de chiffrement TRIPLE_DES est utilisé. Pour cette raison, les clés créées à l'aide d'un algorithme de chiffrement renforcé, tel qu'AES, sont elles-mêmes sécurisées par un algorithme plus faible.  
  
 Pour modifier le chiffrement de la clé symétrique, utilisez les expressions ADD ENCRYPTION et DROP ENCRYPTION. Il est impossible qu'une clé ne présente aucun chiffrement. Pour cette raison, la méthode recommandée consiste à ajouter le nouveau type de chiffrement avant de supprimer l'ancien.  
  
 Pour changer le propriétaire d’une clé symétrique, utilisez [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER sur la clé symétrique. En cas d'ajout d'un chiffrement par certificat ou clé asymétrique, l'autorisation VIEW DEFINITION est requise sur le certificat ou la clé asymétrique. En cas de suppression d'un chiffrement par certificat ou clé asymétrique, l'autorisation CONTROL est requise sur le certificat ou la clé asymétrique.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, la méthode de chiffrement utilisée pour protéger une clé symétrique est modifiée. La clé symétrique `JanainaKey043` a été chiffrée à l'aide du certificat `Shipping04` lors de sa création. Comme la clé ne peut jamais être stockée sous forme non chiffrée, dans cet exemple, le chiffrement est ajouté par mot de passe, puis le chiffrement est supprimé à l'aide d'un certificat.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey043 WITH ALGORITHM = AES_256   
    ENCRYPTION BY CERTIFICATE Shipping04;  
-- Open the key.   
OPEN SYMMETRIC KEY JanainaKey043 DECRYPTION BY CERTIFICATE Shipping04  
    WITH PASSWORD = '<enterStrongPasswordHere>';   
-- First, encrypt the key with a password.  
ALTER SYMMETRIC KEY JanainaKey043   
    ADD ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
-- Now remove encryption by the certificate.  
ALTER SYMMETRIC KEY JanainaKey043   
    DROP ENCRYPTION BY CERTIFICATE Shipping04;  
CLOSE SYMMETRIC KEY JanainaKey043;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

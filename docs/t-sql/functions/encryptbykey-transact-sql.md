---
description: ENCRYPTBYKEY (Transact-SQL)
title: ENCRYPTBYKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYKEY_TSQL
- ENCRYPTBYKEY
dev_langs:
- TSQL
helpviewer_keywords:
- authenticators [SQL Server]
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYKEY function
- ENCRYPTBYKEY function
ms.assetid: 0e11f8c5-f79d-46c1-ab11-b68ef05d6787
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b9d1a8d6e4fcc765ac0638e31e97ed8d9e7b685b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199548"
---
# <a name="encryptbykey-transact-sql"></a>ENCRYPTBYKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Chiffre les données à l'aide d'une clé symétrique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
EncryptByKey ( key_GUID , { 'cleartext' | @cleartext }  
    [, { add_authenticator | @add_authenticator }  
     , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *key_GUID*  
 GUID de la clé à utiliser pour chiffrer *cleartext*. **uniqueidentifier**.  
  
 '*cleartext*'  
 Données à chiffrer à l'aide de la clé.  
  
 @cleartext  
 Variable de type **nvarchar**, **char**, **varchar**, **binary**, **varbinary** ou **nchar** qui contient des données à chiffrer avec la clé.  
  
 *add_authenticator*  
 Indique si un authentificateur sera chiffré avec *cleartext*. Sa valeur doit être égale à 1 si vous utilisez un authentificateur. **int**.  
  
 @add_authenticator  
 Indique si un authentificateur sera chiffré avec *cleartext*. Sa valeur doit être égale à 1 si vous utilisez un authentificateur. **int**.  
  
 *authenticator*  
 Données à partir desquelles un authentificateur peut être dérivé. **sysname**.  
  
 @authenticator  
 Variable contenant les données à partir desquelles l'authentificateur sera dérivé.  
  
## <a name="return-types"></a>Types de retour  
 **varbinary** d’une taille maximale de 8 000 octets.  
  
 Retourne NULL si la clé n'est pas ouverte, si elle n'existe pas ou s'il s'agit d'une clé RC4 déconseillée et que la base de données n'est pas au niveau de compatibilité 110 ou supérieur.  
 
 Retourne NULL si la valeur *cleartext* est NULL.
  
## <a name="remarks"></a>Remarques  
 EncryptByKey utilise une clé symétrique. Cette clé doit être ouverte. Si la clé symétrique est déjà ouverte dans la session en cours, il n'est pas nécessaire de l'ouvrir à nouveau dans le contexte de la requête.  
  
 L'authentificateur vous permet d'empêcher la substitution en valeurs entières des champs chiffrés. Par exemple, considérons la table suivante des données de salaires du personnel :  
  
|Employee_ID|Standard_Title|Base_Pay|  
|------------------|---------------------|---------------|  
|345|Copy Room Assistant|Fskj%7^edhn00|  
|697|Chief Financial Officer|M0x8900f56543|  
|694|Data Entry Supervisor|Cvc97824%^34f|  
  
 Sans même casser le code de chiffrement, un utilisateur malveillant peut déduire des informations importantes à partir du contexte dans lequel le texte chiffré est stocké. Comme à la fonction de Chief Financial Officer correspond un salaire plus élevé qu'à celle de Copy Room Assistant, la valeur chiffrée en M0x8900f56543 doit être supérieure à la valeur chiffrée en Fskj%7^edhn00. S'il en est ainsi, tout utilisateur disposant de l'autorisation ALTER sur la table peut donner une augmentation à la fonction Copy Room Assistant en remplaçant les données du champ Base_Pay par une copie des données stockées dans le champ Base_Pay de la fonction Chief Financial Officer. Cette attaque par substitution complète de valeur outrepasse complètement le chiffrement.  
  
 Vous pouvez déjouer les attaques par substitution de ce type en ajoutant des informations contextuelles au texte brut avant de le chiffrer. Ces informations contextuelles sont utilisées pour vérifier que les données en texte brut n'ont pas été déplacées.  
  
 Si un authentificateur est précisé lors du chiffrement des données, il faut utiliser le même pour déchiffrer les données à l'aide de DecryptByKey. Au moment du chiffrement, un hachage de l'authentificateur est chiffré avec le texte brut. Au moment du déchiffrement, vous devez passer le même authentificateur à DecryptByKey. Si les deux ne correspondent pas, le déchiffrement échoue. Cela indique que la valeur a été déplacée depuis qu'elle a été chiffrée. Nous recommandons l'utilisation d'une colonne contenant une valeur unique et constante comme authentificateur. Si la valeur de l'authentificateur change, vous risquez de perdre l'accès aux données.  
  
 Le chiffrement et le déchiffrement symétriques sont relativement rapides et adaptés à la manipulation de grandes quantités de données.  
  
> [!IMPORTANT]  
>  L'utilisation des fonctions de chiffrement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conjointement au paramètre ANSI_PADDING OFF peut entraîner la perte de données en raison de conversions implicites. Pour plus d’informations sur ANSI_PADDING, consultez [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 La fonctionnalité illustrée dans les exemples suivants utilise les clés et les certificats créés dans [Guide pratique pour chiffrer une colonne de données](../../relational-databases/security/encryption/encrypt-a-column-of-data.md).  
  
### <a name="a-encrypting-a-string-with-a-symmetric-key"></a>R. Chiffrer une chaîne à l'aide d'une clé symétrique  
 L'exemple de code suivant ajoute une colonne à la table `Employee`, puis chiffre la valeur du numéro de sécurité sociale stockée dans la colonne `NationalIDNumber`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
  
-- Encrypt the value in column NationalIDNumber with symmetric key  
-- SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
```  
  
### <a name="b-encrypting-a-record-together-with-an-authentication-value"></a>B. Chiffrer un enregistrement avec une valeur d'authentification  
  
```sql 
USE AdventureWorks2012;  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_Encrypted varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY CreditCards_Key11  
    DECRYPTION BY CERTIFICATE Sales09;  
  
-- Encrypt the value in column CardNumber with symmetric   
-- key CreditCards_Key11.  
-- Save the result in column CardNumber_Encrypted.    
UPDATE Sales.CreditCard  
SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11'),   
    CardNumber, 1, CONVERT( varbinary, CreditCardID) );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  

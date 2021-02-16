---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a483870b4cc4d7419d853c5002f06c29b9713120
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348566"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Modifie le mot de passe utilisé pour chiffrer la clé privée d’un certificat, supprime la clé privée, ou importe la clé privée si aucune n’est présente. Affecte à un certificat la disponibilité [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *certificate_name*  
 Nom unique sous lequel le certificat est connu dans la base de données.  
  
 REMOVE PRIVATE KEY  
 Spécifie que la clé privée ne doit plus être conservée dans la base de données.  
  
 WITH PRIVATE KEY Spécifie que la clé privée du certificat est chargée dans SQL Server.

 FILE ='*path_to_private_key*'  
 Spécifie le chemin d'accès complet, y compris le nom du fichier, à la clé privée. Ce paramètre peut être un chemin d'accès local ou un chemin d'accès UNC à un emplacement réseau. L'accès au fichier a lieu dans le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque vous utilisez cette option, vous devez vérifier que le compte de service a accès au fichier spécifié.
 
 Si seul le nom de fichier est spécifié, le fichier est enregistré dans le dossier de données utilisateur par défaut de l’instance. Ce dossier peut (ou pas) être le dossier DATA de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour la base de données locale SQL Server Express, le dossier de données utilisateur par défaut de l’instance correspond au chemin d’accès spécifié par la variable d’environnement `%USERPROFILE%` pour le compte qui a créé l’instance.  
  
 BINARY ='*private_key_bits*'  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Bits de clé privée spécifiés comme constante binaire. Ces bits peuvent être sous forme chiffrée. Si chiffrés, l'utilisateur doit fournir un mot de passe de déchiffrement. Les contrôles de stratégie de mot de passe ne sont pas exécutés sur ce mot de passe. Les bits de clé privée doivent être dans un format de fichier PVK.  
  
 DECRYPTION BY PASSWORD ='*current_password*'  
 Spécifie le mot de passe exigé pour déchiffrer la clé privée.  
  
 ENCRYPTION BY PASSWORD ='*new_password*'  
 Spécifie le mot de passe utilisé pour chiffrer la clé privée du certificat dans la base de données. *new_password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Met le certificat à disposition de l'initiateur d'une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="remarks"></a>Notes  
 La clé privée doit correspondre à la clé publique spécifiée par *certificate_name*.  
  
 Vous pouvez omettre la clause DECRYPTION BY PASSWORD si le mot de passe dans le fichier est protégé par un mot de passe vide.  
  
 Lorsque la clé privée d’un certificat qui existe déjà dans la base de données est importée, elle est automatiquement protégée par la clé principale de la base de données. Pour protéger la clé privée avec un mot de passe, utilisez la clause ENCRYPTION BY PASSWORD.  
  
 L'option REMOVE PRIVATE KEY supprime de la base de données la clé privée du certificat. Il est possible de supprimer la clé privée dans le cas où le certificat sera utilisé pour vérifier des signatures ou dans des scénarios [!INCLUDE[ssSB](../../includes/sssb-md.md)] qui n’exigent pas de clé privée. Ne supprimez pas la clé privée d'un certificat qui protège une clé symétrique. La clé privée doit être restaurée pour signer les modules ou les chaînes complémentaires devant être vérifiés avec le certificat, ou pour déchiffrer une valeur ayant été chiffrée avec le certificat.   
  
 Il n'est pas nécessaire de spécifier un mot de passe de déchiffrement lorsque la clé privée est chiffrée à l'aide de la clé principale de la base de données.  
 
 Pour modifier le mot de passe utilisé pour chiffrer la clé privée, ne spécifiez pas la clause FILE ou BINARY.
  
> [!IMPORTANT]  
>  Effectuez toujours une copie de la clé privée avant de la supprimer de la base de données. Pour plus d’informations, consultez [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md) et [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md).  
  
 L'option WITH PRIVATE KEY n'est pas disponible dans une base de données autonome.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur le certificat.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>R. Suppression de la clé privée d’un certificat  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. Modification du mot de passe utilisé pour chiffrer la clé privée  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. Importation d'une clé privée pour un certificat déjà présent dans la base de données  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D. Remplacement de la protection de la clé privée par mot de passe par une protection par clé principale de la base de données  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


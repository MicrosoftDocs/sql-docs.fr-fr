---
description: BACKUP CERTIFICATE (Transact-SQL)
title: BACKUP CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DUMP_CERTIFICATE_TSQL
- BACKUP CERTIFICATE
- sql13.swb.exportcertificate.f1
- DUMP CERTIFICATE
- BACKUP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- decryption [SQL Server], certificates
- exporting certificates
- certificates [SQL Server], exporting
- BACKUP CERTIFICATE statement
- backing up certificates [SQL Server]
- decryption [SQL Server]
- cryptography [SQL Server], certificates
ms.assetid: 509b9462-819b-4c45-baae-3d2d90d14a1c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9caae25f91fcddbe5dd5cdaaa941fcfddf55c004
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199403"
---
# <a name="backup-certificate-transact-sql"></a>BACKUP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  Permet d'exporter un certificat dans un fichier.  
  
 ![Icône de lien](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server  
  
BACKUP CERTIFICATE certname TO FILE = 'path_to_file'  
    [ WITH PRIVATE KEY   
      (   
        FILE = 'path_to_private_key_file' ,  
        ENCRYPTION BY PASSWORD = 'encryption_password'   
        [ , DECRYPTION BY PASSWORD = 'decryption_password' ]   
      )   
    ]  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
BACKUP CERTIFICATE certname TO FILE ='path_to_file'  
      WITH PRIVATE KEY   
      (   
        FILE ='path_to_private_key_file',  
        ENCRYPTION BY PASSWORD ='encryption_password'   
      )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *certname*  
 Nom du certificat à sauvegarder.

 TO FILE = '*path_to_file*'  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, du fichier dans lequel le certificat doit être enregistré. Il peut s’agir d’un chemin d’accès local ou d’un chemin d’accès UNC à un emplacement réseau. Si seul un nom de fichier est spécifié, le fichier sera enregistré dans le dossier de données utilisateur de l’instance par défaut (qui peut être ou non le dossier DATA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Pour la base de données locale SQL Server Express, le dossier de données utilisateur par défaut de l’instance correspond au chemin d’accès spécifié par la variable d’environnement `%USERPROFILE%` pour le compte qui a créé l’instance.  

 WITH PRIVATE KEY Spécifie que la clé privée du certificat doit être enregistrée dans un fichier. Cette clause est facultative.

 FILE = '*path_to_private_key_file*'  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, du fichier dans lequel la clé privée doit être enregistrée. Il peut s’agir d’un chemin d’accès local ou d’un chemin d’accès UNC à un emplacement réseau. Si seul un nom de fichier est spécifié, le fichier sera enregistré dans le dossier de données utilisateur de l’instance par défaut (qui peut être ou non le dossier DATA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Pour la base de données locale SQL Server Express, le dossier de données utilisateur par défaut de l’instance correspond au chemin d’accès spécifié par la variable d’environnement `%USERPROFILE%` pour le compte qui a créé l’instance.  

 ENCRYPTION BY PASSWORD = '*encryption_password*'  
 Mot de passe utilisé pour chiffrer la clé privée avant de l'enregistrer dans le fichier de sauvegarde. Le mot de passe est sujet à des vérifications de la complexité.  
  
 DECRYPTION BY PASSWORD = '*decryption_password*'  
 Mot de passe utilisé pour déchiffrer la clé privée avant de sauvegarder la clé. Cet argument n’est pas nécessaire si le certificat est chiffré par la clé principale. 
  
## <a name="remarks"></a>Notes  
 Si la clé privée est chiffrée au moyen d'un mot de passe dans la base de données, le mot de passe de déchiffrement doit être spécifié.  
  
 Lorsque vous sauvegardez la clé privée dans un fichier, un chiffrement est nécessaire. Le mot de passe utilisé pour protéger la clé privée dans le fichier est différent de celui qui sert à chiffrer la clé privée du certificat dans la base de données.  

 Les clés privées sont enregistrées au format de fichier PVK.

 Pour restaurer un certificat sauvegardé, avec ou sans la clé privée, utilisez l’instruction [CREATE CERTIFICATE](../../t-sql/statements/create-certificate-transact-sql.md).
 
 Pour restaurer une clé privée pour un certificat existant dans la base de données, utilisez l’instruction [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md).
 
 Lors de l’exécution d’une sauvegarde, les fichiers seront mis sur une liste de contrôle d’accès du compte de service de l’instance SQL Server. Si vous souhaitez restaurer le certificat sur un serveur en cours d’exécution sous un compte différent, vous devrez ajuster les autorisations sur les fichiers afin qu’ils puissent être lus par le nouveau compte. 
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le certificat et la connaissance du mot de passe utilisé pour chiffrer la clé privée. Si seule la partie publique du certificat est sauvegardée, cette commande requiert quelques autorisations sur le certificat ; l’autorisation VIEW sur le certificat ne doit pas avoir été refusée à l’appelant.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-exporting-a-certificate-to-a-file"></a>R. Exportation d'un certificat dans un fichier  
 Dans l'exemple ci-dessous, un certificat est exporté dans un fichier.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert';  
GO  
```  
  
### <a name="b-exporting-a-certificate-and-a-private-key"></a>B. Exportation d'un certificat et d'une clé privée  
 Dans l'exemple ci-dessous, la clé privée du certificat sauvegardé sera déchiffrée au moyen du mot de passe `997jkhUbhk$w4ez0876hKHJH5gh`.  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert'  
    WITH PRIVATE KEY ( FILE = 'c:\storedkeys\sales05key' ,   
    ENCRYPTION BY PASSWORD = '997jkhUbhk$w4ez0876hKHJH5gh' );  
GO  
```  
  
### <a name="c-exporting-a-certificate-that-has-an-encrypted-private-key"></a>C. Exportation d'un certificat qui possède une clé privée chiffrée  
 Dans l'exemple ci-dessous, la clé privée du certificat est chiffrée dans la base de données. La clé privée doit être déchiffrée au moyen du mot de passe `9875t6#6rfid7vble7r`. Lorsque le certificat est stocké dans le fichier de sauvegarde, la clé privée est chiffrée au moyen du mot de passe `9n34khUbhk$w4ecJH5gh`.  
  
```sql
BACKUP CERTIFICATE sales09 TO FILE = 'c:\storedcerts\sales09cert'   
    WITH PRIVATE KEY ( DECRYPTION BY PASSWORD = '9875t6#6rfid7vble7r' ,  
    FILE = 'c:\storedkeys\sales09key' ,   
    ENCRYPTION BY PASSWORD = '9n34khUbhk$w4ecJH5gh' );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


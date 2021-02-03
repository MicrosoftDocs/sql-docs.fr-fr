---
description: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER DATABASE SCOPED CREDENTIAL
- ALTER_DATABASE_SCOPED_CREDENTIAL_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], ALTER DATABASE SCOPED CREDENTIAL statement
ms.assetid: 966b75b5-ca87-4203-8bf9-95c4e00cb0b5
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 245e99b6a377b1026c6865ac16989daf8080960c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179206"
---
# <a name="alter-database-scoped-credential-transact-sql"></a>ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie les propriétés d’informations d’identification délimitées à la base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *credential_name*  
 Spécifie le nom des informations d’identification délimitées à la base de données à modifier.  
  
 IDENTITY **='** _identity_name_*_'_*  
 Spécifie le nom du compte à utiliser lors d'une connexion en dehors du serveur. Pour importer un fichier à partir du stockage Blob Azure, il faut que le nom de l’identité soit `SHARED ACCESS SIGNATURE`.  Pour plus d’informations sur les signatures d’accès partagé, consultez [Utilisation des signatures d’accès partagé (SAP)](/azure/storage/storage-dotnet-shared-access-signature-part-1).  
    
  
 SECRET **='** _secret_*_'_*  
 Spécifie le secret requis pour l'authentification sortante. *secret* est obligatoire pour importer un fichier à partir du stockage Blob Azure. *secret* peut être facultatif à d’autres fins.   
> [!WARNING]
>  La valeur de clé SAP peut commencer par un point d’interrogation (« ? »). Quand vous utilisez la clé SAP, vous devez supprimer le caractère « ? » initial. Sinon, vos efforts risquent d’être vains.    
  
## <a name="remarks"></a>Notes  
 Quand des informations d’identification délimitées à la base de données sont modifiées, les valeurs *identity_name* et *secret* sont réinitialisées. Si l'argument facultatif SECRET n'est pas spécifié, sa valeur stockée est NULL.  
  
 Le secret est chiffré au moyen de la clé principale du service. Si cette clé est regénérée, le secret est à nouveau chiffré à l'aide de la nouvelle clé principale du service.  
  
 Des détails sur les informations d’identification délimitées à la base de données sont consultables dans la vue de catalogue [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Exige l’autorisation `ALTER` sur les informations d’identification.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-password-of-a-database-scoped-credential"></a>R. Modification du mot de passe d’informations d’identification délimitées à la base de données  
 L’exemple suivant modifie le secret stocké dans les informations d’identification délimitées à la base de données nommées `Saddles`. Les informations d’identification délimitées à la base de données contiennent la connexion Windows `RettigB` et son mot de passe. Le nouveau mot de passe est ajouté aux informations d’identification délimitées à la base de données à l’aide de la clause SECRET.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. Suppression du mot de passe d'une information d'identification  
 Le code exemple suivant supprime le mot de passe des informations d’identification délimitées à la base de données nommées `Frames`. Les informations d’identification délimitées à la base de données contiennent la connexion Windows `Aboulrus8` et un mot de passe. Après l’exécution de l’instruction, le mot de passe des informations d’identification délimitées à la base de données a la valeur NULL car l’option SECRET n’est pas spécifiée.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)   
 [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)   
 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  

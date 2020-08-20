---
description: GRANT - Octroyer des autorisations sur les informations d’identification délimitées à la base de données (Transact-SQL)
title: GRANT - Octroyer des autorisations sur les informations d’identification délimitées à la base de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- GRANT DATABASE SCOPED CREDENTIAL
- GRANT_DATABASE_SCOPED_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], database scoped credential
- permissions [SQL Server], database scoped credential
- database scoped credential [SQL Server], permissions
- GRANT statement, database scoped credentials
ms.assetid: 501f2c8a-6aeb-41af-bf0b-974d17af33c0
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f65bf32fb857b5039f6a48e26d48a8f49eca5a04
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472227"
---
# <a name="grant-database-scoped-credential-permissions-transact-sql"></a>GRANT - Octroyer des autorisations sur les informations d’identification délimitées à la base de données (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Permet d’accorder des autorisations sur des informations d’identification délimitées à la base de données. 
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT permission  [ ,...n ]    
    ON DATABASE SCOPED CREDENTIAL :: credential_name   
    TO principal [ ,...n ] [ WITH GRANT OPTION ]   
    [ AS granting_principal ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être accordée sur des informations d’identification délimitées à la base de données. Voir ci-dessous.  
  
 ON DATABASE SCOPED CREDENTIAL **::** _credential_name_  
 Spécifie les informations d’identification délimitées à la base de données sur lesquelles l’autorisation est accordée. Le qualificateur d'étendue "::" est indispensable.  
  
 *database_principal*  
 Spécifie le principal auquel l'autorisation est accordée. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
-   d'un rôle de base de données ;  
-   d'un rôle d'application ;  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
-   d'un utilisateur de base de données mappé sur un certificat ;  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
AS *granting_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit d'accorder l'autorisation. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
-   d'un rôle de base de données ;  
-   d'un rôle d'application ;  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
-   d'un utilisateur de base de données mappé sur un certificat ;  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
## <a name="remarks"></a>Remarques  
 Les informations d’identification délimitées à la base de données sont des éléments sécurisables au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être accordées sur des informations d’identification incluses dans l’étendue de base de données sont indiquées ci-dessous, avec les autorisations plus générales qui les incluent implicitement.  
  
|Autorisation sur des informations d’identification délimitées à la base de données|Implicite avec l’autorisation sur les informations d’identification délimitées à la base de données|Impliquée par une autorisation de base de données|  
|----------------------------|---------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée.  
  
 En cas d'utilisation de l'option AS, ces critères s'appliquent.  
  
|AS *granting_principal*|Autres autorisations nécessaires|  
|------------------------------|------------------------------------|  
|Utilisateur de base de données|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à une connexion Windows|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à un groupe Windows|Appartenance au groupe Windows, appartenance au rôle de base de données fixes **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à un certificat|Appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à une clé asymétrique|Appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données qui n'est mappé sur aucun principal d'un serveur|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Rôle de base de données|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Rôle d'application|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
  
 Les propriétaires d'objets peuvent accorder des autorisations sur les objets qu'ils possèdent. Les principaux ayant l'autorisation CONTROL sur un élément sécurisable peuvent accorder une autorisation sur cet élément.  
  
 Les bénéficiaires de l’autorisation CONTROL SERVER, comme les membres du rôle serveur fixe **sysadmin**, peuvent accorder n’importe quelle autorisation sur n’importe quel sécurisable du serveur. Les bénéficiaires de l’autorisation CONTROL sur une base de données, comme les membres du rôle de base de données fixe **db_owner**, peuvent accorder n’importe quelle autorisation sur n’importe quel sécurisable de la base de données. Les détenteurs de l'autorisation CONTROL sur un schéma peuvent accorder une autorisation sur n'importe quel objet dans ce schéma.  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE - Révoquer des autorisations sur des informations d’identification délimitées à la base de données (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)   
 [DENY - Refuser des autorisations sur des informations d’identification délimitées à la base de données (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

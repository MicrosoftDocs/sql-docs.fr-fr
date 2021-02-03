---
description: REVOKE (Autorisations de clé asymétrique) (Transact-SQL)
title: REVOKE - Révoquer des autorisations sur une clé asymétrique (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- REVOKE statement, asymmetric keys
ms.assetid: 1a1063e8-ffc7-4775-a40d-e155740ad7b2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3ab78d858d084a139e3932dab30990e495f6717c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209946"
---
# <a name="revoke-asymmetric-key-permissions-transact-sql"></a>REVOKE (Autorisations de clé asymétrique) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Révoque les autorisations associées à une clé asymétrique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 GRANT OPTION FOR  
 Indique que la possibilité d'accorder l'autorisation spécifiée sera révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 *permission*  
 Spécifie une autorisation qu'il est possible de révoquer sur un assembly. Voir ci-dessous.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Spécifie la clé asymétrique sur laquelle l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *database_principal*  
 Spécifie le principal pour lequel l'autorisation est révoquée. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   Utilisateur de base de données  
  
-   Rôle de base de données  
  
-   Rôle d'application  
  
-   Utilisateur de base de données mappé à une connexion Windows  
  
-   Utilisateur de base de données mappé à un groupe Windows  
  
-   Utilisateur de base de données mappé à un certificat  
  
-   Utilisateur de base de données mappé à une clé asymétrique  
  
-   Utilisateur de la base de données non associé à un principal de serveur  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels cette autorisation a été accordée ou révoquée par ce principal. L'autorisation elle-même ne sera pas révoquée.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *revoking_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de révoquer l'autorisation. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   Utilisateur de base de données  
  
-   Rôle de base de données  
  
-   Rôle d'application  
  
-   Utilisateur de base de données mappé à une connexion Windows  
  
-   Utilisateur de base de données mappé à un groupe Windows  
  
-   Utilisateur de base de données mappé à un certificat  
  
-   Utilisateur de base de données mappé à une clé asymétrique  
  
-   Utilisateur de la base de données non associé à un principal de serveur  
  
## <a name="remarks"></a>Remarques  
 Une clé asymétrique est un élément sécurisable au niveau base de données contenu dans la base de données parente dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qui peuvent être révoquées sur une clé asymétrique sont énumérées ci-dessous, ainsi que les autorisations plus générales qui les incluent par implication.  
  
|Autorisation de clé asymétrique|Impliquée par une autorisation de clé asymétrique|Impliquée par une autorisation de base de données|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur la clé asymétrique.  
  
## <a name="see-also"></a>Voir aussi  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

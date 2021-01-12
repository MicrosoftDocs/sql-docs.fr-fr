---
description: DENY - Autorisations relatives au texte intégral (Transact-SQL)
title: DENY - Autorisations relatives au texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- denying permissions [SQL Server], fulll-text
- full-text catalogs [SQL Server], permissions
- full-text stoplist [SQL Server], permissions
- DENY statement, full-text permissions
ms.assetid: d86e9a1d-0938-4ec2-a169-2d0564f3642e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fffa797dbe182b9b8111795c5ffbc670bc2b61b1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081773"
---
# <a name="deny-full-text-permissions-transact-sql"></a>DENY - Autorisations relatives au texte intégral (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Refuse des autorisations sur un catalogue de texte intégral et des listes de mots vides de texte intégral.  
  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DENY permission [ ,...n ] ON  
    FULLTEXT   
        {  
           CATALOG :: full-text_catalog_name  
           |  
           STOPLIST :: full-text_stoplist_name  
        }  
    TO database_principal [ ,...n ] [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Nom d'une autorisation. Les mappages valides des autorisations des éléments sécurisables sont décrits dans la section « Notes », plus loin dans cette rubrique.  
  
 ON FULLTEXT CATALOG **::** _full-text_catalog_name_  
 Indique le catalogue de texte intégral pour lequel l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 ON FULLTEXT STOPLIST **::** _full-text_stoplist_name_  
 Indique la liste de mots vides de texte intégral pour laquelle l'autorisation est refusée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *database_principal*  
 Spécifie le principal auquel l'autorisation est refusée. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
  
-   d'un rôle de base de données ;  
  
-   d'un rôle d'application ;  
  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
  
-   d'un utilisateur de base de données mappé sur un certificat ;  
  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
 CASCADE  
 Indique que l'autorisation à refuser est également refusée pour les autres principaux auxquels elle a été accordée par ce principal.  
  
 *denying_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de refuser l'autorisation. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
  
-   d'un rôle de base de données ;  
  
-   d'un rôle d'application ;  
  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
  
-   d'un utilisateur de base de données mappé sur un certificat ;  
  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
  
## <a name="fulltext-catalog-permissions"></a>Autorisations FULLTEXT CATALOG  
 Un catalogue de texte intégral est un élément sécurisable au niveau base de données contenu dans la base de données parente dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de refuser pour un catalogue de texte intégral sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation pour un catalogue de texte intégral|Déduite d'une autorisation pour un catalogue de texte intégral|Impliquée par une autorisation de base de données|  
|-----------------------------------|----------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="fulltext-stoplist-permissions"></a>Autorisations FULLTEXT STOPLIST  
 Une liste de mots vides de texte intégral est un élément sécurisable au niveau base de données contenu dans la base de données parente dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de refuser pour une liste de mots vides de texte intégral sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation pour une liste de mots vides de texte intégral|Impliquée par une autorisation pour une liste de mots vides de texte intégral|Impliquée par une autorisation de base de données|  
|------------------------------------|-----------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur le catalogue de texte intégral. Si vous utilisez l'option AS, le principal spécifié doit être propriétaire du catalogue de texte intégral.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT - Autorisations relatives au texte intégral &#40;Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
  
  

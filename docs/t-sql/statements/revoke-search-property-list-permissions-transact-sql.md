---
title: REVOKE - Autorisations sur une liste de propriétés de recherche
description: Révoquez des autorisations sur une liste de propriétés de recherche.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- search property lists [SQL Server], permissions
- revoking permissions [SQL Server]
- REVOKE statement, search property list permissions
ms.assetid: a6b8b37c-0015-4596-898c-51a567659e2e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: af58d742557f4cbff21a1d8f07afb73a3eaa58ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200551"
---
# <a name="revoke-search-property-list-permissions-transact-sql"></a>Autorisations de liste des propriétés de recherche REVOKE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Révoque des autorisations dans une liste de propriétés de recherche.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON  
        SEARCH PROPERTY LIST :: search_property_list_name  
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 GRANT OPTION FOR  
 Indique que le droit d'accorder l'autorisation spécifiée à d'autres principaux sera révoqué. L'autorisation elle-même ne sera pas révoquée.  
  
> [!IMPORTANT]  
>  Si le principal possède l'autorisation spécifiée sans l'option GRANT, l'autorisation elle-même sera révoquée.  
  
 *permission*  
 Nom d'une autorisation. Les mappages valides des autorisations des éléments sécurisables sont décrits dans la section « Notes », plus loin dans cette rubrique.  
  
 ON SEARCH PROPERTY LIST **::** _search_property_list_name_  
 Indique la liste de propriétés de recherche pour laquelle l'autorisation est révoquée. Le qualificateur d’étendue **::** est obligatoire.  
  
 *database_principal*  
 Spécifie le principal pour lequel l'autorisation est révoquée. Le principal peut prendre l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
  
-   d'un rôle de base de données ;  
  
-   d'un rôle d'application ;  
  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
  
-   d'un utilisateur de base de données mappé sur un certificat ;  
  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
 CASCADE  
 Indique que l'autorisation en cours de révocation est également révoquée sur les principaux auxquels elle a été accordée par ce principal.  
  
> [!CAUTION]  
>  Une révocation en cascade d'une autorisation accordée avec l'option WITH GRANT OPTION entraîne la révocation des deux options GRANT et DENY de cette autorisation.  
  
 AS *revoking_principal*  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de révoquer l'autorisation. Le principal peut prendre l'une des valeurs suivantes :  
  
-   d'un utilisateur de base de données ;  
  
-   d'un rôle de base de données ;  
  
-   d'un rôle d'application ;  
  
-   d'un utilisateur de base de données mappé sur une connexion Windows ;  
  
-   d'un utilisateur de base de données mappé sur un groupe Windows ;  
  
-   d'un utilisateur de base de données mappé sur un certificat ;  
  
-   d'un utilisateur de base de données mappé à une clé asymétrique ;  
  
-   d'un utilisateur de base de données qui n'est pas mappé sur le principal d'un serveur.  
  
## <a name="remarks"></a>Notes  
  
## <a name="search-property-list-permissions"></a>Autorisations SEARCH PROPERTY LIST  
 Une liste de propriétés de recherche est un élément sécurisable au niveau base de données inclus dans la base de données qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de révoquer pour une liste de propriétés de recherche sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de liste de propriétés de recherche|Déduite d'une autorisation de liste de propriétés de recherche|Impliquée par une autorisation de base de données|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur le catalogue de texte intégral.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DENY - Refuser des autorisations sur une liste de propriétés de recherche &#40;Transact-SQL&#41;](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT - Accorder des autorisations sur une liste de propriétés de recherche &#40;Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Rechercher les propriétés du document à l’aide des listes des propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  

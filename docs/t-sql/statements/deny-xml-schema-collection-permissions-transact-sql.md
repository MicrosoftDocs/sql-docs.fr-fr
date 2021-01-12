---
title: DENY - Autorisations sur une collection de schémas XML
description: Refusez des autorisations sur une collection de schémas XML.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], XML schema collections
- XML schema collections [SQL Server], permissions
- DENY statement, XML schema collections
- schema collections [SQL Server], permissions
ms.assetid: 159969a7-8313-41bc-bb19-c55af76597e6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e52584f3fe4bcd1528557dc04e602a3f11ca37c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081772"
---
# <a name="deny-xml-schema-collection-permissions-transact-sql"></a>DENY – refus d'autorisations de collection de schémas XML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de refuser des autorisations sur une collection de schémas XML.  
  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DENY permission  [ ,...n ] ON   
    XML SCHEMA COLLECTION :: [ schema_name . ]  
    XML_schema_collection_name  
    TO <database_principal> [ ,...n ]  
        [ CASCADE ]  
    [ AS <database_principal> ]   
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *permission*  
 Spécifie une autorisation qui peut être refusée sur une collection de schémas XML. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ON XML SCHEMA COLLECTION :: [ _schema_name_ **.** ] *XML_schema_collection_name*  
 Spécifie la collection de schémas XML sur laquelle l'autorisation doit être refusée. L'identificateur d'étendue (::) est requis. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma (.) est obligatoire.  
  
 TO \<database_principal>  
 Spécifie le principal auquel l'autorisation est refusée.  
  
 CASCADE  
 Indique que l'autorisation à refuser est également refusée pour les autres principaux auxquels elle a été accordée par ce principal.  
  
 AS \<database_principal>  
 Spécifie un principal dont le principal qui exécute cette requête dérive son droit de refuser l'autorisation.  
  
 *Database_user*  
 Spécifie un utilisateur de base de données.  
  
 *Database_role*  
 Spécifie un rôle de base de données.  
  
 *Application_role*  
 Spécifie un rôle d'application.  
  
 *Database_user_mapped_to_Windows_User*  
 Spécifie un utilisateur de base de données mappé sur un utilisateur Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
 Spécifie un utilisateur de base de données mappé à un groupe Windows.  
  
 *Database_user_mapped_to_certificate*  
 Spécifie un utilisateur de base de données mappé sur un certificat.  
  
 *Database_user_mapped_to_asymmetric_key*  
 Spécifie un utilisateur de base de données mappé à une clé asymétrique.  
  
 *Database_user_with_no_login*  
 Spécifie un utilisateur de base de données sans principal au niveau serveur correspondant.  
  
## <a name="remarks"></a>Notes  
 Des informations sur les collections de schémas XML sont consultables dans la vue de catalogue [sys.xml_schema_collections](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md).  
  
 Une collection de schémas XML est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible de refuser sur une collection de schémas XML sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de collection de schémas XML|Déduite d'une autorisation de collection de schémas XML|Déduite d'une autorisation de schéma|  
|--------------------------------------|-------------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|Exécutez|CONTROL|Exécutez|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la collection de schémas XML. Si vous utilisez l'option AS, le principal spécifié doit posséder la collection de schémas XML.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur la collection de schémas XML `Invoices4` est refusée à l'utilisateur `Wanida`. La collection de schémas XML `Invoices4` se trouve à l'intérieur du schéma `Sales` de la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
DENY EXECUTE ON XML SCHEMA COLLECTION::Sales.Invoices4 TO Wanida;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT – Accorder des autorisations sur une collection de schémas XML &#40;Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)   
 [REVOKE – Révoquer des autorisations sur une collection de schémas XML &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)   
 [sys.xml_schema_collections &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md)   
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

---
description: DENY – refus d'autorisations de type (Transact-SQL)
title: DENY – Refuser des autorisations sur un type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, types
- permissions [SQL Server], types
- type permissions [SQL Server]
- denying permissions [SQL Server], types
ms.assetid: 564e3500-c567-43dc-993b-9ab50e99cf3f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 18aa1ff5bbfdf44d17a5b107f07c623f60f2a4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211673"
---
# <a name="deny-type-permissions-transact-sql"></a>DENY – refus d'autorisations de type (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de refuser des autorisations sur un type dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DENY permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
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
 Spécifie une autorisation qui peut être refusée sur un type. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ON TYPE **::** [ _schema_name_ **.** ] *type_name*  
 Spécifie le type sur lequel l'autorisation doit être refusée. Le qualificateur d’étendue ( **::** ) est obligatoire. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma ( **.** ) est obligatoire.  
  
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
 Un type est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations.  
  
> [!IMPORTANT]  
>  Les autorisations **GRANT**, **DENY** et **REVOKE** ne s’appliquent pas aux types système. Des autorisations peuvent être accordées aux types définis par l'utilisateur. Pour plus d’informations sur les types définis par l’utilisateur, consultez [Utilisation des types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md).  
  
 Les autorisations les plus spécifiques et limitées qu'il est possible de refuser sur un type sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de type|Déduite d'une autorisation de type|Déduite d'une autorisation de schéma|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|Exécutez|CONTROL|Exécutez|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le type. Si vous utilisez la clause AS, le principal spécifié doit posséder le type sur lequel les autorisations doivent être refusées.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant refuse l'autorisation `VIEW DEFINITION` avec `CASCADE` sur le type défini par l'utilisateur `PhoneNumber` à `KhalidR`. `PhoneNumber` se trouve dans le schéma `Telemarketing`.  
  
```sql 
DENY VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT – Accorder des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)   
 [REVOKE – Révoquer des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Éléments sécurisables](../../relational-databases/security/securables.md)  
  
  

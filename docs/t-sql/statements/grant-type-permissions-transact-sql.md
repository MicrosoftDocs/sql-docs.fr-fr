---
description: GRANT – octroi d'autorisations de type (Transact-SQL)
title: GRANT – Octroyer des autorisations sur un type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], types
- granting permissions [SQL Server], types
- GRANT statement, types
- type permissions [SQL Server]
ms.assetid: 14bd2fb3-1446-49c0-be87-c6a670317ed0
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c562aac8a5b0922c235ec8c24d170fdf19c4d088
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180154"
---
# <a name="grant-type-permissions-transact-sql"></a>GRANT – octroi d'autorisations de type (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet d'accorder des autorisations sur un type.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GRANT permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
    TO <database_principal> [ ,...n ]  
    [ WITH GRANT OPTION ]  
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
 Spécifie une autorisation qui peut être accordée sur un type. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.  
  
 ON TYPE **::** [ _schema_name_ **.** ] *type_name*  
 Spécifie le type sur lequel l'autorisation doit être accordée. Le qualificateur d’étendue ( **::** ) est obligatoire. Si *schema_name* n’est pas spécifié, le schéma par défaut est utilisé. Si *schema_name* est spécifié, le qualificateur d’étendue de schéma ( **.** ) est obligatoire.  
  
 TO \<database_principal> Spécifie le principal auquel l'autorisation est accordée.  
  
 WITH GRANT OPTION  
 Indique que le principal a également la possibilité d'accorder l'autorisation spécifiée à d'autres principaux.  
  
 AS \<database_principal> Spécifie un principal dont le principal qui exécute cette requête dérive son droit d’accorder l’autorisation.  
  
 *Database_user*  
 Spécifie un utilisateur de base de données.  
  
 *Database_role*  
 Spécifie un rôle de base de données.  
  
 *Application_role*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Spécifie un rôle d'application.  
  
 *Database_user_mapped_to_Windows_User*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé sur un utilisateur Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé à un groupe Windows.  
  
 *Database_user_mapped_to_certificate*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé sur un certificat.  
  
 *Database_user_mapped_to_asymmetric_key*  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
 Spécifie un utilisateur de base de données mappé à une clé asymétrique.  
  
 *Database_user_with_no_login*  
 Spécifie un utilisateur de base de données sans principal au niveau serveur correspondant.  
  
## <a name="remarks"></a>Notes  
 Un type est un élément sécurisable de niveau schéma inclus dans le schéma qui est son parent dans la hiérarchie des autorisations.  
  
> [!IMPORTANT]  
>  Les autorisations **GRANT**, **DENY** et **REVOKE** ne s’appliquent pas aux types système. Des autorisations peuvent être accordées aux types définis par l'utilisateur. Pour plus d’informations sur les types définis par l’utilisateur, consultez [Utilisation des types définis par l’utilisateur dans SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md).  
  
 Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur un type sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.  
  
|Autorisation de type|Déduite d'une autorisation de type|Déduite d'une autorisation de schéma|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|Exécutez|CONTROL|Exécutez|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorisations  
 Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée.  
  
 Si vous utilisez l'option AS, les conditions supplémentaires ci-dessous s'appliquent.  
  
|AS|Autres autorisations nécessaires|  
|--------|------------------------------------|  
|Utilisateur de base de données|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à une connexion Windows|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à un groupe Windows|Appartenance au groupe Windows, appartenance au rôle de base de données fixes **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à un certificat|Appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données mappé à une clé asymétrique|Appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Utilisateur de base de données qui n'est mappé sur aucun principal d'un serveur|Autorisation IMPERSONATE sur l’utilisateur, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Rôle de base de données|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
|Rôle d'application|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe **db_securityadmin**, appartenance au rôle de base de données fixe **db_owner** ou appartenance au rôle serveur fixe **sysadmin**.|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant accorde l'autorisation `VIEW DEFINITION` avec `GRANT OPTION` sur le type défini par l'utilisateur `PhoneNumber` à l'utilisateur `KhalidR`. `PhoneNumber` se trouve dans le schéma `Telemarketing`.  
  
```sql  
GRANT VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DENY - Refuser des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)   
 [REVOKE – Révoquer des autorisations sur un type &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  


---
description: CREATE APPLICATION ROLE (Transact-SQL)
title: CREATE APPLICATION ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLICATION_ROLE_TSQL
- CREATE APPLICATION ROLE
- sql13.swb.applicationrole.permissions.f1
- APPLICATION
- APPLICATION ROLE
- CREATE_APPLICATION_ROLE_TSQL
- APPLICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE APPLICATION ROLE statement
- application roles [SQL Server], creating
ms.assetid: 647386da-ee80-41cf-86c9-dd590f9d66b6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 61ea2134ab91109434aab73072f8befb3d6bd29f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205806"
---
# <a name="create-application-role-transact-sql"></a>CREATE APPLICATION ROLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ajoute un rôle d'application à la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
CREATE APPLICATION ROLE application_role_name   
    WITH PASSWORD = 'password' [ , DEFAULT_SCHEMA = schema_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *application_role_name*  
 Indique le nom du rôle d'application. Ce nom ne doit pas être déjà utilisé pour référencer un principal dans la base de données.  
  
 PASSWORD **='** _password_ **'**  
 Spécifie le mot de passe que les utilisateurs de base de données doivent utiliser pour activer le rôle d'application. Vous devez toujours utiliser des mots de passe forts. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 DEFAULT_SCHEMA **=** _schéma\_nom_  
 Indique le premier schéma dans lequel le serveur doit effectuer des recherches lorsqu'il résout les noms des objets pour ce rôle. Si DEFAULT_SCHEMA n'est pas défini, le rôle d'application utilise DBO comme schéma par défaut. *schema_name* peut être un schéma qui n’existe pas dans la base de données.  
  
## <a name="remarks"></a>Remarques  
  
> [!IMPORTANT]  
>  La complexité des mots de passe est vérifiée lors de la définition des mots de passe des rôles d'application. Les applications qui appellent des rôles d'application doivent stocker leurs mots de passe. Les mots de passe des rôles d'application doivent toujours être stockés sous forme chiffrée.  
  
 Les rôles d’application sont consultables dans la vue de catalogue [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).  
  
 Pour plus d’informations sur l’utilisation des rôles d’application, consultez [Rôles d’application](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY APPLICATION ROLE sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, un rôle d'application nommé `weekly_receipts` est créé, doté du mot de passe `987Gbv876sPYY5m23` et du schéma `Sales` par défaut.  
  
```sql  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987G^bv876sPY)Y5m23'   
    , DEFAULT_SCHEMA = Sales;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles d’application](../../relational-databases/security/authentication-access/application-roles.md)   
 [sp_setapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [ALTER APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [Stratégie de mot de passe](../../relational-databases/security/password-policy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

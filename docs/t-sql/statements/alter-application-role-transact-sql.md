---
description: ALTER APPLICATION ROLE (Transact-SQL)
title: ALTER APPLICATION ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_APPLICATION_ROLE_TSQL
- ALTER APPLICATION ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying application roles
- passwords [SQL Server], application roles
- ALTER APPLICATION ROLE statement
- application roles [SQL Server], modifying
ms.assetid: c6cd5d0f-18f4-49be-b161-64d9c5569086
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f697afc214f830ad853e5e82597f749d66e82ecd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194700"
---
# <a name="alter-application-role-transact-sql"></a>ALTER APPLICATION ROLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifie le nom, le mot de passe ou le schéma par défaut d'un rôle d'application.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe
  
```syntaxsql
  
ALTER APPLICATION ROLE application_role_name
    WITH <set_item> [ ,...n ]  
  
<set_item> ::=
    NAME = new_application_role_name
    | PASSWORD = 'password'  
    | DEFAULT_SCHEMA = schema_name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *application_role_name*  
 Nom du rôle d'application à modifier.  
  
 NAME =*new_application_role_name*  
 Spécifie le nouveau nom du rôle d'application. Ce nom ne doit pas être déjà utilisé pour référencer un principal dans la base de données.  
  
 PASSWORD ='*password*'  
 Spécifie le mot de passe du rôle d'application. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous devez toujours utiliser des mots de passe forts.  
  
 DEFAULT_SCHEMA =*schema_name*  
 Indique le premier schéma dans lequel le serveur doit effectuer des recherches lorsqu'il résout les noms des objets. *schema_name* peut être un schéma qui n’existe pas dans la base de données.  
  
## <a name="remarks"></a>Notes

Si le nouveau nom du rôle d'application existe déjà dans la base de données, l'instruction échoue. Lorsque le nom, le mot de passe ou le schéma par défaut d'un rôle d'application est modifié, l'ID associé au rôle ne l'est pas.  
  
> [!IMPORTANT]  
>  Les stratégies d'expiration des mots de passe ne s'appliquent pas aux mots de passe des rôles d'application. Pour cette raison, faites extrêmement attention lorsque vous choisissez des mots de passe forts. Les applications qui appellent des rôles d'application doivent stocker leurs mots de passe.  
  
 Les rôles d'application sont visibles dans l'affichage de catalogue sys.database_principals.  
  
> [!CAUTION]  
>  Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], le comportement des schémas n’est pas le même que dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un code qui suppose que les schémas sont équivalents aux utilisateurs de base de données peut retourner des résultats incorrects. Vous ne devez pas recourir aux anciens affichages catalogue, notamment sysobjects, dans une base de données où une des instructions DDL suivantes a été utilisée : CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. Dans une base de données où une de ces instructions a été utilisée, vous devez recourir aux nouveaux affichages catalogue. Les nouveaux affichages catalogue prennent en compte la séparation des principaux et des schémas introduite dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations sur les affichages catalogue, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY APPLICATION ROLE sur la base de données. Pour modifier le schéma par défaut, l'utilisateur doit également bénéficier de l'autorisation ALTER sur le rôle d'application. Un rôle d'application peut modifier son schéma par défaut, mais pas son nom ni son mot de passe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-name-of-application-role"></a>R. Modification du nom d'un rôle d'application  
 Le code exemple suivant remplace le nom du rôle d'application `weekly_receipts` par `receipts_ledger`.  
  
```sql  
USE AdventureWorks2012;  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987Gbv8$76sPYY5m23' ,   
    DEFAULT_SCHEMA = Sales;  
GO  
ALTER APPLICATION ROLE weekly_receipts   
    WITH NAME = receipts_ledger;  
GO  
```  
  
### <a name="b-changing-the-password-of-application-role"></a>B. Modification du mot de passe d'un rôle d'application  
 Le code exemple suivant modifie le mot de passe du rôle d'application `receipts_ledger`.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH PASSWORD = '897yUUbv867y$200nk2i';  
GO  
```  
  
### <a name="c-changing-the-name-password-and-default-schema"></a>C. Modification du nom, du mot de passe et du schéma par défaut  
 Le code exemple suivant modifie simultanément le nom, le mot de passe et le schéma par défaut du rôle d'application `receipts_ledger`.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH NAME = weekly_ledger,   
    PASSWORD = '897yUUbv77bsrEE00nk2i',   
    DEFAULT_SCHEMA = Production;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles d’application](../../relational-databases/security/authentication-access/application-roles.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

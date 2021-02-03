---
description: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
title: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_AUDIT_SPECIFICATION_TSQL
- ALTER DATABASE AUDIT SPECIFICATION
- ALTER_DATABASE_AUDIT_TSQL
- ALTER DATABASE AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE AUDIT SPECIFICATION statement
ms.assetid: 85f4e7e6-a330-4de0-9048-64f386ccc314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 088c1503fd306386ca85f36be97b78dcf480e7ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195926"
---
# <a name="alter-database-audit-specification-transact-sql"></a>ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie un objet de spécification d'audit de base de données à l'aide de la fonctionnalité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    [ FOR SERVER AUDIT audit_name ]  
    [ { { ADD | DROP } (   
           { <audit_action_specification> | audit_action_group_name }   
                )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      <action_specification>[ ,...n ] ON [ class :: ] securable   
     BY principal [ ,...n ]   
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *audit_specification_name*  
 Nom de la spécification de l'audit.  
  
 *audit_name*  
 Nom de l'audit auquel cette spécification est appliquée.  
  
 *audit_action_specification*  
 Nom d'une ou plusieurs actions pouvant être auditées au niveau de la base de données. Pour obtenir la liste des groupes d’actions d’audit, consultez [Actions et groupes d’actions SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Nom d'un ou plusieurs groupes d'actions pouvant être audités au niveau de la base de données. Pour obtenir la liste des groupes d’actions d’audit, consultez [Actions et groupes d’actions SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Nom de classe (si applicable) sur l'élément sécurisable.  
  
 *securable*  
 Table, vue ou autre objet sécurisable de la base de données sur lequel appliquer l’action d’audit ou le groupe d’actions d’audit. Pour plus d'informations, consultez [Securables](../../relational-databases/security/securables.md).  
  
 *column*  
 Nom de la colonne (si applicable) sur l'élément sécurisable.  
  
 *principal*  
 Nom de principal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel appliquer l'action d'audit ou le groupe d'actions d'audit. Pour plus d’informations, consultez [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH **(** STATE **=** { ON | OFF } **)**  
 Active ou désactive la collecte d'enregistrements d'audit pour cette spécification d'audit. Les modifications de l'état de la spécification d'audit doivent être effectuées à l'extérieur d'une transaction utilisateur et elles ne peuvent pas comporter d'autres modifications dans la même instruction lorsque la transition passe de ON à OFF.  
  
## <a name="remarks"></a>Notes  
 Les spécifications d'audit de base de données sont des objets non sécurisables qui résident dans une base de données spécifiée. Vous devez définir l’état d’une spécification d’audit sur OFF pour pouvoir modifier celle d’une base de données. Si ALTER DATABASE AUDIT SPECIFICATION est exécuté alors qu'un audit est activé avec des options autres que STATE=OFF, vous recevez un message d'erreur. Pour plus d'informations, consultez [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
## <a name="permissions"></a>Autorisations  
 Les utilisateurs disposant de l'autorisation ALTER ANY DATABASE AUDIT peuvent modifier des spécifications d'audit de la base de données et les lier à un audit quelconque.  
  
 Une fois qu’une spécification d’audit de la base de données est créée, elle est consultable par des principaux disposant des autorisations CONTROL SERVER ou ALTER ANY DATABASE AUDIT, le compte sysadmin ou des principaux ayant un accès explicite à l’audit.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie une spécification d'audit de la base de données nommée `HIPAA_Audit_DB_Specification` qui audite les instructions `SELECT` par l'utilisateur `dbo`, pour un audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommé `HIPAA_Audit`.  
  
```sql  
ALTER DATABASE AUDIT SPECIFICATION HIPAA_Audit_DB_Specification  
FOR SERVER AUDIT HIPAA_Audit  
    ADD (SELECT  
         ON OBJECT::dbo.Table1  
         BY dbo)  
    WITH (STATE = ON);  
GO  
```  
  
 Pour obtenir un exemple complet de création d’audit, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

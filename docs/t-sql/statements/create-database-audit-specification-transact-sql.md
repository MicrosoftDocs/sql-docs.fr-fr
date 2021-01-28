---
title: CREATE DATABASE AUDIT SPECIFICATION
description: Créez un objet de spécification d’audit de base de données à l’aide de la fonctionnalité d’audit de SQL Server.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/03/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE DATABASE AUDIT
- DATABASE_AUDIT_SPECIFICATION_TSQL
- DATABASE AUDIT SPECIFICATION
- CREATE_DATABASE_AUDIT_SPECIFICATION_TSQL
- CREATE_DATABASE_AUDIT_TSQL
- CREATE DATABASE AUDIT SPECIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- database audit specification
- CREATE DATABASE AUDIT SPECIFICATION statement
ms.assetid: 0544da48-0ca3-4a01-ba4c-940e23dc315b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8d1f8288a0e09c3382c9c31420fb1da6083292f8
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783476"
---
# <a name="create-database-audit-specification-transact-sql"></a>CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un objet Spécification de l'audit de la base de données à l'aide de la fonctionnalité d'audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    FOR SERVER AUDIT audit_name   
        [ { ADD ( { <audit_action_specification> | audit_action_group_name } )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      action [ ,...n ]ON [ class :: ] securable BY principal [ ,...n ]  
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *audit_specification_name*  
 Nom de la spécification d’audit.  
  
 *audit_name*  
 Nom de l’audit auquel cette spécification est appliquée.  
  
 *audit_action_specification*  
 Spécification des actions exécutées sur des éléments sécurisables par des principaux qui doivent être enregistrées dans l'audit.  
  
 *action*  
 Nom d'une ou de plusieurs actions pouvant être auditées au niveau de la base de données. Pour obtenir la liste des actions d’audit, consultez [Actions et groupes d’actions SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Nom d'un ou de plusieurs groupes d'actions pouvant être auditées au niveau de la base de données. Pour obtenir la liste des groupes d’actions d’audit, consultez [Actions et groupes d’actions SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Nom de classe (le cas échéant) sur l'élément sécurisable.  
  
 *securable*  
 Table, vue ou autre objet sécurisable dans la base de données sur lequel appliquer l'action d'audit ou le groupe d'actions d'audit. Pour plus d'informations, consultez [Securables](../../relational-databases/security/securables.md).  
  
 *principal*  
 Nom du principal de la base de données sur lequel appliquer l’action d’audit ou le groupe d’actions d’audit. Pour auditer tous les principaux de base de données, utilisez le principal de base de données `public`. Pour plus d’informations, consultez [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH ( STATE = { ON | OFF } )  
 Active ou désactive la collecte d'enregistrements d'audit pour cette spécification d'audit.  
  
## <a name="remarks"></a>Notes  
 Les spécifications d'audit de base de données sont des objets non sécurisables qui résident dans une base de données spécifiée. Lorsqu'une spécification d'audit de base de données est créée, elle se trouve dans un état désactivé.  
  
## <a name="permissions"></a>Autorisations  
 Les utilisateurs dotés de l’autorisation `ALTER ANY DATABASE AUDIT` peuvent créer des spécifications d’audit de base de données et les lier à n’importe quel audit.  
  
 Une fois qu’une spécification d’audit de la base de données est créée, elle est consultable par les utilisateurs disposant de l’autorisation `CONTROL SERVER` ou du compte `sysadmin`.  
  
## <a name="examples"></a>Exemples

### <a name="a-audit-select-and-insert-on-a-table-for-any-database-principal"></a>R. Auditer SELECT et INSERT dans une table pour n’importe quel principal de base de données 
 L’exemple suivant crée un audit du serveur nommé `Payrole_Security_Audit`, puis une spécification d’audit de la base de données nommée `Payrole_Security_Audit` qui audite les instructions `SELECT` et `INSERT` par l’utilisateur `dbo`, pour la table `HumanResources.EmployeePayHistory` dans la base de données `AdventureWorks2012`.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.  
CREATE SERVER AUDIT Payrole_Security_Audit  
    TO FILE ( FILEPATH =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;  
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT Payrole_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
FOR SERVER AUDIT Payrole_Security_Audit  
ADD (SELECT , INSERT  
     ON HumanResources.EmployeePayHistory BY dbo )  
WITH (STATE = ON) ;  
GO  
``` 

### <a name="b-audit-any-dml-insert-update-or-delete-on-_all_-objects-in-the-_sales_-schema-for-a-specific-database-role"></a>B. Auditer le DML (INSERT, UPDATE ou DELETE) dans _tous_ les objets du schéma _sales_ d’un rôle de base de données  
 L’exemple suivant crée un audit du serveur nommé `DataModification_Security_Audit`, puis une spécification d’audit de la base de données nommée `Audit_Data_Modification_On_All_Sales_Tables` qui audite les instructions `INSERT`, `UPDATE` et `DELETE` par les utilisateurs d’un nouveau rôle de base de données `SalesUK`, pour tous les objets du schéma `Sales` de la base de données `AdventureWorks2012`.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.
-- Change the path to a path that the SQLServer Service has access to. 
CREATE SERVER AUDIT DataModification_Security_Audit  
    TO FILE ( FILEPATH = 
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ; 
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT DataModification_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
CREATE ROLE SalesUK
GO
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Modification_On_All_Sales_Tables  
FOR SERVER AUDIT DataModification_Security_Audit  
ADD ( INSERT, UPDATE, DELETE  
     ON Schema::Sales BY SalesUK )  
WITH (STATE = ON) ;    
GO  
```  


## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
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
  
  

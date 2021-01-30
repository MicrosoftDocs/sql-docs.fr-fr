---
description: sys.database_audit_specifications (Transact-SQL)
title: sys.database_audit_specifications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_audit_specifications_TSQL
- sys.database_audit_specifications_TSQL
- sys.database_audit_specifications
- database_audit_specifications
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_audit_specifications catalog view
ms.assetid: bf80e5c6-0588-4eb7-86ff-aa7c73461335
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: da93017ae9f419c02ece8a50731f21d4b7cf36df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210399"
---
# <a name="sysdatabase_audit_specifications-transact-sql"></a>sys.database_audit_specifications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur les spécifications de l'audit de la base de données dans un audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une instance de serveur. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Nom|**sysname**|Nom de la spécification d’audit.|  
|database_specification_id|**int**|ID de la spécification de base de données.|  
|create_date|**datetime**|Date de création de la spécification d'audit.|  
|modified_date|**datetime**|Date de dernière modification de la spécification d'audit.|  
|is_state_enabled|**bit**|État de la spécification d'audit :<br /><br /> 0-DÉSACTIVÉ<br /><br /> 1-ACTIVÉ|  
|audit_GUID|**uniqueidentifer**|GUID de l'audit qui contient cette spécification. Il est utilisé durant l'énumération des spécifications d'audit des bases de données membres pendant le démarrage ou l'attachement des bases de données.|  
  
## <a name="remarks"></a>Notes  
 Si une base de données est en mode lecture seule, la fonctionnalité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit ne peut pas ajouter de spécifications de l'audit de la base de données.  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec les autorisations **ALTER ANY DATABASE audit** ou **View definition** , le rôle DBO et les membres du rôle de base de données fixe db_owners ont accès à cet affichage catalogue. En outre, l’autorisation **View definition** ne doit pas être refusée au principal.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

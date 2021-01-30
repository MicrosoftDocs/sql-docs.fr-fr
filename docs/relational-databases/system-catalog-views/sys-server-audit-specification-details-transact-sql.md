---
description: sys.server_audit_specification_details (Transact-SQL)
title: sys.server_audit_specification_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_audit_specification_details
- sys.server_audit_specification_details_TSQL
- server_audit_specification_details_TSQL
- sys.server_audit_specification_details
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audit_specification_details catalog view
ms.assetid: 792724dc-402e-4b17-9f2c-029d910bf88e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4644b002d02ec55684201c8d9774dc12b2e15525
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159057"
---
# <a name="sysserver_audit_specification_details-transact-sql"></a>sys.server_audit_specification_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur les détails (actions) d’une spécification de l’audit du serveur dans un audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une instance de serveur. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md). Pour obtenir la liste de tous les audit_action_id et leurs noms, interrogez [sys.dm_audit_actions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|server_specification_id|**int**|ID de la spécification du serveur d'audit|  
|audit_action_id|**int**|ID de l'action d'audit|  
|audit_action_name|**sysname**|Nom du groupe ou nom de l'action d'audit|  
|class|**tinyint**|Réservé|  
|class_desc|**nvarchar(60)**|Réservé|  
|major_id|**int**|Réservé|  
|minor_id|**int**|Réservé|  
|audited_principal_id|**int**|Réservé|  
|audited_result|**nvarchar(60)**|Résultat de l'audit :<br /><br /> - SUCCESS AND FAILURE<br /><br /> - SUCCESS<br /><br /> - FAILURE|  
|is_group|**bit**|Indique si l'objet audité est un groupe :<br /><br /> 0 - Pas un groupe<br /><br /> 1 - Groupe|  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec l’autorisation **ALTER ANY Server audit** ou **View any Definition** ont accès à cet affichage catalogue. En outre, le principal ne doit pas être refusé pour **afficher une autorisation de définition** .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
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
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

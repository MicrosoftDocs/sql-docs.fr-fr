---
description: sys.dm_audit_actions (Transact-SQL)
title: sys.dm_audit_actions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_audit_actions_TSQL
- sys.dm_audit_actions
- dm_audit_actions_TSQL
- dm_audit_actions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_audit_actions dynamic management view
ms.assetid: b987c2b9-998a-4a5f-a82d-280dc6963cbe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f880fa229e64990b729d193c6e39bfb05523788
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095296"
---
# <a name="sysdm_audit_actions-transact-sql"></a>sys.dm_audit_actions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Retourne une ligne pour chaque action d'audit qui peut être signalée dans le journal d'audit et chaque groupe d'actions d'audit qui peut être configuré dans le cadre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’audit, consultez [SQL Server audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**action_id**|**varchar(4)**|ID de l'action d'audit. Relatif à la valeur **action_id** écrite dans chaque enregistrement d’audit. Autorise la valeur NULL. Elle a la valeur NULL pour les groupes d'audit.|  
|**action_in_log**|**bit**|Indique si une action peut être écrite dans un journal d'audit. Les valeurs sont les suivantes :<br /><br /> 1 = Oui<br /><br /> 0 = Non|  
|**name**|**sysname**|Nom de l'action d'audit ou du groupe d'actions d'audit. N'accepte pas la valeur NULL.|  
|**class_desc**|**nvarchar(120)**|Nom de la classe de l'objet auquel l'action d'audit s'applique. Peut être un objet de portée Serveur, Base de données ou Schéma, mais n'inclut pas les objets de schéma. N'accepte pas la valeur NULL.|  
|**parent_class_desc**|**nvarchar(120)**|Nom de la classe parente pour l'objet décrit par class_desc. Est NULL si le class_desc est Serveur.|  
|**covering_parent_action_name**|**nvarchar(120)**|Nom de l'action d'audit ou du groupe d'audit qui contient l'action d'audit décrite dans cette ligne. Sert à créer une hiérarchie d'actions. Autorise la valeur NULL.|  
|**configuration_level**|**nvarchar(10**|Indique que l'action ou le groupe d'actions spécifié dans cette ligne est configurable au niveau Groupe ou Action. Est NULL si l'action n'est pas configurable.|  
|**containing_group_name**|**nvarchar(120)**|Nom du groupe d'audit qui contient l'action spécifiée. Est NULL si la valeur de « name » est un groupe.|  
  
## <a name="permissions"></a>Autorisations  
Cette vue est visible par le public.
  
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
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

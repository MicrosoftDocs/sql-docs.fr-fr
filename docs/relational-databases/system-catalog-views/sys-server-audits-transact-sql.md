---
description: sys.server_audits (Transact-SQL)
title: sys.server_audits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_audits_TSQL
- sys.server_audits_TSQL
- sys.server_audits
- server_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audits catalog view
ms.assetid: c2c4a000-1127-46a8-b1e9-947fd1136e1e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3bb5b8b56cb2fec01b6fe655f3f51841cc7ebb21
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093085"
---
# <a name="sysserver_audits-transact-sql"></a>sys.server_audits (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une instance de serveur. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**audit_id**|**int**|ID de l'audit.|  
|**name**|**sysname**|Nom de l’audit.|  
|**audit_guid**|**uniqueidentifier**|GUID de l’audit utilisé pour énumérer les audits avec les spécifications de l’audit de la base de données du serveur membre&#124;lors du démarrage du serveur et des opérations d’attachement de la base de données.|  
|**create_date**|**datetime**|Date UTC de création de l'audit.|  
|**modify_date**|**datetime**|Date UTC de dernière modification de l'audit.|  
|**principal_id**|**int**|ID du propriétaire de l’audit, inscrit auprès du serveur.|  
|**type**|**char(2)**|Type d'audit :<br /><br /> SL-journal des événements de sécurité NT<br /><br /> AL-journal des événements de l’application NT<br /><br /> FL-fichier sur le système de fichiers|  
|**type_desc**|**nvarchar(60)**|JOURNAL DE SÉCURITÉ<br /><br /> JOURNAL DES APPLICATIONS<br /><br /> FILE|  
|**on_failure**|**tinyint**|En cas d'échec d'écriture d'une entrée d'audit :<br /><br /> 0-continuer<br /><br /> 1-arrêter l’instance de serveur<br /><br /> 2-opération d’échec|  
|**on_failure_desc**|**nvarchar(60)**|En cas d'échec d'écriture d'une entrée d'audit :<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL_OPERATION|  
|**is_state_enabled**|**tinyint**|0 - Désactivée<br /><br /> 1 – Activé|  
|**queue_delay**|**int**|Durée maximale, en millisecondes, avant d'écrire sur disque. Si la valeur 0 est spécifiée, l'audit garantira une écriture avant qu'un événement puisse continuer.|  
|**predicate**|**nvarchar (3000)**|Expression de prédicat qui est appliquée à l’événement.|  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec l’autorisation **ALTER ANY Server audit** ou **View any Definition** ont accès à cet affichage catalogue. En outre, le principal ne doit pas être refusé pour **afficher une autorisation de définition** .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
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
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

---
description: log_shipping_secondary (Transact-SQL)
title: log_shipping_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_secondary
- log_shipping_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_secondary system table
ms.assetid: 69723419-4544-49c6-a517-adb30ffa5741
author: cawrites
ms.author: chadam
ms.openlocfilehash: c4a19cbdd26e1c93e0f132c419e3fb2b2e4294a8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097438"
---
# <a name="log_shipping_secondary-transact-sql"></a>log_shipping_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke un enregistrement par ID secondaire. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**secondary_id**|**uniqueidentifier**|ID du serveur secondaire dans la configuration d'envoi de journaux.|  
|**primary_server**|**sysname**|Nom de l'instance primaire de moteur de base de données SQL Server dans la configuration d'envoi de journaux.|  
|**primary_database**|**sysname**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_source_directory**|**nvarchar (500)**|Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés.|  
|**backup_destination_directory**|**nvarchar (500)**|Répertoire sur le serveur secondaire où sont copiés les fichiers de sauvegarde.|  
|**file_retention_period**|**int**|Durée, en minutes, de conservation d'un fichier de sauvegarde sur le serveur secondaire avant sa suppression.|  
|**copy_job_id**|**uniqueidentifier**|ID associé au travail de copie sur le serveur secondaire.|  
|**restore_job_id**|**uniqueidentifier**|ID associé au travail de restauration sur le serveur secondaire.|  
|**monitor_server**|**sysname**|Nom de l’instance du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilisé comme serveur moniteur dans la configuration de la copie des journaux de session.|  
|**monitor_server_security_mode**|**bit**|Mode de sécurité utilisé pour la connexion au serveur moniteur.<br /><br /> 1 = Authentification Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**last_copied_file**|**nvarchar (500)**|Nom de fichier du dernier fichier de sauvegarde copié sur le serveur secondaire.|  
|**last_copied_date**|**datetime**|Heure et date de la dernière copie sur le serveur secondaire.|  
  
## <a name="remarks"></a>Notes  
 Plusieurs bases de données secondaires sur le même serveur secondaire pour une base de données primaire donnée partagent certains paramètres dans la table **log_shipping_secondary** . En cas de modification d'un paramètre partagé pour l'une d'entre elles, ce paramètre est modifié pour toutes.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_change_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

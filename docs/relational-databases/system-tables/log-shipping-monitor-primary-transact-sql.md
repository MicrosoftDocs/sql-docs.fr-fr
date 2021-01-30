---
description: log_shipping_monitor_primary (Transact-SQL)
title: log_shipping_monitor_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_primary
- log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_primary system table
ms.assetid: 5f629a29-1a62-40e6-ae33-6f6b7dd09a36
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8511681e673429cbe45ef3885e72fb6c5e8009e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209642"
---
# <a name="log_shipping_monitor_primary-transact-sql"></a>log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke un enregistrement de surveillance par base de données primaire dans chaque configuration de la copie des journaux de transaction. Cette table est stockée dans la base de données **msdb** .  
  
 Les tables liées à l'historique et à la surveillance sont également utilisées au niveau du serveur principal et des serveurs secondaires.   
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|ID de la base de données primaire pour la configuration de la copie des journaux de transaction.|  
|**primary_server**|**sysname**|Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session.|  
|**primary_database**|**sysname**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_threshold**|**int**|Le nombre de minutes qui peuvent s'écouler entre les opérations de sauvegarde avant le déclenchement d'une alerte.|  
|**threshold_alert**|**int**|Alerte à déclencher lorsque le seuil de sauvegarde est dépassé.|  
|**threshold_alert_enabled**|**bit**|Détermine si les alertes du seuil de sauvegarde sont activées. 1 = Activé.<br /><br /> 0 = Désactivées.|  
|**last_backup_file**|**nvarchar (500)**|Chemin d'accès absolu de la sauvegarde la plus récente des journaux de transactions.|  
|**last_backup_date**|**datetime**|Heure et date de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire.|  
|**last_backup_date_utc**|**datetime**|Date et heure de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire, exprimée en temps universel coordonné (UTC).|  
|**history_retention_period**|**int**|Durée de conservation (en minutes) avant suppression des enregistrements historiques de copie des journaux de transaction pour une base de données primaire donnée.|  
  
## <a name="remarks"></a>Notes  
 En plus d’être stockées sur le serveur moniteur distant, les informations relatives au serveur principal sont stockées sur le serveur principal dans sa table **log_shipping_monitor_primary** .  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_change_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_help_log_shipping_monitor_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)   
 [sp_delete_log_shipping_alert_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

---
description: log_shipping_monitor_history_detail (Transact-SQL)
title: log_shipping_monitor_history_detail (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_history_detail_TSQL
- log_shipping_monitor_history_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_history_detail system table
ms.assetid: 7080c888-323b-4206-a1ab-e6c51f9e2579
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2b97aaf55c31f7ae79468d4cfa16fbff947f1fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186938"
---
# <a name="log_shipping_monitor_history_detail-transact-sql"></a>log_shipping_monitor_history_detail (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke les détails de l'historique des opérations de copie des journaux de transaction. Cette table est stockée dans la base de données **msdb** .  
  
 Les tables liées à l'historique et à la surveillance sont également utilisées au niveau du serveur principal et des serveurs secondaires.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|ID principal pour la sauvegarde ou ID secondaire pour la copie ou la restauration.|  
|**agent_type**|**tinyint**|Type d'opération de copie des journaux de transaction.<br /><br /> 0 = Sauvegarde.<br /><br /> 1 = Copie.<br /><br /> 2 = Restauration.|  
|**session_id**|**int**|ID de session pour le travail de sauvegarde/copie/restauration.|  
|**database_name**|**sysname**|Nom de la base de données associée à cet enregistrement. Base de données primaire pour la sauvegarde, base de données secondaire pour la restauration ou vide pour la copie.|  
|**session_status**|**tinyint**|État de la session.<br /><br /> 0 = Démarrage.<br /><br /> 1 = Exécution.<br /><br /> 2 = Réussite.<br /><br /> 3 = Erreur.<br /><br /> 4 = Avertissement.|  
|**log_time**|**datetime**|Date et heure de création de l'enregistrement.|  
|**log_time_utc**|**datetime**|Date et heure de création de l'enregistrement, exprimée en heure UTC.|  
|**message**|**nvarchar(max)**|Texte du message.|  
  
## <a name="remarks"></a>Notes  
 Cette table contient des détails d'historique pour les agents d'envoi de journaux. Pour identifier une session d’agent, utilisez les colonnes **agent_id**, **agent_type** et **session_id**. Pour afficher les détails de l’historique de la session de l’agent, triez par **log_time**.  
  
 En plus d’être stockées sur le serveur moniteur distant, les informations relatives au serveur principal sont stockées sur le serveur principal dans la table **log_shipping_monitor_history_detail** , et les informations relatives à un serveur secondaire sont également stockées sur le serveur secondaire dans sa table **log_shipping_monitor_history_detail** .  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [Tables système &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [log_shipping_monitor_error_detail &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)  
  
  

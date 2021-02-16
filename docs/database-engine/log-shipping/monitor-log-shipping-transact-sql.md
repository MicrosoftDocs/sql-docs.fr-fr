---
title: Surveiller la copie des journaux de transaction (Transact-SQL) | Microsoft Docs
description: Découvrez les tables qui stockent l’historique contenant des informations de surveillance et des procédures stockées pour la surveillance de l’envoi de journaux dans SQL Server.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], status
- history tables [SQL Server]
- historical information [SQL Server], log shipping
- log shipping [SQL Server], monitoring
- alerts [SQL Server], log shipping
- status information [SQL Server], log shipping
- monitoring log shipping [SQL Server]
ms.assetid: acf3cd99-55f7-4287-8414-0892f830f423
author: cawrites
ms.author: chadam
ms.openlocfilehash: 389a5810611b4721fb78b3fd97f484e5ba7b1974
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346984"
---
# <a name="monitor-log-shipping-transact-sql"></a>Surveiller la copie des journaux de transaction (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Une fois l'envoi de journaux configuré, vous pouvez contrôler les informations d'état de l'ensemble des serveurs d'envoi de journaux. L'historique et l'état des opérations d'envoi de journaux sont toujours enregistrés localement par les travaux d'envoi de journaux. L'historique et l'état de l'opération de sauvegarde sont stockés sur le serveur principal, tandis que l'historique et l'état des opérations de copie et de restauration sont stockés sur le serveur secondaire. Si vous avez implémenté un serveur moniteur distant, ces informations sont également stockées sur le serveur moniteur.  
  
 Vous pouvez définir des alertes qui se déclenchent si les opérations d'envoi de journaux ne sont pas exécutées conformément à la planification. Les erreurs sont signalées par un travail d'alerte qui surveille l'état des opérations de sauvegarde et de restauration. Vous pouvez définir des alertes qui indiquent à l'opérateur l'occurrence de ces erreurs. Si un serveur moniteur est configuré, un travail d'alerte s'exécute sur le serveur moniteur qui signale les erreurs de toutes les opérations dans la configuration d'envoi des journaux. Si aucun serveur moniteur n'est défini, un travail d'alerte s'exécute sur l'instance du serveur principal qui surveille la sauvegarde. Si aucun serveur moniteur n'est défini, un travail d'alerte s'exécute également sur chaque instance de serveur secondaire pour surveiller les opérations de copie et de restauration.  
  
> [!IMPORTANT]  
>  Pour analyser une configuration d'envoi de journaux, vous devez ajouter le serveur moniteur lorsque vous activez l'envoi de journaux. Si vous ajoutez un serveur moniteur ultérieurement, vous devez supprimer la configuration d'envoi de journaux, puis la remplacer par une nouvelle configuration qui inclut un serveur moniteur. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md). Qui plus est, une fois le serveur moniteur configuré, il ne peut pas être modifié sans que l'envoi de journaux ne soit auparavant supprimé.  
  
## <a name="history-tables-containing-monitoring-information"></a>Tables d'historique contenant les informations d'analyse  
 Les tables d'historique d'analyse contiennent des métadonnées stockées sur le serveur moniteur. Une copie des informations spécifiques à un serveur principal ou secondaire est également stockée localement.  
  
 Vous pouvez interroger ces tables pour contrôler l'état d'une session d'envoi de journaux. Pour connaître l'état de l'envoi des journaux, par exemple, vérifiez l'état et l'historique du travail de sauvegarde, du travail de copie et du travail de restauration. Vous pouvez afficher un historique d'envoi de journaux et des informations d'erreur spécifiques en interrogeant les tables d'analyse.  
  
|Table de charge de travail|Description|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Contient l'ID du travail d'alerte.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Contient les informations d'erreur des travaux d'envoi des journaux. Vous pouvez interroger cette table pour identifier les erreurs d'une session d'agent. Vous pouvez également trier les erreurs en fonction de leurs date et heure d'enregistrement. Chaque erreur est consignée sous la forme d'une séquence d'exceptions, et plusieurs erreurs (séquences) peuvent être consignées pour chaque session d'agent.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Contient l'historique des agents d'envoi de journaux. Vous pouvez interroger cette table pour consulter l'historique d'une session d'agent.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Contient un enregistrement d'analyse pour la base de données principale dans chaque configuration d'envoi de journaux, y compris des informations sur le dernier fichier de sauvegarde et le dernier fichier restauré, qui sont utiles pour l'analyse.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Contient un enregistrement d'analyse pour chaque base de données secondaire, y compris des informations sur le dernier fichier de sauvegarde et le dernier fichier restauré, qui sont utiles pour l'analyse.|  
  
## <a name="stored-procedures-for-monitoring-log-shipping"></a>Procédures stockées pour l'analyse de l'envoi des journaux  
 Les informations d’analyse et d’historique sont stockées dans des tables dans **msdb**, accessibles à l’aide de procédures stockées d’envoi de journaux. Exécutez les procédures stockées suivantes sur les serveurs indiqués dans le tableau ci-dessous.  
  
|Procédure stockée|Description|Serveur concerné|  
|----------------------|-----------------|---------------------------|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Retourne les enregistrements d’analyse de la base de données primaire spécifiée, à partir de la table **log_shipping_monitor_primary** .|Serveur moniteur ou serveur principal|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Retourne les enregistrements d’analyse de la base de données secondaire définie depuis la table **log_shipping_monitor_secondary** .|Serveur moniteur ou serveur secondaire|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Retourne l'ID du travail d'alerte.|Serveur moniteur, serveur principal ou serveur secondaire si aucune surveillance n'est définie|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Extrait les paramètres de la base de données primaire et affiche les valeurs des tables **log_shipping_primary_databases** et **log_shipping_monitor_primary** .|Serveur principal|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Extrait les noms des bases de données secondaires d'une base de données principale.|Serveur principal|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Extrait les paramètres des bases de données secondaires depuis les tables **log_shipping_secondary**, **log_shipping_secondary_databases** et **log_shipping_monitor_secondary** tables.|Serveur secondaire|  
|[sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Cette procédure stockée récupère les paramètres d'une base de données primaire donnée sur le serveur secondaire.|Serveur secondaire|  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher le rapport de la copie des journaux de transaction &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)   
 [Tables et procédures stockées de copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  

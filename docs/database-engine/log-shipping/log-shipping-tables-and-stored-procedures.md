---
title: Tables et procédures stockées de copie des journaux de transaction | Microsoft Docs
description: Évaluez toutes les tables et procédures stockées associées à une configuration d’envoi de journaux. Chaque serveur stocke les tables de copie des journaux de données dans msdb.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- secondary servers [SQL Server]
- monitor servers [SQL Server]
- log shipping [SQL Server], system tables
- log shipping [SQL Server], stored procedures
- primary servers [SQL Server]
ms.assetid: 03420810-4c38-4c0c-adf0-913eb044c50a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 93990b05d791fab8edb1d7e194f5c70cb448db3e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643875"
---
# <a name="log-shipping-tables-and-stored-procedures"></a>Log Shipping Tables and Stored Procedures
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit toutes les tables et procédures stockées associées à une configuration d'envoi de journaux. Toutes les tables liées à l’envoi de journaux sont stockées, sur chaque serveur, dans **msdb** . Le tableau ci-dessous décrit les tables et les procédures stockées utilisées par serveur, au sein d'une configuration d'envoi de journaux.  
  
## <a name="primary-server-tables"></a>Tables du serveur principal  
  
|Table de charge de travail|Description|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Contient l'ID du travail d'alerte. Cette table est utilisée uniquement sur le serveur principal, dans le cas où aucun serveur moniteur distant n'a été configuré.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Stocke le détail des erreurs des travaux d'envoi de journaux associés à ce serveur principal.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Stocke le détail de l'historique des travaux d'envoi de journaux associés à ce serveur principal.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Stocke un enregistrement de surveillance pour cette base de données primaire.|  
|[log_shipping_primary_databases](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)|Contient les informations de configuration des bases de données primaires d'un serveur donné. Stocke une ligne par base de données primaire.|  
|[log_shipping_primary_secondaries](../../relational-databases/system-tables/log-shipping-primary-secondaries-transact-sql.md)|Corrèle les bases de données primaires avec les bases de données secondaires.|  
  
## <a name="primary-server-stored-procedures"></a>Procédures stockées du serveur principal  
  
|Procédure stockée|Description|  
|----------------------|-----------------|  
|[sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)|Initialise la base de données primaire d'une configuration d'envoi de journaux, ce qui inclut le travail de sauvegarde, l'enregistrement de surveillance local, et l'enregistrement de surveillance distant.|  
|[sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md)|Ajoute une base de données secondaire à une base de données primaire existante.|  
|[sp_change_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)|Modifie les paramètres de la base de données primaire, ce qui inclut l'enregistrement de surveillance local et l'enregistrement de surveillance distant.|  
|[sp_cleanup_log_shipping_history](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)|Purge l'historique localement et sur le moniteur, en fonction de la période de rétention.|  
|[sp_delete_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)|Supprime l'envoi du journal de la base de données primaire, ce qui inclut le travail de sauvegarde ainsi que les historiques local et distant.|  
|[sp_delete_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md)|Supprime le nom d'une base de données secondaire d'une base de données primaire.|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Extrait les paramètres de la base de données primaire et affiche les valeurs des tables **log_shipping_primary_databases** et **log_shipping_monitor_primary** .|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Extrait les noms des bases de données secondaires d'une base de données principale.|  
|[sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)|Actualise le moniteur et affiche les informations les plus récentes relatives à l'agent d'envoi de journal spécifié.|  
  
## <a name="secondary-server-tables"></a>Tables du serveur secondaire  
  
|Table de charge de travail|Description|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Contient l'ID du travail d'alerte. Cette table est utilisée uniquement sur le serveur secondaire, dans le cas où aucun serveur moniteur distant n'a été configuré.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Stocke le détail des erreurs des travaux d'envoi de journaux associés à ce serveur secondaire.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Stocke le détail de l'historique des travaux d'envoi de journaux associés à ce serveur secondaire.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Stocke un enregistrement de surveillance par base de données secondaire associée à ce serveur secondaire.|  
|[log_shipping_secondary](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)|Contient les informations de configuration des bases de données secondaires d'un serveur donné. Stoke une ligne par ID secondaire.|  
|[log_shipping_secondary_databases](../../relational-databases/system-tables/log-shipping-secondary-databases-transact-sql.md)|Stocke les informations de configuration d'une base de données secondaire donnée. Stoke une ligne par base de données secondaire.|  
  
> [!NOTE]  
>  Les bases de données secondaires d’une base de données primaire donnée, situées sur le même serveur secondaire, partagent les paramètres de la table **log_shipping_secondary** . Si un paramètre partagé est modifié pour une base de données secondaire, la modification est effectuée pour l'ensemble des bases de données secondaires.  
  
## <a name="secondary-server-stored-procedures"></a>Procédures stockées du serveur secondaire  
  
|Procédure stockée|Description|  
|----------------------|-----------------|  
|[sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)|Initialise une base de données secondaire pour l'envoi de journaux.|  
|[sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md)|Initialise les informations liées au serveur principal, ajoute des liens de surveillance local et distant, et crée des travaux de copie et de restauration sur le serveur secondaire pour la base de données primaire spécifiée.|  
|[sp_change_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-database-transact-sql.md)|Modifie les paramètres de la base de données secondaire, ce qui inclut les enregistrements de surveillance local et distant.|  
|[sp_change_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-primary-transact-sql.md)|Modifie les paramètres de la base de données secondaire, tels que les répertoires sources et de destination, et la période de rétention des fichiers.|  
|[sp_cleanup_log_shipping_history](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)|Purge l'historique localement et sur le moniteur, en fonction de la période de rétention.|  
|[sp_delete_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)|Supprime une base de données secondaire, ainsi que les historiques local et distant.|  
|[sp_delete_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-primary-transact-sql.md)|Supprime les informations relatives au serveur principal spécifié du serveur secondaire.|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Extrait les paramètres des bases de données secondaires à partir des tables **log_shipping_secondary**, **log_shipping_secondary_databases** et **log_shipping_monitor_secondary** .|  
|[sp_help_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Cette procédure stockée récupère les paramètres d'une base de données primaire donnée sur le serveur secondaire.|  
|[sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)|Actualise le moniteur et affiche les informations les plus récentes relatives à l'agent d'envoi de journal spécifié.|  
  
## <a name="monitor-server-tables"></a>Tables du serveur moniteur  
  
|Table de charge de travail|Description|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Contient l'ID du travail d'alerte.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Stocke les détails des erreurs des opérations de copie des journaux de transaction.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Stocke le détail de l'historique des travaux d'envoi de journaux.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Stocke un enregistrement de surveillance par base de données primaire associée à ce serveur moniteur.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Stocke un enregistrement de surveillance par base de données secondaire associée à ce serveur moniteur.|  
  
## <a name="monitor-server-stored-procedures"></a>Procédures stockées du serveur moniteur  
  
|Procédure stockée|Description|  
|----------------------|-----------------|  
|[sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md)|Crée un travail d'alerte pour l'envoi de journaux si aucun n'a encore été créé.|  
|[sp_delete_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)|Supprime un travail d'alerte pour l'envoi de journaux si aucune base de données primaire associée n'existe.|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Retourne l'ID du travail d'alerte.|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Retourne les enregistrements d’analyse de la base de données primaire spécifiée, à partir de la table **log_shipping_monitor_primary** .|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Retourne les enregistrements d’analyse de la base de données secondaire définie depuis la table **log_shipping_monitor_secondary** .|  
  
  

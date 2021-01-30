---
description: sp_add_log_shipping_secondary_database (Transact-SQL)
title: sp_add_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_secondary_database
- sp_add_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_database
ms.assetid: d29e1c24-3a3c-47a4-a726-4584afa6038a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d13a58428c4fefe2cc19668de22e604f721d0a96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206710"
---
# <a name="sp_add_log_shipping_secondary_database-transact-sql"></a>sp_add_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit une base de données secondaire pour la copie des journaux de transaction.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @secondary_database = ] 'secondary_database'` Nom de la base de données secondaire. *secondary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @primary_server = ] 'primary_server'` Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session. *primary_server* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @restore_delay = ] 'restore_delay'` Durée, en minutes, pendant laquelle le serveur secondaire attend avant de restaurer un fichier de sauvegarde donné. *restore_delay* est de **type int** et ne peut pas avoir la valeur null. La valeur par défaut est 0.  
  
`[ @restore_all = ] 'restore_all'` Si la valeur est définie sur 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles lors de l’exécution du travail de restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier est restauré. *restore_all* est de type **bit** et ne peut pas être null.  
  
`[ @restore_mode = ] 'restore_mode'` Mode de restauration de la base de données secondaire.  
  
 0 = Restauration du journal avec l'option NORECOVERY.  
  
 1 = Restauration du journal avec l'option STANDBY.  
  
 *Restore* est de type **bit** et ne peut pas être null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Si la valeur est définie sur 1, les utilisateurs sont déconnectés de la base de données secondaire lors de l’exécution d’une opération de restauration. Valeur par défaut = 0. la *déconnexion* des utilisateurs est de type **bit** et ne peut pas être null.  
  
`[ @block_size = ] 'block_size'` Taille, en octets, utilisée comme taille de bloc pour l’unité de sauvegarde. *block_size* est de **type int** avec la valeur par défaut-1.  
  
`[ @buffer_count = ] 'buffer_count'` Nombre total de mémoires tampons utilisées par l’opération de sauvegarde ou de restauration. *buffer_count* est de **type int** avec la valeur par défaut-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Taille, en octets, de la demande d’entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’unité de sauvegarde. *max_transfersize* est de **type int** et peut avoir la valeur null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Nombre de minutes autorisées à s’écouler entre les opérations de restauration avant la génération d’une alerte. *restore_threshold* est de **type int** et ne peut pas avoir la valeur null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Alerte à déclencher lorsque le seuil de sauvegarde est dépassé. *threshold_alert* est de **type int**, avec 14 420 comme valeur par défaut.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Spécifie si une alerte est déclenchée lorsque *backup_threshold* est dépassé. La valeur 1 par défaut indique que l'alerte sera générée. *threshold_alert_enabled* est de **bits**.  
  
`[ @history_retention_period = ] 'history_retention_period'` Durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est de **type int**, avec NULL comme valeur par défaut. La valeur 14 420 est utilisée si aucune autre valeur n'est spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_secondary_database** doit être exécuté à partir de la base de données **Master** sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  **sp_add_log_shipping_secondary_primary** doit être appelé avant cette procédure stockée pour initialiser les informations de la base de données principale de copie des journaux de session sur le serveur secondaire.  
  
2.  Ajoute une entrée pour la base de données secondaire dans **log_shipping_secondary_databases** à l’aide des arguments fournis.  
  
3.  Ajoute un enregistrement de moniteur local dans **log_shipping_monitor_secondary** sur le serveur secondaire à l’aide des arguments fournis.  
  
4.  Si le serveur moniteur est différent du serveur secondaire, ajoute un enregistrement de moniteur dans **log_shipping_monitor_secondary** sur le serveur moniteur à l’aide des arguments fournis.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de la procédure stockée **sp_add_log_shipping_secondary_database** pour ajouter la base de données **LogShipAdventureWorks** en tant que base de données secondaire dans une configuration d’envoi de journaux avec la base de données primaire [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] résidant sur le serveur principal Tribeca.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_database   
@secondary_database = N'LogShipAdventureWorks'   
,@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks2012'   
,@restore_delay = 0   
,@restore_mode = 1   
,@disconnect_users = 0   
,@restore_threshold = 45     
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

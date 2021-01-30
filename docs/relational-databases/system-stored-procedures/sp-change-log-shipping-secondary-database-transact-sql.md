---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 50383a1389c6327ae9caa6dcabf69bdcb8c720b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209359"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les paramètres de la base de données secondaire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
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
`[ @restore_delay = ] 'restore_delay'` Durée, en minutes, pendant laquelle le serveur secondaire attend avant de restaurer un fichier de sauvegarde donné. *restore_delay* est de **type int** et ne peut pas avoir la valeur null. La valeur par défaut est 0.  
  
`[ @restore_all = ] 'restore_all'` Si la valeur est définie sur 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles lors de l’exécution du travail de restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier a été restauré. *restore_all* est de type **bit** et ne peut pas être null.  
  
`[ @restore_mode = ] 'restore_mode'` Mode de restauration de la base de données secondaire.  
  
 0 = restaurer le journal avec NORECOVERY.  
  
 1 = Restauration du journal avec l'option STANDBY.  
  
 *Restore* est de type **bit** et ne peut pas être null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Si la valeur est définie sur 1, les utilisateurs sont déconnectés de la base de données secondaire lors de l’exécution d’une opération de restauration. Valeur par défaut = 0. *disconnect_users* est de type **bit** et ne peut pas être null.  
  
`[ @block_size = ] 'block_size'` Taille, en octets, utilisée comme taille de bloc pour l’unité de sauvegarde. *block_size* est de **type int** avec la valeur par défaut-1.  
  
`[ @buffer_count = ] 'buffer_count'` Nombre total de mémoires tampons utilisées par l’opération de sauvegarde ou de restauration. *buffer_count* est de **type int** avec la valeur par défaut-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Taille, en octets, de la demande d’entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’unité de sauvegarde. *max_transfersize* est de **type int** et peut avoir la valeur null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Nombre de minutes autorisées à s’écouler entre les opérations de restauration avant la génération d’une alerte. *restore_threshold* est de **type int** et ne peut pas avoir la valeur null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Alerte à déclencher lorsque le seuil de restauration est dépassé. *threshold_alert* est de **type int**, avec 14421 comme valeur par défaut.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Spécifie si une alerte sera déclenchée lorsque *restore_threshold* est dépassé. 1 = activées ; 0 = désactivées. *threshold_alert_enabled* est de type **bit** et ne peut pas être null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est de **type int**. La valeur 1440 est utilisée si aucun n’est spécifié.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_secondary_database** doit être exécuté à partir de la base de données **Master** sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres dans les enregistrements **log_shipping_secondary_database** si nécessaire.  
  
2.  Modifie l’enregistrement du moniteur local dans **log_shipping_monitor_secondary** sur le serveur secondaire à l’aide des arguments fournis, si nécessaire.  

## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_change_log_shipping_secondary_database** pour mettre à jour les paramètres de base de données secondaire pour la base de données **LogShipAdventureWorks**.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_change_log_shipping_primary_database (Transact-SQL)
title: sp_change_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_primary_database
- sp_change_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_primary_database
ms.assetid: 8c9dce6b-d2a3-4ca7-a832-8f59a5adb214
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e1bcd150ff810f5a4a16deb09a7ea72956ccd681
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181358"
---
# <a name="sp_change_log_shipping_primary_database-transact-sql"></a>sp_change_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les paramètres de la base de données primaire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_primary_database [ @database = ] 'database'  
[, [ @backup_directory = ] 'backup_directory']   
[, [ @backup_share = ] 'backup_share']   
[, [ @backup_retention_period = ] 'backup_retention_period']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] 'backup_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
[, [ @backup_compression = ] backup_compression_option ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @backup_directory = ] 'backup_directory'` Est le chemin d’accès au dossier de sauvegarde sur le serveur principal. *backup_directory* est de type **nvarchar (500)**, sans valeur par défaut et ne peut pas être null.  
  
`[ @backup_share = ] 'backup_share'` Est le chemin d’accès réseau au répertoire de sauvegarde sur le serveur principal. *backup_share* est de type **nvarchar (500)**, sans valeur par défaut et ne peut pas être null.  
  
`[ @backup_retention_period = ] 'backup_retention_period'` Durée, en minutes, de conservation du fichier de sauvegarde du journal dans le répertoire de sauvegarde sur le serveur principal. *backup_retention_period* est de **type int**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Mode de sécurité utilisé pour se connecter au serveur moniteur.  
  
 1 = Authentification Windows.  
  
 0 = Authentification SQL Server.  
  
 *monitor_server_security_mode* est de type **bit** et ne peut pas être null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nom d’utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Mot de passe du compte utilisé pour accéder au serveur moniteur.  
  
`[ @backup_threshold = ] 'backup_threshold'` Durée, en minutes, après la dernière sauvegarde avant qu’une erreur de *threshold_alert* ne soit générée. *backup_threshold* est de **type int**, avec une valeur par défaut de 60 minutes.  
  
`[ @threshold_alert = ] 'threshold_alert'` Alerte à déclencher lorsque le seuil de sauvegarde est dépassé. *threshold_alert* est de **type int** et ne peut pas avoir la valeur null.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Spécifie si une alerte est déclenchée lorsque *backup_threshold* est dépassé.  
  
 1 = Activé.  
  
 0 = Désactivé.  
  
 *threshold_alert_enabled* est de type **bit** et ne peut pas être null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est de **type int**. La valeur 14420 est utilisée si aucun n’est spécifié.  
  
`[ @backup_compression = ] backup_compression_option` Spécifie si une configuration d’envoi de journaux utilise la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Ce paramètre est pris en charge uniquement dans le [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure).  
  
 0 = Désactivées. Ne jamais compresser des sauvegardes de journal.  
  
 1 = Activé. Toujours compresser des sauvegardes de journal.  
  
 2 = utiliser le paramètre de la [vue ou configurer l’option de configuration de serveur compression de la sauvegarde par défaut](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Il s’agit de la valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_primary_database** doit être exécuté à partir de la base de données **Master** sur le serveur principal. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres de l’enregistrement **log_shipping_primary_database** , si nécessaire.  
  
2.  Modifie l’enregistrement local dans **log_shipping_monitor_primary** sur le serveur principal à l’aide des arguments fournis, si nécessaire.  
  
3.  Si le serveur moniteur est différent du serveur principal, modifie l’enregistrement dans **log_shipping_monitor_primary** sur le serveur moniteur à l’aide des arguments fournis, si nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_change_log_shipping_primary_database** pour mettre à jour les paramètres associés à la base de données primaire [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
EXEC master.dbo.sp_change_log_shipping_primary_database   
 @database = N'AdventureWorks'   
, @backup_directory = N'c:\LogShipping'   
, @backup_share = N'\\tribeca\LogShipping'   
, @backup_retention_period = 1440   
, @backup_threshold = 60   
, @threshold_alert = 0   
, @threshold_alert_enabled = 1   
, @history_retention_period = 1440   
,@monitor_server_security_mode = 1  
,@backup_compression = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
  
  

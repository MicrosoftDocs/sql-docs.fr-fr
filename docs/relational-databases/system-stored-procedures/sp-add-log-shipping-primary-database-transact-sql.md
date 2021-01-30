---
description: sp_add_log_shipping_primary_database (Transact-SQL)
title: sp_add_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_database
- sp_add_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_database
ms.assetid: 69531611-113f-46b5-81a6-7bf496d0353c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c967127366c935b42ac33158982c6734114b2d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206720"
---
# <a name="sp_add_log_shipping_primary_database-transact-sql"></a>sp_add_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Initialise la base de données primaire d'une configuration d'envoi de journaux, ce qui inclut le travail de sauvegarde, l'enregistrement de surveillance local, et l'enregistrement de surveillance distant.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_primary_database [ @database = ] 'database',   
[ @backup_directory = ] 'backup_directory',   
[ @backup_share = ] 'backup_share',   
[ @backup_job_name = ] 'backup_job_name',   
[, [ @backup_retention_period = ] backup_retention_period]  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] backup_threshold ]   
[, [ @threshold_alert = ] threshold_alert ]   
[, [ @threshold_alert_enabled = ] threshold_alert_enabled ]   
[, [ @history_retention_period = ] history_retention_period ]  
[, [ @backup_job_id = ] backup_job_id OUTPUT ]  
[, [ @primary_id = ] primary_id OUTPUT]  
[, [ @backup_compression = ] backup_compression_option ]  
  
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database'` Nom de la base de données primaire d’envoi de journaux. *Database est de* **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @backup_directory = ] 'backup_directory'` Est le chemin d’accès au dossier de sauvegarde sur le serveur principal. *backup_directory* est de type **nvarchar (500)**, sans valeur par défaut et ne peut pas être null.  
  
`[ @backup_share = ] 'backup_share'` Est le chemin d’accès réseau au répertoire de sauvegarde sur le serveur principal. *backup_share* est de type **nvarchar (500)**, sans valeur par défaut et ne peut pas être null.  
  
`[ @backup_job_name = ] 'backup_job_name'` Nom du travail de SQL Server Agent sur le serveur principal qui copie la sauvegarde dans le dossier de sauvegarde. *backup_job_name* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @backup_retention_period = ] backup_retention_period` Durée, en minutes, de conservation du fichier de sauvegarde du journal dans le répertoire de sauvegarde sur le serveur principal. *backup_retention_period* est de **type int**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @monitor_server = ] 'monitor_server'` Nom du serveur moniteur. *Monitor_server* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @monitor_server_security_mode = ] monitor_server_security_mode` Mode de sécurité utilisé pour se connecter au serveur moniteur.  
  
 1 = Authentification Windows.  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification. *monitor_server_security_mode* est de type **bit** et ne peut pas être null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nom d’utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Mot de passe du compte utilisé pour accéder au serveur moniteur.  
  
`[ @backup_threshold = ] backup_threshold` Durée, en minutes, après la dernière sauvegarde avant qu’une erreur de *threshold_alert* ne soit générée. *backup_threshold* est de **type int**, avec une valeur par défaut de 60 minutes.  
  
`[ @threshold_alert = ] threshold_alert` Alerte à déclencher lorsque le seuil de sauvegarde est dépassé. *threshold_alert* est de **type int**, avec 14 420 comme valeur par défaut.  
  
`[ @threshold_alert_enabled = ] threshold_alert_enabled` Spécifie si une alerte sera déclenchée lorsque *backup_threshold* est dépassé. La valeur par défaut zéro (0) indique que l'alerte est désactivée et ne sera pas déclenchée. *threshold_alert_enabled* est de **bits**.  
  
`[ @history_retention_period = ] history_retention_period` Durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est de **type int**, avec NULL comme valeur par défaut. Une valeur de 14420 sera utilisée en l'absence de toute autre spécification.  
  
`[ @backup_job_id = ] backup_job_id OUTPUT`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ID de travail de l’agent associé au travail de sauvegarde sur le serveur principal. *backup_job_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @primary_id = ] primary_id OUTPUT` ID de la base de données primaire pour la configuration de la copie des journaux de la copie. *primary_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @backup_compression = ] backup_compression_option` Spécifie si une configuration d’envoi de journaux utilise la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Ce paramètre est pris en charge uniquement dans le [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure).  
  
 0 = Désactivées. Ne jamais compresser des sauvegardes de journal.  
  
 1 = Activé. Toujours compresser des sauvegardes de journal.  
  
 2 = utiliser le paramètre de la [vue ou configurer l’option de configuration de serveur compression de la sauvegarde par défaut](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Il s’agit de la valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_primary_database** doit être exécuté à partir de la base de données **Master** sur le serveur principal. Cette procédure stockée remplit les fonctions suivantes :  
  
1.  Génère un ID primaire et ajoute une entrée pour la base de données primaire dans la table **log_shipping_primary_databases** à l’aide des arguments fournis.  
  
2.  Elle crée un travail de sauvegarde pour la base de données primaire qui est désactivée.  
  
3.  Définit l’ID de tâche de sauvegarde dans l’entrée **log_shipping_primary_databases** à l’ID de travail du travail de sauvegarde.  
  
4.  Ajoute un enregistrement de moniteur local dans la table **log_shipping_monitor_primary** sur le serveur principal à l’aide des arguments fournis.  
  
5.  Si le serveur moniteur est différent du serveur principal, ajoute un enregistrement de moniteur dans **log_shipping_monitor_primary** sur le serveur moniteur à l’aide des arguments fournis.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple ajoute la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en tant que base de données primaire dans une configuration d'envoi de journaux.  
  
```  
DECLARE @LS_BackupJobId AS uniqueidentifier ;  
DECLARE @LS_PrimaryId AS uniqueidentifier ;  
  
EXEC master.dbo.sp_add_log_shipping_primary_database   
@database = N'AdventureWorks'   
,@backup_directory = N'c:\lsbackup'   
,@backup_share = N'\\tribeca\lsbackup'   
,@backup_job_name = N'LSBackup_AdventureWorks'   
,@backup_retention_period = 1440  
,@monitor_server = N'rockaway'   
,@monitor_server_security_mode = 1   
,@backup_threshold = 60   
,@threshold_alert = 0   
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440   
,@backup_job_id = @LS_BackupJobId OUTPUT   
,@primary_id = @LS_PrimaryId OUTPUT   
,@overwrite = 1   
,@backup_compression = 0;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

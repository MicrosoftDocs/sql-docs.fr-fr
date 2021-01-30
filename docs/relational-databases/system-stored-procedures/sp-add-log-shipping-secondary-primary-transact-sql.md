---
description: sp_add_log_shipping_secondary_primary (Transact-SQL)
title: sp_add_log_shipping_secondary_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_secondary_primary_TSQL
- sp_add_log_shipping_secondary_primary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_primary
ms.assetid: bfbbbee2-c255-4a59-a963-47d6e980a8e2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7675de9f5d3e322511af474fcf9495dad3e4454a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179782"
---
# <a name="sp_add_log_shipping_secondary_primary-transact-sql"></a>sp_add_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Initialise les informations liées au serveur principal, ajoute des liens de surveillance local et distant, et crée des travaux de copie et de restauration sur le serveur secondaire pour la base de données primaire spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_secondary_primary  
 [ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[ @backup_source_directory = ] 'backup_source_directory' ,   
[ @backup_destination_directory = ] 'backup_destination_directory'  
[ @copy_job_name = ] 'copy_job_name'  
[ @restore_job_name = ] 'restore_job_name'  
[, [ @file_retention_period = ] 'file_retention_period']  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @copy_job_id = ] 'copy_job_id' OUTPUT ]  
[, [ @restore_job_id = ] 'restore_job_id' OUTPUT ]  
[, [ @secondary_id = ] 'secondary_id' OUTPUT]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @primary_server = ] 'primary_server'` Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session. *primary_server* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Répertoire où sont stockés les fichiers de sauvegarde du journal des transactions du serveur principal. *backup_source_directory* est de type **nvarchar (500)** et ne peut pas être null.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Répertoire sur le serveur secondaire sur lequel les fichiers de sauvegarde sont copiés. *backup_destination_directory* est de type **nvarchar (500)** et ne peut pas être null.  
  
`[ @copy_job_name = ] 'copy_job_name'` Nom à utiliser pour le travail de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent en cours de création pour copier les sauvegardes du journal des transactions sur le serveur secondaire. *copy_job_name* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @restore_job_name = ] 'restore_job_name'` Nom du travail de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent sur le serveur secondaire qui restaure les sauvegardes dans la base de données secondaire. *restore_job_name* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @file_retention_period = ] 'file_retention_period'` Durée, en minutes, pendant laquelle un fichier de sauvegarde est conservé sur le serveur secondaire dans le chemin d’accès spécifié par le @backup_destination_directory paramètre avant d’être supprimé. *history_retention_period* est de **type int**, avec NULL comme valeur par défaut. Une valeur de 14420 sera utilisée en l'absence de toute autre spécification.  
  
`[ @monitor_server = ] 'monitor_server'` Nom du serveur moniteur. *Monitor_server* est de **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Mode de sécurité utilisé pour se connecter au serveur moniteur.  
  
 1 = Authentification Windows.  
  
 0 = Authentication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *monitor_server_security_mode* est de type **bit** et ne peut pas être null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nom d’utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Mot de passe du compte utilisé pour accéder au serveur moniteur.  
  
`[ @copy_job_id = ] 'copy_job_id' OUTPUT` ID associé au travail de copie sur le serveur secondaire. *copy_job_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @restore_job_id = ] 'restore_job_id' OUTPUT` ID associé au travail de restauration sur le serveur secondaire. *restore_job_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @secondary_id = ] 'secondary_id' OUTPUT` ID du serveur secondaire dans la configuration de la copie des journaux de session. *secondary_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_secondary_primary** doit être exécuté à partir de la base de données **Master** sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Génère un ID secondaire pour le serveur principal et la base de données primaire spécifiés.  
  
2.  Se charge :  

    1.  Ajoute une entrée pour l’ID secondaire dans **log_shipping_secondary** à l’aide des arguments fournis.  
  
    2.  de créer un travail de copie pour l'ID secondaire qui est désactivé ;  
  
    3.  Définit l’ID de travail de copie dans l’entrée **log_shipping_secondary** à l’ID de travail du travail de copie.  
  
    4.  de créer un travail de restauration pour l'ID secondaire qui est désactivé ;  
  
    5.  Définissez l’ID de travail de restauration dans l’entrée **log_shipping_secondary** sur l’ID de travail du travail de restauration.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de la procédure stockée **sp_add_log_shipping_secondary_primary** pour configurer les informations de la base de données primaire [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur le serveur secondaire.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_primary   
@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks'   
,@backup_source_directory = N'\\tribeca\LogShipping'   
,@backup_destination_directory = N''   
,@copy_job_name = N''   
,@restore_job_name = N''   
,@file_retention_period = 1440   
,@monitor_server = N'ROCKAWAY'   
,@monitor_server_security_mode = 1   
,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT   
,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT   
,@secondary_id = @LS_Secondary__SecondaryId OUTPUT ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

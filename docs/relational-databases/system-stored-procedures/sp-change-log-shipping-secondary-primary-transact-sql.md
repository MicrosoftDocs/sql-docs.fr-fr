---
description: sp_change_log_shipping_secondary_primary (Transact-SQL)
title: sp_change_log_shipping_secondary_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_primary
- sp_change_log_shipping_secondary_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_primary
ms.assetid: 5bcb4df7-6df3-4f2b-9207-b97b5addf2a6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8352706512e71a5cba3a0b22befe2b7416d276bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209343"
---
# <a name="sp_change_log_shipping_secondary_primary-transact-sql"></a>sp_change_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les paramètres de la base de données secondaire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_secondary_primary  
[ @primary_server = ] 'primary_server',  
[ @primary_database = ] 'primary_database',  
[, [ @backup_source_directory = ] 'backup_source_directory']  
[, [ @backup_destination_directory = ] 'backup_destination_directory']  
[, [ @file_retention_period = ] file_retention_period]  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @primary_server = ] 'primary_server'` Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session. *primary_server* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Répertoire où sont stockés les fichiers de sauvegarde du journal des transactions du serveur principal. *backup_source_directory* est de type **nvarchar (500)** et ne peut pas être null.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Répertoire sur le serveur secondaire sur lequel les fichiers de sauvegarde sont copiés. *backup_destination_directory* est de type **nvarchar (500)** et ne peut pas être null.  
  
`[ @file_retention_period = ] 'file_retention_period'` Durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est de **type int**, avec NULL comme valeur par défaut. Une valeur de 14420 sera utilisée en l'absence de toute autre spécification.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` Mode de sécurité utilisé pour se connecter au serveur moniteur.  
  
 1 = Authentification Windows ;  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification. *monitor_server_security_mode* est de type **bit** et ne peut pas être null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Nom d’utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Mot de passe du compte utilisé pour accéder au serveur moniteur.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_secondary_primary** doit être exécuté à partir de la base de données **Master** sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres dans les enregistrements **log_shipping_secondary** si nécessaire.  
  
2.  Si le serveur moniteur est différent du serveur secondaire, modifie l’enregistrement du moniteur dans **log_shipping_monitor_secondary** sur le serveur moniteur à l’aide des arguments fournis, si nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

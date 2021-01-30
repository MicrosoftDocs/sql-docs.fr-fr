---
description: sp_help_log_shipping_secondary_database (Transact-SQL)
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5fc81b903438d874a80399563527747ada9ef43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199625"
---
# <a name="sp_help_log_shipping_secondary_database-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée extrait les paramètres d'une ou plusieurs bases de données secondaires.  
  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @secondary_database = ] 'secondary_database'` Nom de la base de données secondaire. *secondary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_id = ] 'secondary_id'` ID du serveur secondaire dans la configuration de la copie des journaux de session. *secondary_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**secondary_id**|ID du serveur secondaire dans la configuration d'envoi de journaux.|  
|**primary_server**|Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_source_directory**|Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés.|  
|**backup_destination_directory**|Répertoire sur le serveur secondaire où sont copiés les fichiers de sauvegarde.|  
|**file_retention_period**|Durée, en minutes, de conservation d'un fichier de sauvegarde sur le serveur secondaire avant sa suppression.|  
|**copy_job_id**|ID associé au travail de copie sur le serveur secondaire.|  
|**restore_job_id**|ID associé au travail de restauration sur le serveur secondaire.|  
|**monitor_server**|Nom de l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilisé comme serveur moniteur dans la configuration de la copie des journaux de session.|  
|**monitor_server_security_mode**|Mode de sécurité utilisé pour la connexion au serveur moniteur.<br /><br /> 1 = Authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**secondary_database**|Nom de la base de données secondaire dans la configuration de la copie des journaux de transactions.|  
|**restore_delay**|Durée, en minutes, de l'attente du serveur secondaire avant de restaurer un fichier de sauvegarde donné. La valeur par défaut est 0 minute.|  
|**restore_all**|Si la valeur est définie à 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles au moment de la restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier a été restauré.|  
|**restore_mode**|Mode de restauration pour la base de données secondaire.<br /><br /> 0 = Restauration du journal avec l'option NORECOVERY.<br /><br /> 1 = Restauration du journal en mode STANDBY.|  
|**disconnect_users**|Si la valeur est définie à 1, les utilisateurs sont déconnectés de la base de données secondaire au moment de la restauration. Valeur par défaut = 0.|  
|**block_size**|Taille, en octets, qui définit la taille des blocs pour l'unité de sauvegarde.|  
|**buffer_count**|Nombre total de mémoires tampons utilisées par l'opération de sauvegarde ou de restauration.|  
|**max_transfer_size**|Taille, en octets, de la demande d'entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'unité de sauvegarde.|  
|**restore_threshold**|Nombre de minutes pouvant s'écouler entre les opérations de restauration avant qu'une alerte ne soit générée.|  
|**threshold_alert**|Alerte à déclencher lorsque le seuil de restauration est dépassé.|  
|**threshold_alert_enabled**|Détermine si les alertes de seuil de restauration sont activées.<br /><br /> 1 = Activé.<br /><br /> 0 = Désactivées.|  
|**last_copied_file**|Nom de fichier du dernier fichier de sauvegarde copié sur le serveur secondaire.|  
|**last_copied_date**|Heure et date de la dernière copie sur le serveur secondaire.|  
|**last_copied_date_utc**|Date et heure de la dernière opération de copie sur le serveur secondaire, au format UTC (Coordinated Universal Time).|  
|**last_restored_file**|Nom du dernier fichier de sauvegarde restauré dans la base de données secondaire.|  
|**last_restored_date**|Heure et date auxquelles a eu lieu la dernière opération de restauration de la base de données secondaire.|  
|**last_restored_date_utc**|Date et heure de la dernière opération de restauration sur la base de données secondaire, au format UTC (Coordinated Universal Time).|  
|**history_retention_period**|Durée de conservation (en minutes) des enregistrements historiques d'envoi des journaux pour une base de données secondaire donnée avant leur suppression.|  
|**last_restored_latency**|Durée écoulée (en minutes) entre la création de la sauvegarde du journal sur le serveur principal et sa restauration sur le serveur secondaire.<br /><br /> La valeur initiale est NULL.|  
  
## <a name="remarks"></a>Notes  
 Si vous incluez le paramètre *secondary_database* , le jeu de résultats contient des informations sur cette base de données secondaire. Si vous incluez le paramètre *secondary_id* , le jeu de résultats contient des informations sur toutes les bases de données secondaires associées à cet ID secondaire.  
  
 **sp_help_log_shipping_secondary_database** doit être exécuté à partir de la base de données **Master** sur le serveur secondaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

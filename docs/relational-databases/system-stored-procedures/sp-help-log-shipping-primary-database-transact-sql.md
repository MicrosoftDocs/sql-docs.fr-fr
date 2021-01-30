---
description: sp_help_log_shipping_primary_database (Transact-SQL)
title: sp_help_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_primary_database_TSQL
- sp_help_log_shipping_primary_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_primary_database
ms.assetid: e711b01c-ef29-4eb6-a016-0e647e337818
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bbae989332c0aa998699aacd4f8e9d16f61e0180
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200066"
---
# <a name="sp_help_log_shipping_primary_database-transact-sql"></a>sp_help_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Extrait les paramètres de la base de données primaire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_primary_database  
[ @database = ] 'database' OR  
[ @primary_id = ] 'primary_id'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database'` Nom de la base de données primaire d’envoi de journaux. *Database est de* **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
`[ @primary_id = ] 'primary_id'` ID de la base de données primaire pour la configuration de la copie des journaux de la copie. *primary_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**primary_id**|ID de la base de données primaire pour la configuration de la copie des journaux de transaction.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_directory**|Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés.|  
|**backup_share**|Réseau ou chemin d'accès UNC au répertoire de sauvegarde.|  
|**backup_retention_period**|Durée de conservation (en minutes) avant la suppression d'un fichier de sauvegarde de journal dans le répertoire de sauvegarde.|  
|**backup_compression**|Indique si la configuration de la copie des journaux de sauvegarde utilise la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md).<br /><br /> **0** = désactivé. Ne jamais compresser des sauvegardes de journal.<br /><br /> **1** = activé. Toujours compresser des sauvegardes de journal.<br /><br /> **2** = utiliser le paramètre de la [vue ou configurer l’option de configuration de serveur compression de la sauvegarde par défaut](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Il s’agit de la valeur par défaut.<br /><br /> La compression de la sauvegarde est prise en charge uniquement dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou les versions ultérieures). Dans les autres éditions, la valeur est toujours 2.|  
|**backup_job_id**|ID de travail de l'Agent [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associé au travail de sauvegarde sur le serveur principal.|  
|**monitor_server**|Nom de l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilisé comme serveur moniteur dans la configuration de la copie des journaux de session.|  
|**monitor_server_security_mode**|Mode de sécurité utilisé pour la connexion au serveur moniteur.<br /><br /> 1 = Authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**backup_threshold**|Le nombre de minutes qui peuvent s'écouler entre les opérations de sauvegarde avant le déclenchement d'une alerte.|  
|**threshold_alert**|Alerte à déclencher lorsque le seuil de sauvegarde est dépassé.|  
|**threshold_alert_enabled**|Détermine si les alertes du seuil de sauvegarde sont activées.<br /><br /> **1** = activé.<br /><br /> **0** = désactivé.|  
|**last_backup_file**|Chemin d'accès absolu de la sauvegarde la plus récente des journaux de transactions.|  
|**last_backup_date**|Date et heure de la dernière opération de sauvegarde des journaux.|  
|**last_backup_date_utc**|Date et heure de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire, exprimée en temps universel coordonné (UTC).|  
|**history_retention_period**|Durée de conservation (en minutes) avant suppression des enregistrements historiques de copie des journaux de transaction pour une base de données primaire donnée.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_log_shipping_primary_database** doit être exécuté à partir de la base de données **Master** sur le serveur principal.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_help_log_shipping_primary_database** pour récupérer les paramètres de base de données primaire de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
EXEC master.dbo.sp_help_log_shipping_primary_database @database=N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

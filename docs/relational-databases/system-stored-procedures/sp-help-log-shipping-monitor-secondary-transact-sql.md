---
description: sp_help_log_shipping_monitor_secondary (Transact-SQL)
title: sp_help_log_shipping_monitor_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_secondary
- sp_help_log_shipping_monitor_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_secondary
ms.assetid: 3ac091ea-c9a8-4c05-a0b6-1ccf4e001339
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a16b96c6edd21e2af3a78b5fa8f91e2c6b98880
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200059"
---
# <a name="sp_help_log_shipping_monitor_secondary-transact-sql"></a>sp_help_log_shipping_monitor_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur une base de données secondaire à partir des tables du moniteur.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_monitor_secondary  
[ @secondary_server = ] 'secondary_server',  
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @secondary_server = ] 'secondary_server'` Nom du serveur secondaire. *secondary_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_database = ] 'secondary_database'` Nom de la base de données secondaire. *secondary_database* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Colonne|Description|  
|------------|-----------------|  
|**secondary_server**|Nom de l’instance secondaire du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session.|  
|**secondary_database**|Nom de la base de données secondaire dans la configuration de la copie des journaux de transactions.|  
|**secondary_id**|ID du serveur secondaire dans la configuration d'envoi de journaux.|  
|**primary_server**|Nom de l’instance principale du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
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
  
## <a name="remarks"></a>Notes  
 **sp_help_log_shipping_monitor_secondary** doit être exécuté à partir de la base de données **Master** sur le serveur moniteur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

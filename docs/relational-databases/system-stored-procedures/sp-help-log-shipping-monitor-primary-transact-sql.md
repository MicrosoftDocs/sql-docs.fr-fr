---
description: sp_help_log_shipping_monitor_primary (Transact-SQL)
title: sp_help_log_shipping_monitor_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_primary
- sp_help_log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_primary
ms.assetid: d9dfcb8f-1da6-49ca-a2c8-411574915434
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 732db5c1427526a62b8103444c0ca7063442dae6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200076"
---
# <a name="sp_help_log_shipping_monitor_primary-transact-sql"></a>sp_help_log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur une base de données primaire à partir des tables du moniteur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_monitor_primary  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @primary_server = ] 'primary_server'` Nom de l’instance principale du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session. *primary_server* est de **type sysname** et ne peut pas avoir la valeur null.  
  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**primary_id**|ID de la base de données primaire pour la configuration de la copie des journaux de transaction.|  
|**primary_server**|Nom de l’instance principale du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration de la copie des journaux de session.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_threshold**|Le nombre de minutes qui peuvent s'écouler entre les opérations de sauvegarde avant le déclenchement d'une alerte.|  
|**threshold_alert**|Alerte à déclencher lorsque le seuil de sauvegarde est dépassé.|  
|**threshold_alert_enabled**|Détermine si les alertes du seuil de sauvegarde sont activées. 1 = activées ; 0 = désactivées.|  
|**last_backup_file**|Chemin d'accès absolu de la sauvegarde la plus récente des journaux de transactions.|  
|**last_backup_date**|Heure et date de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire.|  
|**last_backup_date_utc**|Date et heure de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire, exprimée en temps universel coordonné (UTC).|  
|**history_retention_period**|Durée de conservation (en minutes) avant suppression des enregistrements historiques de copie des journaux de transaction pour une base de données primaire donnée.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_log_shipping_monitor_primary** doit être exécuté à partir de la base de données **Master** sur le serveur moniteur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

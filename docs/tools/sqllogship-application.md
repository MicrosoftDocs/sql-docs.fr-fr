---
title: Application sqllogship
description: L’application sqllogship effectue une opération de sauvegarde, de copie ou de restauration, ainsi que des tâches de nettoyage pour une configuration d’envoi de journaux dans une base de données SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqllogship
ms.assetid: 8ae70041-f3d9-46e4-8fa8-31088572a9f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 349b8f5bd757e623b79b043d14ce9de91c34e942
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341984"
---
# <a name="sqllogship-application"></a>Application sqllogship
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L’application **sqllogship** effectue une opération de sauvegarde, de copie ou de restauration, ainsi que les tâches de nettoyage associées pour une configuration d’envoi de journaux. L'opération a lieu sur une instance spécifique de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour une base de données spécifique.  
  
 ![Icône de lien de la rubrique](../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") Pour les conventions de syntaxe, consultez [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../tools/command-prompt-utility-reference-database-engine.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqllogship -server instance_name { -backup primary_id | -copy secondary_id | -restore secondary_id } [ -verboselevel level ] [ -logintimeout timeout_value ] [ -querytimeout timeout_value ]  
```  
  
## <a name="arguments"></a>Arguments  
 **-server** _instance_name_  
 Spécifie l'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] où va s'exécuter l'opération. L'instance de serveur à spécifier dépend du type de l'opération d'envoi de journaux spécifié. Pour **-backup**, *instance_name* doit correspondre au nom du serveur principal dans une configuration d’envoi de journaux. Pour **-copy** ou **-restore**, *instance_name* doit correspondre au nom d’un serveur secondaire dans une configuration d’envoi de journaux.  
  
 **-backup** _primary_id_  
 Effectue une opération de sauvegarde pour la base de données principale dont l’ID principal est spécifié par *primary_id*. Vous pouvez obtenir cet ID en le sélectionnant dans la table système [log_shipping_primary_databases](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md) ou en utilisant la procédure stockée [sp_help_log_shipping_primary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md) .  
  
 L'opération de sauvegarde crée la sauvegarde du journal dans le répertoire de sauvegarde. L'application **sqllogship** nettoie ensuite les anciens fichiers de sauvegarde en fonction de la durée de rétention des fichiers. Puis, l'application enregistre l'historique de l'opération de sauvegarde sur le serveur principal et le serveur moniteur. Enfin, l’application exécute [sp_cleanup_log_shipping_history](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)qui nettoie les anciennes informations d’historique en fonction de la période de rétention.  
  
 **-copy** _secondary_id_  
 Effectue une opération de copie afin de copier des sauvegardes du serveur secondaire spécifié pour la ou les bases de données secondaires dont l’ID secondaire est spécifié par *secondary_id*. Vous pouvez obtenir cet ID en le sélectionnant dans la table système [log_shipping_secondary](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md) ou en utilisant la procédure stockée [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .  
  
 L'opération copie les fichiers de sauvegarde du répertoire de sauvegarde vers le répertoire de destination. L'application **sqllogship** enregistre ensuite l'historique de l'opération de copie sur le serveur secondaire et le serveur moniteur.  
  
 **-restore** _secondary_id_  
 Effectue une opération de restauration sur le serveur secondaire spécifié pour la ou les bases de données secondaires dont l’ID secondaire est spécifié par *secondary_id*. Vous pouvez obtenir cet ID à l’aide de la procédure stockée **sp_help_log_shipping_secondary_database** .  
  
 Les fichiers de sauvegarde du répertoire de destination créés après le point de restauration le plus récent sont restaurés dans la ou les bases de données secondaires. L'application **sqllogship** nettoie ensuite les anciens fichiers de sauvegarde en fonction de la durée de rétention des fichiers. Puis, l'application enregistre l'historique de l'opération de restauration sur le serveur secondaire et le serveur moniteur. Enfin, l’application exécute **sp_cleanup_log_shipping_history** qui nettoie les anciennes informations d’historique en fonction de la période de rétention.  
  
 **-verboselevel** _level_  
 Spécifie le niveau des messages ajoutés à l'historique d'envoi des journaux. *level* est l'un des entiers suivants :  
  
|Level|Description|  
|-----------|-----------------|  
|0|N'envoie en sortie aucun message de traçage et de débogage.|  
|1|Envoie en sortie des messages de gestion des erreurs.|  
|2|Envoie en sortie des messages de gestion des erreurs et d'avertissement.|  
|**3**|Envoie en sortie des messages de gestion des erreurs, d'avertissement et d'information. Il s’agit de la valeur par défaut.|  
|4|Envoie en sortie tous les messages de traçage et de débogage.|  
  
 **-logintimeout** _timeout_value_  
 Spécifie le délai accordé pour se connecter à l'instance de serveur avant l'expiration de la tentative. La valeur par défaut est 15 secondes. *timeout_value* a la valeur **int** _._  
  
 **-querytimeout** _timeout_value_  
 Spécifie le délai alloué au démarrage de l'opération spécifiée avant l'expiration de la tentative. Le paramètre par défaut est l'absence de délai d'attente. *timeout_value* a la valeur **int** _._  
  
## <a name="remarks"></a>Notes  
 Il est recommandé d'utiliser les travaux de sauvegarde, de copie et de restauration pour effectuer les opérations correspondantes quand cela est possible. Pour démarrer ces travaux à partir d’une opération de traitement ou d’une autre application, appelez la procédure stockée [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) .  
  
 L'historique d'envoi de journaux créé par **sqllogship** comprend également l'historique des travaux de restauration, de copie et de sauvegarde de l'envoi de journaux. Si vous envisagez d'utiliser **sqllogship** de manière répétée pour effectuer des opérations de restauration, de copie ou de sauvegarde pour une configuration de l'envoi de journaux, pensez à désactiver le ou les travaux d'envoi de journaux correspondants. Pour plus d’informations, consultez [Disable or Enable a Job](../ssms/agent/disable-or-enable-a-job.md).  
  
 L’application **sqllogship** , SqlLogShip.exe, est installée dans le répertoire x:\Program Files\Microsoft SQL Server\130\Tools\Binn.  
  
## <a name="permissions"></a>Autorisations  
 **sqllogship** utilise l'authentification Windows. Le compte d'authentification Windows où s'exécute la commande nécessite un accès au répertoire Windows et des autorisations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La configuration requise dépend de l’option spécifiée par la commande **sqllogship** : **-backup**, **-copy** ou **-restore** .  
  
|Option|Accès au répertoire|Autorisations|  
|------------|----------------------|-----------------|  
|**-backup**|Nécessite un accès en lecture/écriture au répertoire de sauvegarde.|Nécessite les mêmes autorisations que l'instruction BACKUP. Pour plus d’informations, consultez [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md).|  
|**-copy**|Nécessite l'accès en lecture au répertoire de sauvegarde et l'accès en écriture au répertoire de copie.|Nécessite les mêmes autorisations que la procédure stockée [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .|  
|**-restore**|Nécessite un accès en lecture/écriture au répertoire de copie.|Nécessite les mêmes autorisations que l'instruction RESTORE. Pour plus d’informations, consultez [RESTORE &#40;Transact-SQL&#41;](../t-sql/statements/restore-statements-transact-sql.md).|  
  
> [!NOTE]  
>  Pour connaître les chemins des répertoires de sauvegarde et de copie, exécutez la procédure stockée **sp_help_log_shipping_secondary_database** ou consultez la table **log_shipping_secondary** dans **msdb**. Les chemins du répertoire de sauvegarde et du répertoire de destination sont dans les colonnes **backup_source_directory** et **backup_destination_directory** respectivement.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)  
  
  

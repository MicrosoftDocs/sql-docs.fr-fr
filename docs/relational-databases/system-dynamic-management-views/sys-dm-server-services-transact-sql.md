---
description: sys.dm_server_services (Transact-SQL)
title: sys.dm_server_services (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/07/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_server_services
- sys.dm_server_services
- sys.dm_server_services_TSQL
- dm_server_services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_services dynamic management view
ms.assetid: 3f0defd0-478d-4e7f-96be-8795c9de4e3f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7b37fc021c8d324076e81c8f864099ddd9fdad14
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235948"
---
# <a name="sysdm_server_services-transact-sql"></a>sys.dm_server_services (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les services de SQL Server, de texte intégral SQL Server Launchpad (SQL Server 2017 +) et de SQL Server Agent dans l’instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez cette vue de gestion dynamique pour signaler des informations d'état sur ces services.  
  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|servicename|**nvarchar (256)**|Nom du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] service, de texte intégral ou de SQL Server Agent. Ne peut pas être null.|  
|startup_type|**int**|Indique le mode de démarrage du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> 0 : autres<br />1 : autres<br />2 : automatique<br />3 : Manuel<br />4 : désactivé<br /><br /> Autorise la valeur NULL.|  
|startup_type_desc|**nvarchar (256)**|Décrit le mode de démarrage du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> Autre : autre (démarrage du démarrage)<br />Autre : autre (démarrage système)<br />Automatique : démarrage automatique<br />Manuel : début de la demande<br />Désactivé : désactivé<br /><br /> Ne peut pas être null.|  
|status|**int**|Indique l'état actuel du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> 1 : arrêté<br />2 : autre (démarrage en attente)<br />3 : autre (arrêt en attente)<br />4 : en cours d’exécution<br />5 : autre (toujours en attente)<br />6 : autre (pause en attente)<br />7 : en pause<br /><br /> Autorise la valeur NULL.|  
|status_desc|**nvarchar (256)**|Décrit l'état actuel du service. Voici les valeurs possibles et leurs descriptions correspondantes.<br /><br /> Arrêté : le service est arrêté.<br />Autre (opération de démarrage en attente) : le service est en cours de démarrage.<br />Autre (opération d’arrêt en attente) : le service est en cours d’arrêt.<br />En cours d’exécution : le service est en cours d’exécution.<br />Autre (opérations de reprise en attente) : le service est dans un état d’attente.<br />Autre (pause en attente) : le service est en cours de suspension.<br />Paused : le service est suspendu.<br /><br /> Ne peut pas être null.|  
|process_id|**int**|ID de processus du service. Ne peut pas être null.|  
|last_startup_time|**datetimeoffset(7)**|Date et heure du dernier démarrage du service. Autorise la valeur NULL.|  
|service_account|**nvarchar (256)**|Compte autorisé à contrôler le service. Ce compte peut démarrer ou arrêter le service ou modifier ses propriétés. Ne peut pas être null.|  
|nom_fichier|**nvarchar (256)**|Chemin d'accès et nom de fichier de l'exécutable du service. Ne peut pas être null.|  
|is_clustered|**nvarchar (1)**|Indique si le service est installé en tant que ressource d'un serveur en cluster. Ne peut pas être null.|  
|cluster_nodename|**nvarchar (256)**|Nom du nœud de cluster sur lequel le service est installé. Autorise la valeur NULL.|
|instant_file_initialization_enabled|**nvarchar (1)**|Spécifie si l’initialisation instantanée des fichiers est activée pour le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] service.<br /><br />Y = l’initialisation instantanée des fichiers est activée pour le service.<br /><br />N = l’initialisation instantanée des fichiers est désactivée pour le service.<br /><br /> Autorise la valeur NULL.<br /><br /> **Remarque :** Ne s’applique pas à d’autres services tels que le SQL Server Agent.<br /><br /> **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (À partir de [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4 et [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 et versions ultérieures).|  

## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_server_registry &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md)  
  

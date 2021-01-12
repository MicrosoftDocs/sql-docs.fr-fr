---
description: MSsubscription_agents (Transact-SQL)
title: MSsubscription_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_agents
- MSsubscription_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_agents system table
ms.assetid: 86ad5891-0bef-4963-9381-7d5b45245a0c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8569eba1925879f2f757953f36e4ae632292100
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094713"
---
# <a name="mssubscription_agents-transact-sql"></a>MSsubscription_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSsubscription_agents** est utilisée par agent de distribution et les déclencheurs d’abonnements modifiables pour suivre les propriétés de l’abonnement. Cette table est stockée dans la base de données d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de la ligne.|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**subscription_type**|**int**|Type d'abonnement :<br /><br /> 0 = Par envoi de données (push).<br /><br /> 1 = par extraction de données (pull)<br /><br /> 2 = par extraction de données anonyme|  
|**queue_id**|**sysname**|ID de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] file d’attente de messages sur le serveur de publication. *queue_id* est défini sur **SQL** pour la mise à jour en file d’attente basée sur SQL.|  
|**update_mode**|**tinyint**|Type de mise à jour :<br /><br /> **0** = lecture seule.<br /><br /> **1** = mise à jour immédiate.<br /><br /> **2** = mise à jour en file d’attente à l’aide de Message Queuing.<br /><br /> **3** = mise à jour immédiate avec mise à jour en file d’attente comme basculement à l’aide de Message Queuing.<br /><br /> **4** = mise à jour en file d’attente à l’aide de SQL Server file d’attente.<br /><br /> **5** = mise à jour immédiate avec basculement de mise à jour en file d’attente à l’aide de SQL Server file d’attente.|  
|**failover_mode**|**bit**|Si un type de basculement de mise à jour a été sélectionné, ce paramètre représente le type de basculement choisi :<br /><br /> **0** = mise à jour immédiate en cours d’utilisation. Le basculement n'est pas activé.<br /><br /> **1** = mise à jour en file d’attente utilisée. Le basculement est activé. La file d’attente utilisée pour le basculement est spécifiée dans la valeur *update_mode* .|  
|**spid**|**int**|ID de processus système de la connexion utilisée par l'Agent de distribution en cours d'exécution ou récemment exécuté.|  
|**login_time**|**datetime**|Date et heure de la connexion de l'Agent de distribution en cours d'exécution ou récemment exécutée.|  
|**allow_subscription_copy**|**bit**|Indique si la possibilité de copier la base de données d'abonnement est autorisée.|  
|**attach_state**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**attach_version**|**Binary(16**|Identificateur unique représentant la version d'un abonnement attaché.|  
|**last_sync_status**|**int**|État de la dernière exécution de l'Agent de distribution en cours d'exécution ou récemment exécuté. L'état peut prendre l'une des valeurs suivantes :<br /><br /> **1** = démarré.<br /><br /> **2** = réussite.<br /><br /> **3** = en cours.<br /><br /> **4** = inactif.<br /><br /> **5** = nouvelle tentative.<br /><br /> **6** = échec.|  
|**last_sync_summary**|**sysname**|Dernier message de l'Agent de distribution en cours d'exécution ou récemment exécuté. L'état peut prendre l'une des valeurs suivantes :<br /><br /> **Cours.**<br /><br /> **A réussi.**<br /><br /> **En cours.**<br /><br /> **Périodes.**<br /><br /> **Réessayez.**<br /><br /> **Incident.**|  
|**last_sync_time**|**datetime**|Date et heure auxquelles les colonnes *last_sync_summary* et *last_sync_status* ont été mises à jour. Les Agents de distribution anonyme ou par extraction de données exécutés en tant que travaux du service SqlServer Agent ne mettent pas à jour ces colonnes. Dans ce cas, les informations d'historique sont consignées dans la table de l'historique des travaux.|  
|**queue_server**|**sysname**|À usage interne uniquement|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)  
  
  

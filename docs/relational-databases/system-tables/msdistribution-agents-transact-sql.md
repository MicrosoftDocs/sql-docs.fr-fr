---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20b2d5f012c33ea12d4b60ed3a8d44ee475f3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091476"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSdistribution_agents** contient une ligne pour chaque agent de distribution en cours d’exécution sur le serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent de distribution.|  
|**name**|**nvarchar(100**|Nom de l'Agent de distribution.|  
|**publisher_database_id**|**int**|Identificateur de la base de données du serveur de publication.|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**subscriber_id**|**smallint**|ID de l'Abonné, utilisé uniquement par les agents reconnus. Pour les agents anonymes, cette colonne est réservée.|  
|**subscriber_db**|**sysname**|Nom de la base de données d'abonnement|  
|**subscription_type**|**int**|Type d’abonnement :<br /><br /> **0** = push.<br /><br /> **1** = pull.<br /><br /> **2** = anonyme.|  
|**local_job**|**bit**|Indique s'il existe une tâche de l'agent SQL Server sur le serveur de distribution local.|  
|**job_id**|**Binary(16**|Numéro d’identification du travail.|  
|**subscription_guid**|**Binary(16**|ID des abonnements de cet agent|  
|**profile_id**|**int**|L’ID de configuration de l' [MSagent_profiles &#40;table Transact-SQL&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**anonymous_subid**|**uniqueidentifier**|ID d'un agent anonyme.|  
|**subscriber_name**|**sysname**|Nom de l'Abonné, utilisé par des agents anonymes uniquement|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|Date et heure de création de l'Agent de distribution ou de l'Agent de fusion|  
|**queue_id**|**sysname**|Identificateur permettant de localiser la file d'attente pour les abonnements de mise à jour en attente. Pour les abonnements qui ne sont pas en attente, la valeur est NULL. Pour les publications [!INCLUDE[msCoName](../../includes/msconame-md.md)] basées sur Message Queuing, la valeur est un GUID qui identifie de manière unique la file d'attente à utiliser pour l'abonnement. Pour les publications de files d’attente basées sur SQL Server, la colonne contient la valeur **SQL**.<br /><br /> Remarque : l’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queuing est dépréciée et n’est plus prise en charge.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Indique si l’agent peut être activé à distance.<br /><br /> **0** indique que l’agent ne peut pas être activé à distance.<br /><br /> **1** indique que l’agent sera activé à distance et sur l’ordinateur distant spécifié dans la propriété *offload_server* .|  
|**offload_server**|**sysname**|Nom de réseau du serveur à utiliser pour l'activation de l'agent à distance|  
|**dts_package_name**|**sysname**|Nom du package DTS. Par exemple, pour un package nommé **DTSPub_Package**, spécifiez `@dts_package_name = N'DTSPub_Package'` .|  
|**dts_package_password**|**nvarchar (524)**|Mot de passe du package.|  
|**dts_package_location**|**int**|Emplacement du package. L’emplacement du package peut être **Distributor** ou **Subscriber**.|  
|**sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) de l'Agent de distribution ou de fusion lors de sa première exécution.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Mode de sécurité utilisé par l'Agent lors de la connexion à l'Abonné. Les valeurs possibles sont les suivantes :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification SQL Server<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.|  
|**subscriber_login**|**sysname**|Nom de connexion utilisé lors de la connexion à l'Abonné.|  
|**subscriber_password**|**nvarchar (524)**|Indique la valeur chiffrée du mot de passe qui est utilisé lors de la connexion à l'Abonné.|  
|**reset_partial_snapshot_progress**|**bit**|Indique si un instantané partiellement téléchargée sera annulée pour que la totalité du processus d'instantané puisse recommencer.|  
|**job_step_uid**|**uniqueidentifier**|ID unique de l'étape de travail de l'agent SQL Server dans laquelle l'agent est démarré.|  
|**SubscriptionStreams**|**tinyint**|Définit le nombre de connexions autorisées par l'Agent de distribution pour appliquer des traitements de modifications en parallèle à un Abonné. Une plage de valeurs allant de 1 à 64 est prise en charge.|  
|**memory_optimized**|**bit**|1 indique que l’abonné peut être utilisé pour les tables optimisées en mémoire.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  

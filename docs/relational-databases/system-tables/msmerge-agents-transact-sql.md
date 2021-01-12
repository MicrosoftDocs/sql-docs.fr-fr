---
description: MSmerge_agents (Transact-SQL)
title: MSmerge_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_agents
- MSmerge_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_agents system table
ms.assetid: 639d2ebb-2c37-4fe0-b14b-1637bc5fc221
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9380cd5c5e04293e0296c098ee7455eb803e3972
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096207"
---
# <a name="msmerge_agents-transact-sql"></a>MSmerge_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_agents** contient une ligne pour chaque agent de fusion en cours d’exécution sur l’abonné. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent de fusion|  
|**name**|**nvarchar(100**|Nom de l'Agent de fusion.|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**subscriber_id**|**smallint**|ID de l’abonné.|  
|**subscriber_db**|**sysname**|Nom de la base de données d’abonnement.|  
|**local_job**|**bit**|Indique s'il existe un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution local.|  
|**job_id**|**Binary(16**|Numéro d’identification du travail.|  
|**profile_id**|**int**|ID de configuration de la table **MSagent_profiles** .|  
|**anonymous_subid**|**uniqueidentifier**|ID d'un agent anonyme.|  
|**subscriber_name**|**sysname**|Nom de l'Abonné.|  
|**creation_date**|**datetime**|Date et heure de création de l'Agent de distribution ou de fusion.|  
|**offload_enabled**|**bit**|Indique si l'Agent peut être activé à distance.<br /><br /> **0** indique que l’agent ne peut pas être activé à distance.<br /><br /> **1** indique que l’agent sera activé à distance et sur l’ordinateur distant spécifié dans la propriété offload_server.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation de l'Agent distant.|  
|**sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) de l'Agent de distribution ou de fusion lors de sa première exécution.|  
|**subscriber_security_mode**|**smallint**|Mode de sécurité utilisé par l'Agent lors de la connexion à l'Abonné. Les valeurs possibles sont les suivantes :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.|  
|**subscriber_login**|**sysname**|Nom de connexion utilisé lors de la connexion à l'Abonné.|  
|**subscriber_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisé lors de la connexion à l'Abonné.|  
|**publisher_security_mode**|**smallint**|Mode de sécurité utilisé par l'agent lors de la connexion au serveur de publication et pouvant prendre la valeur suivante :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.|  
|**publisher_login**|**sysname**|Connexion utilisée lors de la connexion au serveur de publication.|  
|**publisher_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisée lors de la connexion au serveur de publication.|  
|**job_step_uid**|**uniqueidentifier**|ID unique de l'étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle l'Agent est démarré.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

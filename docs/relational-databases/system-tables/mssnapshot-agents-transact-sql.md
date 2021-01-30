---
description: MSsnapshot_agents (Transact-SQL)
title: MSsnapshot_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsnapshot_agents
- MSsnapshot_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshot_agents system table
ms.assetid: aeae0a2e-4c21-4c45-be65-1e426fa52bdd
author: cawrites
ms.author: chadam
ms.openlocfilehash: ba527028dc1b217022b1f0364b4cb2439d30ef61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199076"
---
# <a name="mssnapshot_agents-transact-sql"></a>MSsnapshot_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSsnapshot_agents** contient une ligne pour chaque agent d’instantané associée au serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent d'instantané.|  
|**name**|**nvarchar(100**|Nom de l'Agent d'instantané.|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnel.<br /><br /> **1** = instantané.<br /><br /> **2** = fusion.|  
|**local_job**|**bit**|Indique s'il existe un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution local.|  
|**job_id**|**Binary(16**|Numéro d’identification du travail.|  
|**profile_id**|**int**|L’ID de configuration de l' [MSagent_profiles &#40;table Transact-SQL&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**dynamic_filter_login**|**sysname**|Valeur utilisée pour évaluer le [SUSER_SNAME &#40;fonction Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) dans les filtres paramétrables qui définissent une partition. Cette colonne est utilisée pour un instantané partitionné.|  
|**dynamic_filter_hostname**|**sysname**|Valeur utilisée pour évaluer le [HOST_NAME &#40;fonction Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) dans les filtres paramétrables qui définissent une partition. Cette colonne est utilisée pour un instantané partitionné.|  
|**publisher_security_mode**|**smallint**|Mode de sécurité utilisé par l'agent lors de la connexion au serveur de publication et pouvant prendre la valeur suivante :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.|  
|**publisher_login**|**sysname**|Connexion utilisée lors de la connexion au serveur de publication.|  
|**publisher_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisée lors de la connexion au serveur de publication.|  
|**job_step_uid**|**uniqueidentifier**|ID unique de l'étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle l'Agent est démarré.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

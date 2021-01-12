---
description: MSlogreader_agents (Transact-SQL)
title: MSlogreader_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSlogreader_agents_TSQL
- MSlogreader_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSlogreader_agents system table
ms.assetid: 8baa3c5a-cb40-42d0-b966-00e6d55368e8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58b93b15fc6f8b99d507004b33bbb371ac211663
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096216"
---
# <a name="mslogreader_agents-transact-sql"></a>MSlogreader_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSlogreader_agents** contient une ligne pour chaque agent de lecture du journal qui s’exécute sur le serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent de lecture du journal.|  
|**name**|**nvarchar(100**|Nom de l'Agent de lecture du journal|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**local_job**|**bit**|Indique s'il existe un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur de distribution local.|  
|**job_id**|**Binary(16**|Numéro d’identification du travail.|  
|**profile_id**|**int**|ID de configuration de la table [MSagent_profiles](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**publisher_security_mode**|**smallint**|Mode de sécurité utilisé par l'agent lors de la connexion au serveur de publication et pouvant prendre la valeur suivante :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.|  
|**publisher_login**|**sysname**|Connexion utilisée lors de la connexion au serveur de publication.|  
|**publisher_password**|**nvarchar (524)**|Valeur chiffrée du mot de passe utilisée lors de la connexion au serveur de publication.|  
|**job_step_uid**|**uniqueidentifier**|ID unique de l'étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle l'Agent est démarré.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

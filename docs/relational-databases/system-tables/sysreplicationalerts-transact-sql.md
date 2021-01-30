---
description: sysreplicationalerts (Transact-SQL)
title: sysreplicationalerts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysreplicationalerts_TSQL
- sysreplicationalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysreplicationalerts system table
ms.assetid: 6ed15828-8cca-4cf0-b2ff-1ecd0d8db11a
author: cawrites
ms.author: chadam
ms.openlocfilehash: f9463574cb2cda73842b2e44cda3dfa811e43e2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180377"
---
# <a name="sysreplicationalerts-transact-sql"></a>sysreplicationalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur les conditions déclenchant une alerte de réplication. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Identificateur de l'alerte.|  
|**statut**|**int**|Valeur définie par l'utilisateur :<br /><br /> **0** = n’est pas pris en service.<br /><br /> **1** = Serviced.|  
|**agent_type**|**int**|Type d'agent :<br /><br /> **1** = agent d’instantané.<br /><br /> **2** = agent de lecture du journal.<br /><br /> **3** = agent de distribution.<br /><br /> **4** = agent de fusion.|  
|**agent_id**|**int**|L’ID d’agent des tables **MSsnapshot_agents**, **MSlogreader_agents**, **MSdistribution_agents** ou **MSmerge_agents**.|  
|**error_id**|**int**|ID de l’erreur stockée dans **MSrepl_errors**.|  
|**alert_error_code**|**int**|ID de message de l'alerte déclenchée lors de l'écriture de cet enregistrement dans le journal.|  
|**time**|**datetime**|Date et heure d'insertion de l'enregistrement.|  
|**publisher**|**sysname**|Nom du serveur de publication associé à l'Agent qui a déclenché l'alerte.|  
|**publisher_db**|**sysname**|Base de données du serveur de publication associée à l'Agent qui a déclenché l'alerte.|  
|**édition**|**sysname**|Publication associée à l'Agent qui a déclenché l'alerte.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = instantané.<br /><br /> **1** = transactionnelle.<br /><br /> **2** = fusion.|  
|**côté**|**sysname**|Nom de l'Abonné associé à l'Agent qui a déclenché l'alerte.|  
|**subscriber_db**|**sysname**|Nom de la base de données de l'Abonné associée à l'Agent qui a déclenché l'alerte.|  
|**article**|**sysname**|Nom de l'article associé à l'Agent qui a déclenché l'alerte.|  
|**destination_object**|**sysname**|Nom de la table d'abonnement associée à l'alerte.|  
|**source_object**|**sysname**|Nom de la table publiée associée à l'alerte.|  
|**alert_error_text**|**ntext**|Texte de l'alerte.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

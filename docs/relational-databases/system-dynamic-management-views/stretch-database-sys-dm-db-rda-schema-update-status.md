---
title: sys.dm_db_rda_schema_update_status (Transact-SQL) | Microsoft Docs
description: Découvrez comment sys.dm_db_rda_schema_update_status contient une ligne pour chaque tâche de mise à jour de schéma de l’archive de données distante de chaque table avec extension Stretch dans la base de données.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.dm_db_rda_schema_update_status
- sys.dm_db_rda_schema_update_status_TSQL
- dm_db_rda_schema_update_status
- dm_db_rda_schema_update_status_TSQL
helpviewer_keywords:
- sys.dm_db_rda_schema_update_status dynamic management view
ms.assetid: 364e3caa-a7c6-4be5-a029-0b19da34de3e
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 955f06ba0a7ad991ffe979fc927dfabe6e5a8dd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202421"
---
# <a name="stretch-database---sysdm_db_rda_schema_update_status"></a>Stretch Database-sys.dm_db_rda_schema_update_status
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contient une ligne pour chaque tâche de mise à jour de schéma de l’archive de données distante de chaque table avec extension Stretch dans la base de données active. Les tâches sont identifiées par leur ID de tâche.  
  
 **dm_db_rda_schema_update_status** est limitée au contexte de la base de données actuelle. Vérifiez que vous êtes dans le contexte de la base de données de la table compatible Stretch pour laquelle vous souhaitez afficher l’état de la mise à jour du schéma.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|ID de la table compatible Stretch locale dont le schéma d’archive des données distantes est en cours de mise à jour.|  
|**database_id**|**int**|ID de la base de données qui contient la table locale avec Stretch.|  
|**task_id**|**bigint**|ID de la tâche de mise à jour du schéma d’archive de données distante.|  
|**task_type**|**int**|Type de la tâche de mise à jour du schéma d’archive de données distante.|  
|**task_type_desc**|**nvarchar**|Description du type de la tâche de mise à jour du schéma d’archive de données distante.|  
|**task_state**|**int**|État de la tâche de mise à jour du schéma d’archive de données distante.|  
|**task_state_des**|**nvarchar**|Description de l’état de la tâche de mise à jour du schéma d’archive de données distante.|  
|**start_time_utc**|**datetime**|Heure UTC à laquelle la mise à jour du schéma d’archive de données distante a démarré.|  
|**end_time_utc**|**datetime**|Heure UTC à laquelle la mise à jour du schéma d’archive de données distante s’est terminée.|  
|**error_number**|**int**|En cas d’échec de la mise à jour du schéma d’archive de données distante, numéro d’erreur de l’erreur qui s’est produite ; Sinon, null.|  
|**error_severity**|**int**|En cas d’échec de la mise à jour du schéma d’archive de données distante, gravité de l’erreur qui s’est produite ; Sinon, null.|  
|**error_state**|**int**|En cas d’échec de la mise à jour du schéma d’archive de données distante, état de l’erreur qui s’est produite ; Sinon, null. Le error_state indique la condition ou l’emplacement où l’erreur s’est produite.|  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  

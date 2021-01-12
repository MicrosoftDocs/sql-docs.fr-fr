---
description: sys.dm_broker_activated_tasks (Transact-SQL)
title: sys.dm_broker_activated_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_broker_activated_tasks
- sys.dm_broker_activated_tasks_TSQL
- dm_broker_activated_tasks
- dm_broker_activated_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_activated_tasks dynamic management view
ms.assetid: 17e6f87f-8f56-489d-9aed-216afc8ef310
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f3c48ab890509e93ea2ad193892f1a6dd41e7c0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095268"
---
# <a name="sysdm_broker_activated_tasks-transact-sql"></a>sys.dm_broker_activated_tasks (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque procédure stockée activée par Service Broker.  
 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**spid**|**int**|ID de la session de la procédure stockée activée. Accepte la valeur NULL.|  
|**database_id**|**smallint**|ID de la base de données dans laquelle la file d'attente est définie. Accepte la valeur NULL.|  
|**queue_id**|**int**|ID de l'objet de la file d'attente pour lequel la procédure stockée a été activée. Accepte la valeur NULL.|  
|**procedure_name**|**nvarchar (650)**|Nom de la procédure stockée activée. Accepte la valeur NULL.|  
|**execute_as**|**int**|ID de l'utilisateur sous le nom duquel la procédure stockée s'exécute. Accepte la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures pour sys.dm_broker_activated_tasks](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-activated-tasks-1.gif "Jointures pour sys.dm_broker_activated_tasks")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|dm_broker_activated_tasks.spid|dm_exec_sessions.session_id|Un-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  


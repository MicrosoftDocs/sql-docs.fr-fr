---
description: dbo.sysjobschedules (Transact-SQL)
title: dbo.sysJobSchedules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysjobschedules
- dbo.sysjobschedules
- dbo.sysjobschedules_TSQL
- sysjobschedules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobschedules system table
ms.assetid: ccdafec7-2a9b-4356-bffb-1caa3a12db59
author: cawrites
ms.author: chadam
ms.openlocfilehash: f82fae854a00e73ec7d8d45304ee486e0df33373
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159997"
---
# <a name="dbosysjobschedules-transact-sql"></a>dbo.sysjobschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient les informations de planification pour les travaux qui doivent être exécutés par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette table est stockée dans la base de données **msdb** .  
  
> **Remarque :** La table **sysjobschedules** est actualisée toutes les 20 minutes, ce qui peut affecter les valeurs retournées par la procédure stockée **sp_help_jobschedule** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|ID de la planification.|  
|**job_id**|**uniqueidentifier**|ID du travail.|  
|**next_run_date**|**int**|Date à laquelle est planifiée la prochaine exécution du travail. La date se présente sous la forme AAAAMMJJ.|  
|**next_run_time**|**int**|Heure à laquelle est planifiée la prochaine exécution du travail. L'heure se présente sous la forme HHMMSS et est exprimée sur 24 heures.|  
  
## <a name="see-also"></a>Voir aussi  
 [ Planifications dedbo.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  

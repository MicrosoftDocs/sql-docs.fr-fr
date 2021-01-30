---
description: dbo.cdc_jobs (Transact-SQL)
title: dbo.cdc_jobs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc_jobs
- cdc_jobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.cdc_jobs
ms.assetid: 85e2d580-1c54-4b81-b7e6-2e12997199fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9981deacd56a3cdea220daada3cd1cdff0b998e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158378"
---
# <a name="dbocdc_jobs-transact-sql"></a>dbo.cdc_jobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke les paramètres de configuration de capture de données modifiées pour les travaux de capture et de nettoyage. Cette table est stockée dans la base de données **msdb**.  
  
 
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données dans laquelle le travail est exécuté.|  
|**job_type**|**nvarchar(20**|Type de travail, capture ou nettoyage.|  
|**job_id**|**uniqueidentifier**|ID unique associé au travail.|  
|**maxtrans**|**int**|Nombre maximal de transactions à traiter dans chaque cycle d’analyse.<br /><br /> **maxtrans** est valide uniquement pour les travaux de capture.|  
|**maxscans**|**int**|Nombre maximal de cycles d’analyse à exécuter afin d’extraire toutes les lignes du journal.<br /><br /> **maxscans** est valide uniquement pour les travaux de capture.|  
|**propositions**|**bit**|Indicateur qui précise si le travail de capture doit être exécuté en continu (1) ou ponctuellement (0). Pour plus d’informations, consultez [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).<br /><br /> **Continuous** est valide uniquement pour les travaux de capture.|  
|**PollingInterval**|**bigint**|Nombre de secondes entre les cycles d'analyse du journal.<br /><br /> **PollingInterval** est valide uniquement pour les travaux de capture.|  
|**fixation**|**bigint**|Nombre de minutes pendant lesquelles les lignes modifiées doivent être conservées dans les tables de modifications.<br /><br /> la **rétention** est valide uniquement pour les travaux de nettoyage.|  
|**threshold**|**bigint**|Nombre maximal d'entrées qui peuvent être supprimées à l'aide d'une instruction unique lors du nettoyage.|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)   
 [sys.sp_cdc_change_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md)   
 [sys.sp_cdc_help_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)   
 [sys.sp_cdc_drop_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md)  
  
  

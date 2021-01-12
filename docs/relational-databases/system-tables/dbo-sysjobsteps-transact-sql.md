---
description: dbo.sysjobsteps (Transact-SQL)
title: dbo.sysJobSteps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobsteps
- dbo.sysjobsteps_TSQL
- sysjobsteps
- sysjobsteps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobsteps system table
ms.assetid: 978b8205-535b-461c-91f3-af9b08eca467
author: cawrites
ms.author: chadam
ms.openlocfilehash: a3f59ef0d9a3e2a38a7834ed4395319ec935fc59
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102136"
---
# <a name="dbosysjobsteps-transact-sql"></a>dbo.sysjobsteps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur chaque étape d'un travail devant être exécuté par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID du travail.|  
|**step_id**|**int**|ID de l'étape dans le travail|  
|**step_name**|**sysname**|Nom de l'étape du travail|  
|**sous-système**|**nvarchar(40)**|Nom du sous-système utilisé par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter l'étape du travail|  
|**command**|**nvarchar(max)**|Commande à exécuter par le **sous-système**.|  
|**flags**|**int**|Réservé.|  
|**additional_parameters**|**ntext**|Réservé.|  
|**cmdexec_success_code**|**int**|Valeur de niveau erreur retournée par les étapes du sous-système **CmdExec** pour indiquer la réussite.|  
|**on_success_action**|**tinyint**|Action à exécuter lors du succès d'une étape<br /><br /> **1** = (valeur par défaut) quitter avec succès<br /><br /> **2** = quitter avec échec<br /><br /> **3** = passer à l’étape suivante<br /><br /> **4** = passer à l’étape _on_success_step_id_|
|**on_success_step_id**|**int**|ID de la prochaine étape à exécuter après le succès d'une étape|  
|**on_fail_action**|**tinyint**|Action à exécuter lors de l'échec d'une étape<br /><br /> **1** = quitter avec succès<br /><br /> **2** = (valeur par défaut) quitter avec échec<br /><br /> **3** = passer à l’étape suivante<br /><br /> **4** = passer à l’étape _on_fail_step_id_|
|**on_fail_step_id**|**int**|ID de l'étape à exécuter après l'échec d'une étape|  
|**server**|**sysname**|Réservé.|  
|**database_name**|**sysname**|Nom de la base de données dans laquelle la **commande** est exécutée si le **sous-système** est TSQL.|  
|**database_user_name**|**sysname**|Nom de l'utilisateur de la base de données dont le compte sera utilisé lors de l'exécution de l'étape|  
|**retry_attempts**|**int**|Nombre de tentatives de reprise en cas d'échec de l'étape|  
|**retry_interval**|**int**|Délai d'attente entre chaque tentative de reprise|  
|**os_run_priority**|**int**|Réservé.|  
|**output_file_name**|**nvarchar(200)**|Nom du fichier dans lequel la sortie de l’étape est enregistrée lorsque le **sous-système** est TSQL, PowerShell ou **CmdExec**_._|  
|**last_run_outcome**|**int**|Issue de l'exécution précédente de l'étape du travail<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **2** = nouvelle tentative<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**last_run_duration**|**int**|Durée (hhmmss) de l'étape lors de sa dernière exécution.|  
|**last_run_retries**|**int**|Nombre de tentatives de reprises lors de la dernière exécution de l'étape du travail|  
|**last_run_date**|**int**|Date (aaaammjj) de début de la dernière exécution de l'étape.|  
|**last_run_time**|**int**|Heure (hhmmss) de début de la dernière exécution de l'étape.|  
|**proxy_id**|**int**|Proxy pour les étapes du travail.|  
|**step_uid**|**uniqueidentifier**|Identificateur de l’étape de travail.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  

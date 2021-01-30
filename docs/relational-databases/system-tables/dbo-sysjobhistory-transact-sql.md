---
description: dbo.sysjobhistory (Transact-SQL)
title: dbo.sysjobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobhistory_TSQL
- dbo.sysjobhistory
- sysjobhistory
- sysjobhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobhistory system table
ms.assetid: 1b1fcdbb-2af2-45e6-bf3f-e8279432ce13
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9cbb5238e8cb41d892f336413cf7773281476f35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186315"
---
# <a name="dbosysjobhistory-transact-sql"></a>dbo.sysjobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contient des informations sur l'exécution des travaux planifiés par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
> [!NOTE]
> Dans la plupart des cas, les données sont mises à jour uniquement une fois l’étape de travail terminée et la table ne contient généralement aucun enregistrement pour les étapes de travail qui sont actuellement en cours, mais dans certains cas, les processus sous- *jacents fournissent des* informations sur les étapes de travail en cours.

Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Identificateur unique de la ligne.|  
|**job_id**|**uniqueidentifier**|ID de travail.|  
|**step_id**|**int**|ID de l'étape dans le travail|  
|**step_name**|**sysname**|Nom de l'étape|  
|**sql_message_id**|**int**|Identificateur de tout message d'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourné en cas d'échec du travail.|  
|**sql_severity**|**int**|Gravité des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**message**|**nvarchar(4000)**|Texte éventuel d'une erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**run_status**|**int**|État de l'exécution du travail :<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **2** = nouvelle tentative<br /><br /> **3** = annulé<br /><br />**4** = en cours|  
|**run_date**|**int**|Date du début d'exécution du travail ou de l'étape. Pour l'historique des travaux en cours, date et heure de l'écriture de l'historique.|  
|**run_time**|**int**|Heure à laquelle le travail ou l’étape a démarré au format **HHMMSS** .|  
|**run_duration**|**int**|Temps écoulé dans l’exécution du travail ou de l’étape au format **HHMMSS** .|  
|**operator_id_emailed**|**int**|Identificateur de l'opérateur averti de la fin du travail.|  
|**operator_id_netsent**|**int**|Identificateur de l'opérateur averti par un message de la fin du travail.|  
|**operator_id_paged**|**int**|Identificateur de l'opérateur averti par radiomessagerie de la fin du travail.|  
|**retries_attempted**|**int**|Nombre de nouvelles tentatives pour le travail ou l'étape.|  
|**server**|**sysname**|Nom du serveur sur lequel le travail a été exécuté.|  
  
  ## <a name="example"></a>Exemple
 La [!INCLUDE[tsql](../../includes/tsql-md.md)] requête suivante convertit les colonnes **run_time** et **run_duration** dans un format plus convivial.  Exécutez le script dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .
 
 ```sql
 SET NOCOUNT ON;
 
 SELECT sj.name,
        sh.run_date,
        sh.step_name,
        STUFF(STUFF(RIGHT(REPLICATE('0', 6) +  CAST(sh.run_time as varchar(6)), 6), 3, 0, ':'), 6, 0, ':') 'run_time',
        STUFF(STUFF(STUFF(RIGHT(REPLICATE('0', 8) + CAST(sh.run_duration as varchar(8)), 8), 3, 0, ':'), 6, 0, ':'), 9, 0, ':') 'run_duration (DD:HH:MM:SS)  '
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobhistory sh
ON sj.job_id = sh.job_id
```

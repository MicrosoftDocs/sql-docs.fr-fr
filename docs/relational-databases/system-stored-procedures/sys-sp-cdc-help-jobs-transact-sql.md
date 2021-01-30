---
description: sys.sp_cdc_help_jobs (Transact-SQL)
title: sys.sp_cdc_help_jobs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_help_jobs
- sys.sp_cdc_help_jobs_TSQL
- sp_cdc_help_jobs_TSQL
- sys.sp_cdc_help_jobs
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_help_jobs
ms.assetid: 9399b4bc-8293-408f-b3cb-f904e0657fb5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c1ee57c6e03b41d9cb8a6e6719948a49f61ca58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191944"
---
# <a name="syssp_cdc_help_jobs-transact-sql"></a>sys.sp_cdc_help_jobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Signale des informations sur tous les travaux de capture ou de nettoyage de capture de données modifiées dans la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_help_jobs  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID du travail.|  
|**job_type**|**nvarchar(20**|Type de tâche.|  
|**maxtrans**|**int**|Nombre maximal de transactions à traiter dans chaque cycle d’analyse.<br /><br /> **maxtrans** est valide uniquement pour les travaux de capture.|  
|**maxscans**|**int**|Nombre maximal de cycles d’analyse à exécuter afin d’extraire toutes les lignes du journal.<br /><br /> **maxscans** est valide uniquement pour les travaux de capture.|  
|**propositions**|**bit**|Indicateur qui précise si le travail de capture doit être exécuté en continu (1) ou ponctuellement (0). Pour plus d’informations, consultez [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).<br /><br /> **Continuous** est valide uniquement pour les travaux de capture.|  
|**PollingInterval**|**bigint**|Nombre de secondes entre les cycles d'analyse du journal.<br /><br /> **PollingInterval** est valide uniquement pour les travaux de capture.|  
|**fixation**|**bigint**|Nombre de minutes pendant lesquelles les lignes modifiées doivent être conservées dans les tables de modifications.<br /><br /> la **rétention** est valide uniquement pour les travaux de nettoyage.|  
|**threshold**|**bigint**|Nombre maximal d'entrées qui peuvent être supprimées à l'aide d'une instruction unique lors du nettoyage.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les travaux de capture et de nettoyage définis pour la base de données `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_help_jobs;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  

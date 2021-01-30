---
description: sys.sp_cdc_add_job (Transact-SQL)
title: sys.sp_cdc_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_add_job_TSQL
- sys.sp_cdc_add_job
- sys.sp_cdc_add_job_TSQL
- sp_cdc_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_add_job
ms.assetid: c4458738-ed25-40a6-8294-a26ca5a05bd9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c48413f1f588ea4a70c4f8a1190e819758c76b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186690"
---
# <a name="syssp_cdc_add_job-transact-sql"></a>sys.sp_cdc_add_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un travail de nettoyage ou de capture de données modifiées dans la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_add_job [ @job_type = ] 'job_type'  
    [ , [ @start_job = ] start_job ]   
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ , [ @threshold ] = 'delete_threshold' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_type = ] 'job\_type'` Type de travail à ajouter. *job_type* est de type **nvarchar (20)** et ne peut pas être null. Les entrées valides sont **'capture'** et **'cleanup'**.  
  
`[ @start_job = ] start_job` Indicateur qui spécifie si le travail doit être démarré immédiatement après son ajout. *START_JOB* est de **bits** avec 1 comme valeur par défaut.  
  
`[ @maxtrans ] = max_trans` Nombre maximal de transactions à traiter dans chaque cycle d’analyse. *max_trans* est de **type int** avec 500 comme valeur par défaut. Si elle est spécifiée, la valeur doit être un entier positif.  
  
 *max_trans* est valide uniquement pour les travaux de capture.  
  
`[ @maxscans ] = max\_scans_` Nombre maximal de cycles d’analyse à exécuter afin d’extraire toutes les lignes du journal. *max_scans* est de **type int** avec 10 comme valeur par défaut.  
  
 *max_scan* est valide uniquement pour les travaux de capture.  
  
`[ @continuous ] = continuous_` Indique si le travail de capture doit s’exécuter en continu (1) ou s’exécuter une seule fois (0). *Continuous* est de **bits** avec 1 comme valeur par défaut.  
  
 Lorsque *Continuous* = 1, la tâche de [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) analyse le journal et traite les transactions jusqu’à (*max_trans* \* *max_scans*). Il attend ensuite le nombre de secondes spécifié dans *polling_interval* avant de commencer la prochaine analyse du journal.  
  
 Lorsque *Continuous* = 0, le travail **sp_cdc_scan** s’exécute jusqu’à *max_scans* analyses du journal, en traitant jusqu’à *max_trans* transaction au cours de chaque analyse, puis se ferme.  
  
 *Continuous* est valide uniquement pour les travaux de capture.  
  
`[ @pollinginterval ] = polling\_interval_` Nombre de secondes entre les cycles d’analyse du journal. *polling_interval* est de type **bigint** avec 5 comme valeur par défaut.  
  
 *polling_interval* est valide uniquement pour les travaux de capture quand *Continuous* a la valeur 1. S’il est spécifié, la valeur doit être supérieure ou égale à 0 et inférieure à 24 heures (max : 86399 secondes). Si une valeur de 0 est spécifiée, il n'y a aucune attente entre les analyses de journal.  
  
`[ @retention ] = retention_` Nombre de minutes pendant lesquelles les lignes de données modifiées doivent être conservées dans les tables de modifications. la *rétention* est de type **bigint** et sa valeur par défaut est 4320 (72 heures). La valeur maximale est égale à 52494800 (100 ans). Si elle est spécifiée, la valeur doit être un entier positif.  
  
 la *rétention* est valide uniquement pour les travaux de nettoyage.  
  
`[ @threshold = ] 'delete\_threshold'` Nombre maximal d’entrées qui peuvent être supprimées à l’aide d’une instruction unique lors du nettoyage. *delete_threshold* est de type **bigint** avec 5000 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Un travail de nettoyage est créé en utilisant les valeurs par défaut lorsque la première table dans la base de données est activée pour la capture des données modifiées. Un travail de capture est créé en utilisant les valeurs par défaut lorsque la première table dans la base de données est activée pour la capture des données modifiées et qu'aucune publication transactionnelle n'existe pour la base de données. Lorsqu'une publication transactionnelle existe, le lecteur de journal transactionnel est utilisé pour conduire le mécanisme de capture et un travail de capture séparé n'est ni requis ni autorisé.  
  
 Dans la mesure où les travaux de capture et de nettoyage sont créés par défaut, cette procédure stockée est nécessaire uniquement lorsqu'un travail a été supprimé explicitement et doit être recréé.  
  
 Le nom du travail est **CDC.** _\<database\_name\>_ **\_ nettoyage** ou **CDC.** _\<database\_name\>_ **\_ capture**, où *<database_name>* est le nom de la base de données actuelle. Si un travail portant le même nom existe déjà, le nom est ajouté avec un point (**.**) suivi d’un identificateur unique, par exemple : **CDC.AdventureWorks_capture. A1ACBDED-13FC-428C-8302-10100EF74F52**.  
  
 Pour afficher la configuration actuelle d’un travail de nettoyage ou de capture, utilisez [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md). Pour modifier la configuration d’un travail, utilisez [sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-capture-job"></a>R. Création d'un travail de capture  
 L'exemple suivant crée un travail de capture. Cet exemple suppose que le travail de nettoyage existant a été supprimé explicitement et doit être recréé. Le travail est créé en utilisant les valeurs par défaut.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job @job_type = N'capture';  
GO  
```  
  
### <a name="b-creating-a-cleanup-job"></a>B. Création d'un travail de nettoyage  
 L'exemple suivant crée un travail de nettoyage dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Le paramètre `@start_job` a la valeur 0 et `@retention` a pour valeur 5760 minutes (96 heures). Cet exemple suppose que le travail de nettoyage existant a été supprimé explicitement et doit être recréé.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job  
     @job_type = N'cleanup'  
    ,@start_job = 0  
    ,@retention = 5760;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  

---
description: sp_adddynamicsnapshot_job (Transact-SQL)
title: sp_adddynamicsnapshot_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddynamicsnapshot_job
- sp_adddynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_adddynamicsnapshot_job
ms.assetid: ef50ccf6-e360-4e4b-91b9-6706b8fabefa
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 9798613901be96552813ebe6f419f2e2c24229fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182795"
---
# <a name="sp_adddynamicsnapshot_job-transact-sql"></a>sp_adddynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un travail de l'Agent qui crée l'instantané de données filtrées pour une publication avec des filtres de lignes paramétrables. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication. Un administrateur utilise cette procédure stockée pour créer manuellement des travaux d'instantané de données filtrées pour des Abonnés.  
  
> [!NOTE]  
>  Pour créer un travail d'instantané de données filtrées, un travail d'instantané standard doit déjà exister pour la publication.  
  
 Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]   
    [ , [ @host_name = ] 'host_name' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' OUTPUT ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' OUTPUT ]   
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication à laquelle le travail d’instantané de données filtrées est ajouté. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @suser_sname = ] 'suser_sname'` Valeur utilisée lors de la création d’un instantané de données filtrées pour un abonnement filtré par la valeur de la fonction [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) sur l’abonné. *SUSER_SNAME* est de **type sysname**, sans valeur par défaut. *SUSER_SNAME* doit avoir la valeur null si cette fonction n’est pas utilisée pour filtrer dynamiquement la publication.  
  
`[ @host_name = ] 'host_name'` Valeur utilisée lors de la création d’un instantané de données filtrées pour un abonnement filtré par la valeur de la fonction [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) sur l’abonné. *HOST_NAME* est de **type sysname**, sans valeur par défaut. *HOST_NAME* doit avoir la valeur null si cette fonction n’est pas utilisée pour filtrer dynamiquement la publication.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Nom du travail d’instantané de données filtrées créé. *dynamic_snapshot_jobname* est de **type sysname**, avec NULL comme valeur par défaut. il s’agit d’un paramètre de sortie facultatif. S’il est spécifié, *dynamic_snapshot_jobname* doit être résolu en un travail unique sur le serveur de distribution. S'il n'est pas spécifié, un nom de travail est automatiquement créé et renvoyé dans l'ensemble de résultats, où le nom est créé comme suit :  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
> [!NOTE]  
>  Lors de la création du nom du travail d'instantané dynamique, vous pouvez tronquer le nom du travail d'instantané standard.  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Identificateur du travail d’instantané de données filtrées créé. *dynamic_snapshot_jobid* est de type **uniqueidentifier**, avec NULL comme valeur par défaut. il s’agit d’un paramètre de sortie facultatif.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification du travail d’instantané de données filtrées. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2**|À la demande|  
|**4** (par défaut)|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
  
`[ @frequency_interval = ] frequency_interval` Période (exprimée en jours) pendant laquelle le travail d’instantané de données filtrées est exécuté. *frequency_interval* est de **type int**, avec 1 comme valeur par défaut et dépend de la valeur de *frequency_type*.  
  
|Valeur de *frequency_type*|Effet sur *frequency_interval*|  
|--------------------------------|-------------------------------------|  
|**1**|*frequency_interval* n’est pas utilisé.|  
|**4** (par défaut)|Tous les *frequency_interval* jours, avec une valeur par défaut quotidienne.|  
|**8**|*frequency_interval* est une ou plusieurs des valeurs suivantes (combinées avec un opérateur logique [&#124; &#40;ou au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md) ) :<br /><br /> **1** = dimanche &#124; **2** = lundi &#124; **4** = mardi &#124; **8** = mercredi &#124; **16** = jeudi &#124; **32** = vendredi &#124; **64** = samedi|  
|**16**|Le *frequency_interval* jour du mois.|  
|**32**|*frequency_interval* est l’un des éléments suivants :<br /><br /> **1** = dimanche &#124; **2** = lundi &#124; **3** = mardi &#124; **4** = mercredi &#124; **5** = jeudi &#124; **6** = vendredi &#124; **7** = samedi &#124; **8** = jour &#124; **9** = jour de la semaine &#124; **10** = jour de week-end|  
|**64**|*frequency_interval* n’est pas utilisé.|  
|**128**|*frequency_interval* n’est pas utilisé.|  
  
`[ @frequency_subday = ] frequency_subday` Spécifie les unités pour *frequency_subday_interval*. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Nombre de *frequency_subday* périodes qui se produisent entre chaque exécution du travail. *frequency_subday_interval* est de **type int**, avec 5 comme valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Est l’occurrence du travail d’instantané de données filtrées dans chaque mois. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1** (par défaut)|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le travail d’instantané de données filtrées est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le travail d’instantané de données filtrées cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le travail d’instantané de données filtrées est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le travail d’instantané de données filtrées cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifie le travail d’instantané de données filtrées dans la table système [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) .|  
|**dynamic_snapshot_jobname**|**sysname**|Nom du travail d'instantané de données filtrées.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifie de façon unique le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] travail de l’agent sur le serveur de distribution.|  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddynamicsnapshot_job** est utilisé dans la réplication de fusion pour les publications qui utilisent un filtre paramétré.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-adddynamicsnapshot-jo_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_adddynamicsnapshot_job**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un instantané pour une publication de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)   
 [sp_helpdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md)  
  
  

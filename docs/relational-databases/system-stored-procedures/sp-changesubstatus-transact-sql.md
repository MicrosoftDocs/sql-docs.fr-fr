---
description: sp_changesubstatus (Transact-SQL)
title: sp_changesubstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubstatus
- sp_changesubstatus_TSQL
helpviewer_keywords:
- sp_changesubstatus
ms.assetid: 9370e47a-d128-4f15-9224-1c3642770c39
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89de8f8a4c5f1abb3a07551bcca6eac53e0b2e2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158964"
---
# <a name="sp_changesubstatus-transact-sql"></a>sp_changesubstatus (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie l'état d'un abonné existant. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubstatus [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
        , [ @status = ] 'status'  
    [ , [ @previous_status = ] 'previous_status' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @distribution_jobid = ] distribution_jobid ]  
    [ , [ @from_auto_sync = ] from_auto_sync ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @offloadagent= ] remote_agent_activation ]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] dts_package_location ]  
    [ , [ @skipobjectactivation = ] skipobjectactivation  
  [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** . Si la *publication* n’est pas spécifiée, toutes les publications sont affectées.  
  
`[ @article = ] 'article'` Nom de l’article. Doit être unique et propre à la publication. *article* est de **type sysname**, avec la valeur par défaut **%** . Si *l’article* n’est pas spécifié, tous les articles sont affectés.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné dont l’État doit être modifié. *Subscriber* est de **type sysname**, avec la valeur par défaut **%** . Si l' *abonné* n’est pas spécifié, l’état de tous les abonnés à l’article spécifié est modifié.  
  
`[ @status = ] 'status'` État de l’abonnement dans la table **SYSSUBSCRIPTIONS** . *Status* est de **type sysname**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**active**|L'abonné est synchronisé et reçoit des données.|  
|**inactive**|L'entrée de l'abonné existe, sans abonnement.|  
|**abonné**|L'abonné demande des données mais n'est pas encore synchronisé.|  
  
`[ @previous_status = ] 'previous_status'` État précédent de l’abonnement. *previous_status* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre vous permet de modifier les abonnements qui ont actuellement cet État, en autorisant les fonctions de groupe sur un ensemble spécifique d’abonnements (par exemple, en redéfinissant tous les abonnements actifs sur **subscribed**).  
  
`[ @destination_db = ] 'destination_db'` Nom de la base de données de destination. *destination_db* est de **type sysname**, avec la valeur par défaut **%** .  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de la tâche de distribution. *frequency_type* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_interval = ] frequency_interval` Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la tâche de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur 32 (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
|NULL (par défaut)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Indique, en minutes, la fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure du jour de la première planification de la tâche de distribution, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle la tâche de distribution cesse d’être planifiée, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle la tâche de distribution est planifiée pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle la tâche de distribution cesse d’être planifiée, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @optional_command_line = ] 'optional_command_line'` Est une invite de commandes facultative. *optional_command_line* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
`[ @distribution_jobid = ] distribution_jobid` ID du travail du Agent de distribution sur le serveur de distribution pour l’abonnement lors de la modification de l’état de l’abonnement de inactif à actif. Dans les autres cas, il n'est pas défini. Si plus d'un Agent de distribution est impliqué dans un seul appel à cette procédure stockée, le résultat n'est pas défini. *distribution_jobid* est de **type Binary (16)**, avec NULL comme valeur par défaut.  
  
`[ @from_auto_sync = ] from_auto_sync` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] remote_agent_activation`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. L’affectation d’une valeur autre que **0** à *remote_agent_activation* génère une erreur.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. L’affectation d’une valeur non NULL à *remote_agent_server_name* génère une erreur.  
  
`[ @dts_package_name = ] 'dts_package_name'` Spécifie le nom du package DTS (Data Transformation Services). *dts_package_name* est de **type sysname**, avec NULL comme valeur par défaut. Par exemple, pour un package nommé **DTSPub_Package** vous devez spécifier `@dts_package_name = N'DTSPub_Package'` .  
  
`[ @dts_package_password = ] 'dts_package_password'` Spécifie le mot de passe du package. *dts_package_password* est de **type sysname** , avec NULL comme valeur par défaut, qui spécifie que la propriété de mot de passe doit rester inchangée.  
  
> [!NOTE]  
>  Un package DTS doit avoir un mot de passe.  
  
`[ @dts_package_location = ] dts_package_location` Spécifie l’emplacement du package. *dts_package_location* est de **type int**, avec **0** comme valeur par défaut. Si la **valeur est 0**, l’emplacement du package est sur le serveur de distribution. Si **1**, l’emplacement du package est sur l’abonné. L’emplacement du package peut être **Distributor** ou **Subscriber**.  
  
`[ @skipobjectactivation = ] skipobjectactivation` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @distribution_job_name = ] 'distribution_job_name'` Nom du travail de distribution. *distribution_job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la modification des propriétés d’un article sur un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubstatus** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 **sp_changesubstatus** modifie l’état de l’abonné dans la table **SYSSUBSCRIPTIONS** avec l’état modifié. Si nécessaire, elle met à jour l’état de l’article dans la table **sysarticles** pour indiquer qu’elle est active ou inactive. Si nécessaire, il définit l’indicateur de réplication activé ou désactivé dans la table **sysobjects** pour la table répliquée.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , **db_owner** rôle de base de données fixe ou le créateur de l’abonnement peuvent exécuter **sp_changesubstatus**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

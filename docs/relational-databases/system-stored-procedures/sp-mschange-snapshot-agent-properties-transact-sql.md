---
title: sp_MSchange_snapshot_agent_properties (T-SQL)
description: Décrit la procédure stockée sp_MSchange_snapshot_agent_properties utilisée pour modifier les propriétés du Agent d’instantané utilisé pour Réplication SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_snapshot_agent_properties_TSQL
- sp_MSchange_snapshot_agent_properties
helpviewer_keywords:
- sp_MSchange_snapshot_agent_properties
ms.assetid: 7947a788-3fd7-469f-84db-b03ba89a153c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f07673e9dd7b613e34a5f56d6fa4c3629d3e1540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174497"
---
# <a name="sp_mschange_snapshot_agent_properties-transact-sql"></a>sp_MSchange_snapshot_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés d’un travail de Agent d’instantané qui s’exécute sur un serveur de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] distribution ou version ultérieure. Cette procédure stockée est utilisée pour modifier des propriétés lorsque le serveur de publication s'exécute sur une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Cette procédure stockée est exécutée au niveau du serveur de distribution sur la base de données de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_MSchange_snapshot_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @frequency_type= ] frequency_type  
        , [ @frequency_interval= ] frequency_interval  
        , [ @frequency_subday= ] frequency_subday  
        , [ @frequency_subday_interval= ] frequency_subday_interval  
        , [ @frequency_relative_interval= ] frequency_relative_interval  
        , [ @frequency_recurrence_factor= ] frequency_recurrence_factor  
        , [ @active_start_date= ] active_start_date  
        , [ @active_end_date= ] active_end_date  
        , [ @active_start_time_of_day= ] active_start_time_of_day  
        , [ @active_end_time_of_day= ] active_end_time_of_day  
        , [ @snapshot_job_name = ] 'snapshot_agent_name'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @frequency_type = ] frequency_type` Fréquence à laquelle l’Agent d’instantané est exécutée. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**10**|Mensuelle|  
|**20**|Mensuellement, en fonction de l'intervalle de fréquence|  
|**40**|Au démarrage de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
`[ @frequency_interval = ] frequency_interval` Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, sans valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Est l’unité pour *freq_subday_interval*. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, sans valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date à laquelle le Agent d’instantané s’exécute. *frequency_relative_interval* est de **type int**, sans valeur par défaut.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, sans valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent d’instantané est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, sans valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent d’instantané cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, sans valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent d’instantané est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, sans valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent d’instantané cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, sans valeur par défaut.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Nom d’un Agent d’instantané nom de tâche existant si un travail existant est utilisé. *snapshot_agent_name* est de type **nvarchar (100)**, sans valeur par défaut.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité utilisé par l’agent lors de la connexion au serveur de publication. *publisher_security_mode* est de **type int**, sans valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification et **1** spécifie l’authentification Windows. La valeur **0** doit être spécifiée pour les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication non-. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion utilisé lors de la connexion au serveur de publication. *publisher_login* est de **type sysname**, sans valeur par défaut. *publisher_login* doit être spécifié lorsque *publisher_security_mode* a la **valeur 0**. Si *publisher_login* a la valeur null et que Publisher *_ * * security_mode* a la valeur **1**, le compte Windows spécifié dans *job_login* sera utilisé lors de la connexion au serveur de publication.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de type **nvarchar (524)**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour améliorer la sécurité, nous vous recommandons de fournir les noms de connexion et les mots de passe au moment de l'exécution.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions des agents au serveur de distribution. Vous devez fournir ce paramètre lorsque vous créez un nouveau travail d'Agent d'instantané. *Cela ne peut pas être modifié pour un non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Serveur de *publication.*  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut. Vous devez fournir ce paramètre lorsque vous créez un nouveau travail d'Agent d'instantané.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour améliorer la sécurité, nous vous recommandons de fournir les noms de connexion et les mots de passe au moment de l'exécution.  
  
`[ @publisher_type = ] 'publisher_type'` Spécifie le type de serveur de publication lorsque le serveur de publication n’est pas en cours d’exécution dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* est de **type sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Spécifie un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**SOLUTION**|Spécifie un serveur de publication Oracle standard.|  
|**ORACLE GATEWAY**|Spécifie un serveur de publication Oracle Gateway.|  
  
 Pour plus d’informations sur les différences entre un serveur de publication Oracle et un serveur de publication Oracle Gateway, consultez [vue d’ensemble de la publication Oracle](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_MSchange_snapshot_agent_properties** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
 Vous devez spécifier tous les paramètres lors de l’exécution de **sp_MSchange_snapshot_agent_properties**. Exécutez [sp_helppublication_snapshot](../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md) pour retourner les propriétés actuelles du travail agent d’instantané.  
  
 Quand le serveur de publication s’exécute sur une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, vous devez utiliser [sp_changepublication_snapshot](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) pour modifier les propriétés d’un travail de agent d’instantané.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** sur le serveur de distribution peuvent exécuter **sp_MSchange_snapshot_agent_properties**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
  

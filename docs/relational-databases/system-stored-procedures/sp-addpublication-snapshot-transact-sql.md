---
description: sp_addpublication_snapshot (Transact-SQL)
title: sp_addpublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpublication_snapshot_TSQL
- sp_addpublication_snapshot
helpviewer_keywords:
- sp_addpublication_snapshot
ms.assetid: 192b6214-df6e-44a3-bdd4-9d933a981619
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7fb5fbea8a87d1bacb4a086825ecfe62b292f76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209371"
---
# <a name="sp_addpublication_snapshot-transact-sql"></a>sp_addpublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée l'Agent d'instantané pour la publication spécifiée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpublication_snapshot [ @publication= ] 'publication'  
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
    [ , [ @snapshot_job_name = ] 'snapshot_agent_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @frequency_type = ] frequency_type` Fréquence à laquelle l’Agent d’instantané est exécutée. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une seule fois.|  
|**4** (par défaut)|Tous les jours.|  
|**8**|Toutes les semaines.|  
|**16**|Mensuelle:|  
|**32**|Tous les mois, en fonction de l'intervalle de fréquence.|  
|**64**|Au démarrage de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**128**|Exécution pendant une période d'inactivité de l'ordinateur.|  
  
`[ @frequency_interval = ] frequency_interval` Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur de frequency_type|Effet sur frequency_interval|  
|------------------------------|-----------------------------------|  
|**1**|*frequency_interval* n’est pas utilisé.|  
|**4** (par défaut)|Tous les *frequency_interval* jours, avec une valeur par défaut quotidienne.|  
|**8**|*frequency_interval* est une ou plusieurs des valeurs suivantes (combinées avec un opérateur logique [&#124; (or au niveau du bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) ) :<br /><br /> **1** = dimanche &#124;<br /><br /> **2** = lundi &#124;<br /><br /> **4** = mardi &#124;<br /><br /> **8** = mercredi &#124;<br /><br /> **16** = jeudi &#124;<br /><br /> **32** = vendredi &#124;<br /><br /> **64** = samedi|  
|**16**|Le *frequency_interval* jour du mois.|  
|**32**|*frequency_interval* est l’un des éléments suivants :<br /><br /> **1** = dimanche &#124;<br /><br /> **2** = lundi &#124;<br /><br /> **3** = mardi &#124;<br /><br /> **4** = mercredi &#124;<br /><br /> **5** = jeudi &#124;<br /><br /> **6** = vendredi &#124;<br /><br /> **7** = samedi &#124;<br /><br /> **8** = jour &#124;<br /><br /> **9** = jour de la semaine &#124;<br /><br /> **10** = jour de week-end|  
|**64**|*frequency_interval* n’est pas utilisé.|  
|**128**|*frequency_interval* n’est pas utilisé.|  
  
`[ @frequency_subday = ] frequency_subday` Unité de *freq_subday_interval*. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec 5 comme valeur par défaut, c’est-à-dire toutes les 5 minutes.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date à laquelle le Agent d’instantané s’exécute. *frequency_relative_interval* est de **type int**, avec 1 comme valeur par défaut.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent d’instantané est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent d’instantané cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec 99991231 comme valeur par défaut, ce qui correspond au 31 décembre 9999.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent d’instantané est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent d’instantané cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec 235959 comme valeur par défaut, ce qui signifie 11:59:59 P.M. mesurée sur une horloge de 24 heures.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Nom d’un Agent d’instantané nom de tâche existant si un travail existant est utilisé. *snapshot_agent_name* est de type **nvarchar (100)** avec NULL comme valeur par défaut. Ce paramètre est réservé à un usage interne et ne doit pas être spécifié lors de la création d'une nouvelle publication. Si *snapshot_agent_name* est spécifié, *job_login* et *job_password* doivent avoir la valeur null.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité utilisé par l’agent lors de la connexion au serveur de publication. *publisher_security_mode* est de type **smallint**, avec 1 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification et **1** spécifie l’authentification Windows. La valeur **0** doit être spécifiée pour les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication non-. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion utilisé lors de la connexion au serveur de publication. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut. *publisher_login* doit être spécifié lorsque *publisher_security_mode* a la **valeur 0**. Si *publisher_login* a la valeur null et que *publisher_security_mode* a la valeur **1**, le compte spécifié dans *job_login* sera utilisé lors de la connexion au serveur de publication.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour améliorer la sécurité, nous vous recommandons de fournir les noms de connexion et les mots de passe au moment de l'exécution.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte sous lequel l’agent s’exécute. Sur Azure SQL Managed Instance, utilisez un compte SQL Server. *job_login* est de type **nvarchar (257)**, avec NULL comme valeur par défaut. Ce compte est toujours utilisé pour les connexions de l’agent au serveur de distribution. Vous devez fournir ce paramètre lorsque vous créez un nouveau travail d'Agent d'instantané.  
  
> [!NOTE]
>  Pour les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il doit s’agir de la même connexion que celle spécifiée dans [Sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut. Vous devez fournir ce paramètre lorsque vous créez un nouveau travail d'Agent d'instantané.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour améliorer la sécurité, nous vous recommandons de fournir les noms de connexion et les mots de passe au moment de l'exécution.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la création d’un agent d’instantané sur un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addpublication_snapshot** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-snapsh_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addpublication_snapshot**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Créer et appliquer un instantané](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_startpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

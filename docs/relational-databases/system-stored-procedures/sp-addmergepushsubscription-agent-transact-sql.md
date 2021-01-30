---
description: sp_addmergepushsubscription_agent (Transact-SQL)
title: sp_addmergepushsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepushsubscription_agent_TSQL
- sp_addmergepushsubscription_agent
helpviewer_keywords:
- sp_addmergepushsubscription_agent
ms.assetid: 808a1925-be46-4999-8d69-b3a83010ec81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dce3538b3f6160ccf08502845b960a00165d5b2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209395"
---
# <a name="sp_addmergepushsubscription_agent-transact-sql"></a>sp_addmergepushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un nouveau travail d'Agent permettant de planifier la synchronisation d'un abonnement par envoi de données (push) à une publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepushsubscription_agent [ @publication =] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password = ] 'subscriber_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
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
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Mode de sécurité à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_security_mode* est de **type int**, avec 1 comme valeur par défaut. Si la **valeur est 0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Connexion de l’abonné à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_login* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_password = ] 'subscriber_password'` Mot de passe de l’abonné pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. *subscriber_password* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est de **type sysname**, avec NULL comme valeur par défaut. Si un mot de passe d'abonné est utilisé, il est automatiquement chiffré.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_security_mode* est de **type int**, avec 1 comme valeur par défaut. Si la **valeur est 0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, avec NULL comme valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions d'Agent au serveur de distribution et pour les connexions à l'Abonné et au serveur de publication lors de l'utilisation de l'authentification intégrée de Windows.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @job_name = ] 'job_name'` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est uniquement spécifié lorsque l'abonnement est synchronisé à l'aide d'un travail existant au lieu d'un travail nouvellement créé (option par défaut). Si vous n’êtes pas membre du rôle serveur fixe **sysadmin** , vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de l’Agent de fusion. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2**|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
|NULL (par défaut)||  
  
> [!NOTE]  
>  Si vous spécifiez la valeur **64** , le agent de fusion s’exécute en mode continu. Cela correspond à la définition du paramètre **-Continuous** pour l’agent. Pour plus d’informations, voir [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Jours d’exécution de la Agent de fusion. *frequency_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**3**|Mardi|  
|**4**|Mercredi|  
|**5**|Jeudi|  
|**6**|Vendredi|  
|**7**|Samedi|  
|**8**|Jour|  
|**9**|Jours de la semaine|  
|**10**|Jours de week-end|  
|NULL (par défaut)||  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la Agent de fusion. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
|NULL (par défaut)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent de fusion est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent de fusion cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent de fusion est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent de fusion cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Spécifie si l’abonnement peut être synchronisé via le gestionnaire de synchronisation Windows. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation. Si la **valeur est true**, l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepushsubscription_agent** est utilisé dans la réplication de fusion et utilise des fonctionnalités similaires à [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepushsubscript_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergepushsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  

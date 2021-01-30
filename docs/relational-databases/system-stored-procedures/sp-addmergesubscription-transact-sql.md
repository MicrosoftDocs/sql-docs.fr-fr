---
description: sp_addmergesubscription (Transact-SQL)
title: sp_addmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergesubscription_TSQL
- sp_addmergesubscription
helpviewer_keywords:
- sp_addmergesubscription
ms.assetid: a191d817-0132-49ff-93ca-76f13e609b38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6689fed03126a8e5bc65d8d9e7e556a070ec483
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209384"
---
# <a name="sp_addmergesubscription-transact-sql"></a>sp_addmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un abonnement de fusion par émission ( push) ou extraction (pull) de données. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergesubscription [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @subscriber_type= ] 'subscriber_type' ]  
    [ , [ @subscription_priority= ] subscription_priority ]  
    [ , [ @sync_type= ] 'sync_type' ]  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @optional_command_line= ] 'optional_command_line' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @use_interactive_resolver= ] 'use_interactive_resolver' ]  
    [ , [ @merge_job_name= ] 'merge_job_name' ]  
    [ , [ @hostname = ] 'hostname'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut. La publication doit déjà exister.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscription_type = ] 'subscription_type'` Type d’abonnement. *subscription_type* est de type **nvarchar (15)**, avec Push comme valeur par défaut. Dans le cas d’une notification **Push**, un abonnement par envoi de notification est ajouté et le agent de fusion est ajouté au serveur de distribution. Dans le cas d’une **extraction**, un abonnement par extraction est ajouté sans ajouter de agent de fusion sur le serveur de distribution.  
  
> [!NOTE]  
>  Les abonnements anonymes ne doivent pas utiliser cette procédure stockée.  
  
`[ @subscriber_type = ] 'subscriber_type'` Type de l’abonné. *subscriber_type* est de type **nvarchar (15)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**local** (par défaut)|Abonné connu uniquement sur le serveur de publication.|  
|**Généralités**|Abonné connu sur tous les serveurs.|  
  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, les abonnements locaux sont des abonnements clients, et les abonnements globaux sont des abonnements serveur.  
  
`[ @subscription_priority = ] subscription_priority` Nombre indiquant la priorité de l’abonnement. *subscription_priority* est **Real**, avec NULL comme valeur par défaut. Pour les abonnements de type local et anonyme, la valeur affectée à la priorité est 0.0. Pour les abonnements de type global, la valeur affectée à la priorité doit être inférieure à 100.0.  
  
`[ @sync_type = ] 'sync_type'` Type de synchronisation de l’abonnement. *sync_type* est de type **nvarchar (15)**, avec **Automatic** comme valeur par défaut. Peut être **automatique** ou **aucun**. Si la valeur est **Automatic**, le schéma et les données initiales des tables publiées sont transférés en premier vers l’abonné. Si **aucun**, il est supposé que l’abonné possède déjà le schéma et les données initiales pour les tables publiées. Les données et les tables système sont toujours transférées.  
  
> [!NOTE]  
>  Nous vous recommandons de ne pas spécifier de valeur **None**.  
  
`[ @frequency_type = ] frequency_type` Valeur indiquant le moment où l’Agent de fusion s’exécutera. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**10**|Mensuelle|  
|**20**|Mensuellement, en fonction de l'intervalle de fréquence|  
|**40**|Au démarrage de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|NULL (par défaut)||  
  
`[ @frequency_interval = ] frequency_interval` Jour ou jours d’exécution de l’Agent de fusion. *frequency_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
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
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Occurrence de fusion planifiée de l’intervalle de fréquence de chaque mois. *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
|NULL (par défaut)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Unité de *frequency_subday_interval*. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Fréquence à laquelle *frequency_subday* se produisent entre chaque fusion. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent de fusion est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent de fusion cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent de fusion est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent de fusion cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @optional_command_line = ] 'optional_command_line'` Est l’invite de commandes facultative à exécuter. *optional_command_line* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. Cet argument est utilisé pour ajouter une commande qui permet de capturer le résultat et de le sauvegarder dans un fichier ou de spécifier un fichier de configuration ou un attribut.  
  
`[ @description = ] 'description'` Brève description de cet abonnement de fusion. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Cette valeur est affichée par le moniteur de réplication dans la colonne **nom convivial** , qui peut être utilisée pour trier les abonnements pour une publication analysée.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Spécifie si l’abonnement peut être synchronisé via le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestionnaire de synchronisation Windows. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation. Si la **valeur est true**, l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @offloadagent = ] remote_agent_activation` Spécifie que l’agent peut être activé à distance. *remote_agent_activation* est de **bits** avec **0** comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est conservé que pour la compatibilité descendante des scripts.  
  
`[ @offloadserver = ] 'remote_agent_server_name'` Spécifie le nom réseau du serveur à utiliser pour l’activation de l’agent distant. *remote_agent_server_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver'` Permet de résoudre les conflits de manière interactive pour tous les articles qui autorisent la résolution interactive. *use_interactive_resolver* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
`[ @merge_job_name = ] 'merge_job_name'`Le paramètre *\@ merge_job_name* est déconseillé et ne peut pas être défini. *merge_job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @hostname = ] 'hostname'` Remplace la valeur retournée par [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) lorsque cette fonction est utilisée dans la clause WHERE d’un filtre paramétré. *Hostname* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Pour des raisons de performance, il est recommandé de ne pas appliquer de fonctions aux noms de colonne dans les clauses des filtres de lignes paramétrables, telles que `LEFT([MyColumn]) = SUSER_SNAME()`. Si vous utilisez [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) dans une clause de filtre et que vous remplacez la valeur de HOST_NAME, il peut être nécessaire de convertir les types de données à l’aide de [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Pour plus d'informations sur la conduite à adopter dans cette situation, consultez la section « Substitution de la valeur de HOST_NAME » de la rubrique [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergesubscription** est utilisé dans la réplication de fusion.  
  
 Lorsque **sp_addmergesubscription** est exécuté par un membre du rôle serveur fixe **sysadmin** pour créer un abonnement par émission de type push, le travail agent de fusion est implicitement créé et s’exécute sous le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service agent. Nous vous recommandons d’exécuter [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) et de spécifier les informations d’identification d’un autre compte Windows spécifique à l’agent pour **\@ job_login** et **\@ job_password**. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergesubscription-_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Résolution interactive des conflits](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_changemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  

---
description: sp_changesubscriber (Transact-SQL)
title: sp_changesubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriber
- sp_changesubscriber_TSQL
helpviewer_keywords:
- sp_changesubscriber
ms.assetid: d453c451-e957-490f-b968-5e03aeddaf10
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3f920abb3544800d99ba108e024e200fad769f1a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189609"
---
# <a name="sp_changesubscriber-transact-sql"></a>sp_changesubscriber (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les options d'un Abonné. Toute tâche de distribution destinée aux Abonnés à ce serveur de publication est mise à jour. Cette procédure stockée écrit dans la table **MSsubscriber_info** de la base de données de distribution. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubscriber [ @subscriber= ] 'subscriber'  
    [ , [ @type= ] type ]  
    [ , [ @login= ] 'login' ]  
    [ , [ @password= ] 'password' ]  
    [ , [ @commit_batch_size= ] commit_batch_size ]  
    [ , [ @status_batch_size= ] status_batch_size ]  
    [ , [ @flush_frequency= ] flush_frequency ]  
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
    [ , [ @description= ] 'description' ]  
    [ , [ @security_mode= ] security_mode ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné sur lequel modifier les options. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @type = ] type` Type d’abonné. *type* est de type **tinyint**, avec NULL comme valeur par défaut. **0** indique un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonné. **1** spécifie un abonné de serveur de source de données ODBC qui n’est pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou.  
  
`[ @login = ] 'login'` ID de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion d’authentification. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
`[ @password = ] 'password'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Mot de passe d’authentification. *Password* est de **type sysname**, avec la valeur par défaut **%** . **%** indique qu’il n’y a aucune modification de la propriété de mot de passe.  
  
`[ @commit_batch_size = ] commit_batch_size` Pris en charge pour la compatibilité descendante uniquement.  
  
`[ @status_batch_size = ] status_batch_size` Pris en charge pour la compatibilité descendante uniquement.  
  
`[ @flush_frequency = ] flush_frequency` Pris en charge pour la compatibilité descendante uniquement.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de la tâche de distribution. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
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
  
`[ @frequency_interval = ] frequency_interval` Intervalle de *frequency_type*. *frequency_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la tâche de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Fréquence de répétition de la tâche de distribution au cours de la *frequency_type* définie. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequence_subday*. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure du jour de la première planification de la tâche de distribution, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle la tâche de distribution cesse d’être planifiée, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle la tâche de distribution est planifiée pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle la tâche de distribution cesse d’être planifiée, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @description = ] 'description'` Description facultative du texte. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @security_mode = ] security_mode` Est le mode de sécurité implémenté. *security_mode* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentification|  
|**1**|Authentification Windows|  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la modification des propriétés d’un article sur un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubscriber** est utilisé dans tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_changesubscriber**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

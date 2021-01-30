---
description: sp_addsubscriber (Transact-SQL)
title: sp_addsubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscriber
- sp_addsubscriber_TSQL
helpviewer_keywords:
- sp_addsubscriber
ms.assetid: b8a584ea-2a26-4936-965b-b84f026e39c0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9839d1daa3316fa8cb47af0ed2e362eba194f414
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212005"
---
# <a name="sp_addsubscriber-transact-sql"></a>sp_addsubscriber (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Ajoute un nouvel Abonné à un serveur de publication, lui permettant ainsi de recevoir des publications. Pour les publications transactionnelles et d'instantanés, cette procédure stockée s'exécute à partir du serveur de publication sur la base de données de publication. Pour les publications de fusion, elle s'exécute au niveau d'un serveur de distribution distant.  
  
> [!IMPORTANT]  
>  Cette procédure stockée est désormais déconseillée. Vous n'avez donc plus besoin d'enregistrer un Abonné de façon explicite auprès du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsubscriber [ @subscriber = ] 'subscriber'  
    [ , [ @type = ] type ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @commit_batch_size = ] commit_batch_size ]  
    [ , [ @status_batch_size = ] status_batch_size ]  
    [ , [ @flush_frequency = ] flush_frequency ]  
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
    [ , [ @description = ] 'description' ]  
    [ , [ @security_mode = ] security_mode ]  
    [ , [ @encrypted_password = ] encrypted_password ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @subscriber = ] 'subscriber'` Nom du serveur à ajouter en tant qu’abonné valide aux publications de ce serveur. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @type = ] type` Type de l’abonné. *type* est de type **tinyint** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Abonné|  
|**1**|Serveur de la source de données ODBC.|  
|**2**|Base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|**3**|Fournisseur OLE DB|  
  
`[ @login = ] 'login'` ID de connexion pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @password = ] 'password'` Mot de passe pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. *Password* est de type **nvarchar (524)**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @commit_batch_size = ] commit_batch_size` Ce paramètre est déconseillé et conservé pour la compatibilité descendante des scripts.  
  
> [!NOTE]  
>  Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @status_batch_size = ] status_batch_size` Ce paramètre est déconseillé et conservé pour la compatibilité descendante des scripts.  
  
> [!NOTE]  
>  Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @flush_frequency = ] flush_frequency` Ce paramètre est déconseillé et conservé pour la compatibilité descendante des scripts.  
  
> [!NOTE]  
>  Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de l’agent de réplication. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2**|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64** (valeur par défaut)|Démarrage automatique|  
|**128**|Périodique|  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_interval = ] frequency_interval` Valeur appliquée à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, avec 1 comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de l’agent de réplication. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1** (par défaut)|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec **0** comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec **5** comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle l’agent de réplication est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec **0** comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle l’agent de réplication cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec 235959 comme valeur par défaut, ce qui signifie 11:59:59 P.M. mesurée sur une horloge de 24 heures.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle l’agent de réplication est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec 0 comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle l’agent de réplication cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec 99991231 comme valeur par défaut, ce qui correspond au 31 décembre 9999.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @description = ] 'description'` Description textuelle de l’abonné. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @security_mode = ] security_mode` Est le mode de sécurité implémenté. *security_mode* est de **type int**, avec 1 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. La propriété est maintenant spécifiée sur une base par abonnement lors de l’exécution de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Lorsqu'une valeur est spécifiée, elle sert alors de valeur par défaut au moment de la création des abonnements pour cet Abonné et un message d'avertissement s'affiche.  
  
`[ @encrypted_password = ] encrypted_password` Ce paramètre est déconseillé et fourni à des fins de compatibilité descendante uniquement *encrypted_password* à n’importe quelle valeur, mais **0** génère une erreur.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la publication à partir d’un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addsubscriber** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
 **sp_addsubscriber** n’est pas nécessaire lorsque l’abonné dispose uniquement d’abonnements anonymes aux publications de fusion.  
  
 **sp_addsubscriber** écrit dans la table [MSsubscriber_info](../../relational-databases/system-tables/mssubscriber-info-transact-sql.md) de la base de données de **distribution** .  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_addsubscriber**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [sp_changesubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)  
  
  

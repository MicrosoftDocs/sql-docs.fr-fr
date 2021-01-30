---
description: sp_changesubscriber_schedule (Transact-SQL)
title: sp_changesubscriber_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriber_schedule
- sp_changesubscriber_schedule_TSQL
helpviewer_keywords:
- sp_changesubscriber_schedule
ms.assetid: ff84e8e2-d496-482c-b23e-38a6626596e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e93079a747c3766a47ababf3fdd866053b40968
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171763"
---
# <a name="sp_changesubscriber_schedule-transact-sql"></a>sp_changesubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie la planification de l'Agent de distribution ou de fusion pour un abonné. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubscriber_schedule [ @subscriber = ] 'subscriber', [ @agent_type = ] type  
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
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**. Le nom de l'Abonné doit être unique dans la base de données, ne doit pas déjà exister et ne peut pas avoir la valeur NULL.  
  
`[ @agent_type = ] type` Type d’agent. le *type* est **smallint**, avec **0** comme valeur par défaut. **0** indique un agent de distribution. **1** indique un agent de fusion.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de la tâche de distribution. *frequency_type* est de **type int**, avec **64** comme valeur par défaut. Il y a 10 colonnes de planification.  
  
`[ @frequency_interval = ] frequency_interval` Valeur appliquée à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, avec **1** comme valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la tâche de distribution. *frequency_relative_interval* est de **type int**, avec **1** comme valeur par défaut.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Indique, en minutes, la fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int**, avec **4** comme valeur par défaut.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec **5** comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure du jour de la première planification de la tâche de distribution. *active_start_time_of_day* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle la tâche de distribution cesse d’être planifiée. *active_end_time_of_day* est de **type int**, avec **235959** comme valeur par défaut, ce qui signifie 11:59:59 P.M. avec un affichage horaire au format 24 heures).  
  
`[ @active_start_date = ] active_start_date` Date à laquelle la tâche de distribution est planifiée pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle la tâche de distribution cesse d’être planifiée, au format AAAAMMJJ. *active_end_date* est de **type int**, avec **99991231** comme valeur par défaut, ce qui correspond au 31 décembre 9999.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la modification des propriétés d’un article sur un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubscriber_schedule** est utilisé dans tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_changesubscriber_schedule**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscriber_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-schedule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

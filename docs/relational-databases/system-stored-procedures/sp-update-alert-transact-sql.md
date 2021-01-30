---
description: sp_update_alert (Transact-SQL)
title: sp_update_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_alert_TSQL
- sp_update_alert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_alert
ms.assetid: 4bbaeaab-8aca-4c9e-abc1-82ce73090bd3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c9dce24d3f8dd20d2138ba153fee74212e603e6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184754"
---
# <a name="sp_update_alert-transact-sql"></a>sp_update_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour les paramètres d'une alerte existante.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_alert   
     [ @name =] 'name'   
     [ , [ @new_name =] 'new_name']   
     [ , [ @enabled =] enabled]   
     [ , [ @message_id =] message_id]   
     [ , [ @severity =] severity]   
     [ , [ @delay_between_responses =] delay_between_responses]   
     [ , [ @notification_message =] 'notification_message']   
     [ , [ @include_event_description_in =] include_event_description_in]   
     [ , [ @database_name =] 'database']   
     [ , [ @event_description_keyword =] 'event_description_keyword']   
     [ , [ @job_id =] job_id | [@job_name =] 'job_name']   
     [ , [ @occurrence_count = ] occurrence_count]   
     [ , [ @count_reset_date =] count_reset_date]   
     [ , [ @count_reset_time =] count_reset_time]   
     [ , [ @last_occurrence_date =] last_occurrence_date]   
     [ , [ @last_occurrence_time =] last_occurrence_time]   
     [ , [ @last_response_date =] last_response_date]   
     [ , [ @last_response_time =] last_response _time]  
     [ , [ @raise_snmp_trap =] raise_snmp_trap]  
     [ , [ @performance_condition =] 'performance_condition' ]   
     [ , [ @category_name =] 'category']  
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom de l’alerte à mettre à jour. *Name* est de **type sysname**, sans valeur par défaut.  
  
`[ @new_name = ] 'new_name'` Nouveau nom pour l’alerte. Le nom doit être unique. *new_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @enabled = ] enabled` Spécifie si l’alerte est activée (**1**) ou désactivée (**0**). *Enabled* est de **type tinyint**, avec NULL comme valeur par défaut. Pour pouvoir se déclencher, une alerte doit être activée.  
  
`[ @message_id = ] message_id` Nouveau message ou numéro d’erreur pour la définition de l’alerte. En règle générale, *message_id* correspond à un numéro d’erreur dans la table **sysmessages** . *message_id* est de **type int**, avec NULL comme valeur par défaut. Un ID de message ne peut être utilisé que si le paramètre de niveau de gravité de l’alerte est **0**.  
  
`[ @severity = ] severity` Nouveau niveau de gravité (compris entre **1** et **25**) pour la définition de l’alerte. Tout [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message envoyé au journal des applications Windows avec la gravité spécifiée active l’alerte. *Severity* est de **type int**, avec NULL comme valeur par défaut. Un niveau de gravité ne peut être utilisé que si le paramètre ID de message de l’alerte est **égal à 0**.  
  
`[ @delay_between_responses = ] delay_between_responses` Nouvelle période d’attente, en secondes, entre les réponses à l’alerte. *delay_between_responses* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @notification_message = ] 'notification_message'` Texte révisé d’un message supplémentaire envoyé à l’opérateur dans le cadre de la notification par courrier électronique, **net send** ou radiomessagerie. *notification_message* est de type **nvarchar (512)**, avec NULL comme valeur par défaut.  
  
`[ @include_event_description_in = ] include_event_description_in` Spécifie si la description de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Erreur du journal des applications Windows doit être incluse dans le message de notification. *include_event_description_in* est de **type tinyint**, avec NULL comme valeur par défaut et peut prendre une ou plusieurs des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Aucun|  
|**1**|Messagerie électronique|  
|**2**|Récepteur de radiomessagerie|  
|**4**|**net send**|  
|**7**|Tous|  
  
`[ @database_name = ] 'database'` Nom de la base de données dans laquelle l’erreur doit se produire pour déclencher l’alerte. *Database est de* **type sysname.** Les noms placés entre crochets ([ ]) ne sont pas autorisés. La valeur par défaut est NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword'` Séquence de caractères qui doit être trouvée dans la description de l’erreur dans le journal des messages d’erreur. Les caractères correspondant au modèle d'expression [!INCLUDE[tsql](../../includes/tsql-md.md)] LIKE sont admis. *event_description_keyword* est de type **nvarchar (100)**, avec NULL comme valeur par défaut. Ce paramètre est utile pour filtrer les noms d’objets (par exemple, **% customer_table%**).  
  
`[ @job_id = ] job_id` Numéro d’identification du travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut. Si *job_id* est spécifié, *job_name* doit être omis.  
  
`[ @job_name = ] 'job_name'` Nom du travail qui s’exécute en réponse à cette alerte. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Si *job_name* est spécifié, *job_id* doit être omis.  
  
`[ @occurrence_count = ] occurrence_count` Réinitialise le nombre de fois que l’alerte s’est produite. *occurrence_count* est de **type int**, avec NULL comme valeur par défaut, et ne peut avoir que la valeur **0**.  
  
`[ @count_reset_date = ] count_reset_date` Réinitialise la date de la dernière réinitialisation du nombre d’occurrences. *count_reset_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @count_reset_time = ] count_reset_time` Réinitialise l’heure de la dernière réinitialisation du nombre d’occurrences. *count_reset_time* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @last_occurrence_date = ] last_occurrence_date` Réinitialise la date de la dernière erreur de l’alerte. *last_occurrence_date* est de **type int**, avec NULL comme valeur par défaut, et ne peut avoir que la valeur **0**.  
  
`[ @last_occurrence_time = ] last_occurrence_time` Réinitialise l’heure de la dernière erreur de l’alerte. *last_occurrence_time* est de **type int**, avec NULL comme valeur par défaut, et ne peut avoir que la valeur **0**.  
  
`[ @last_response_date = ] last_response_date` Réinitialise la date à laquelle le service SQLServerAgent a répondu pour la dernière fois à l’alerte. *last_response_date* est de **type int**, avec NULL comme valeur par défaut, et ne peut avoir que la valeur **0**.  
  
`[ @last_response_time = ] last_response_time` Réinitialise l’heure à laquelle le service SQLServerAgent a répondu pour la dernière fois à l’alerte. *last_response_time* est de **type int**, avec NULL comme valeur par défaut, et ne peut avoir que la valeur **0**.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` Réservé.  
  
`[ @performance_condition = ] 'performance_condition'` Valeur exprimée au format **'**_itemcomparatorvalue_**'**. *performance_condition* est de type **nvarchar (512)**, avec NULL comme valeur par défaut et se compose de ces éléments.  
  
|Élément de format|Description|  
|--------------------|-----------------|  
|*Item*|Objet de performances, compteur de performances ou instance nommée du compteur.|  
|*Comparaison*|L’un des opérateurs suivants : **>** , **<** , **=**|  
|*Valeur*|Valeur numérique du compteur.|  
  
`[ @category_name = ] 'category'` Nom de la catégorie d’alerte. *Category* est de **type sysname** , avec NULL comme valeur par défaut.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Espace de noms WMI pour interroger les événements. *wmi_namespace* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @wmi_query = ] 'wmi_query'` Requête qui spécifie l’événement WMI pour l’alerte. *wmi_query* est de type **nvarchar (512)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Seul **sysmessages** écrit dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Journal des applications Windows peut déclencher une alerte.  
  
 **sp_update_alert** modifie uniquement les paramètres d’alerte pour lesquels des valeurs de paramètre sont fournies. Si un paramètre est manquant, la valeur actuelle est retenue.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent être membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie le paramètre activé depuis `Test Alert` à `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_alert  
    @name = N'Test Alert',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_help_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

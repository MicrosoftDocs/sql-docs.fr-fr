---
description: sp_add_alert (Transact-SQL)
title: sp_add_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_alert
- sp_add_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_alert
ms.assetid: d9b41853-e22d-4813-a79f-57efb4511f09
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b6d7d1e451949d17dc7e4637dc82b86c4225f06c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186039"
---
# <a name="sp_add_alert-transact-sql"></a>sp_add_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une alerte.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_alert [ @name = ] 'name'   
     [ , [ @message_id = ] message_id ]   
     [ , [ @severity = ] severity ]   
     [ , [ @enabled = ] enabled ]  
     [ , [ @delay_between_responses = ] delay_between_responses ]   
     [ , [ @notification_message = ] 'notification_message' ]   
     [ , [ @include_event_description_in = ] include_event_description_in ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @event_description_keyword = ] 'event_description_keyword_pattern' ]   
     [ , { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ]   
     [ , [ @raise_snmp_trap = ] raise_snmp_trap ]   
     [ , [ @performance_condition = ] 'performance_condition' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom de l’alerte. Ce nom apparaît dans le message envoyé par courrier électronique ou par radiomessagerie en réponse à l'alerte. Il doit être unique et peut contenir le caractère de pourcentage ( **%** ). *Name* est de **type sysname**, sans valeur par défaut.  
  
`[ @message_id = ] message_id` Numéro d’erreur du message qui définit l’alerte. (Il correspond généralement à un numéro d’erreur dans la table **sysmessages** .) *message_id* est de **type int**, avec **0** comme valeur par défaut. Si le niveau de *gravité* est utilisé pour définir l’alerte, *message_id* doit avoir la valeur **0** ou null.  
  
> [!NOTE]  
>  Seules les erreurs **sysmessages** écrites dans le journal des applications Microsoft Windows peuvent entraîner l’envoi d’une alerte.  
  
`[ @severity = ] severity` Niveau de gravité (compris entre **1** et **25**) qui définit l’alerte. Tout [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message stocké dans la table **sysmessages** envoyé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] Journal des applications Windows avec la gravité indiquée entraîne l’envoi de l’alerte. *Severity* est de **type int**, avec 0 comme valeur par défaut. Si *message_id* est utilisé pour définir l’alerte, la *gravité* doit être **0**.  
  
`[ @enabled = ] enabled` Indique l’état actuel de l’alerte. *Enabled* est de **type tinyint**, avec 1 comme valeur par défaut (activé). Si la **valeur est 0**, l’alerte n’est pas activée et ne se déclenche pas.  
  
`[ @delay_between_responses = ] delay_between_responses` Délai d’attente, en secondes, entre les réponses à l’alerte. *delay_between_responses* est de **type int**, avec **0** comme valeur par défaut, ce qui signifie qu’il n’y a pas d’attente entre les réponses (chaque occurrence de l’alerte génère une réponse). La réponse peut prendre l'une des formes suivantes, ou les deux :  
  
-   Une ou plusieurs notifications envoyées par courrier électronique ou par radiomessagerie  
  
-   Un travail à exécuter  
  
 En définissant cette valeur, il est possible d'éviter, par exemple, l'envoi d'un flot de messages par courrier électronique lorsqu'une alerte se produit à plusieurs reprises en peu de temps.  
  
`[ @notification_message = ] 'notification_message'` Message supplémentaire facultatif envoyé à l’opérateur dans le cadre de la notification par courrier électronique, **net send** ou radiomessagerie. *notification_message* est de type **nvarchar (512)**, avec NULL comme valeur par défaut. La spécification de *notification_message* est utile pour l’ajout de notes spéciales, telles que les procédures de réparation.  
  
`[ @include_event_description_in = ] include_event_description_in` Indique si la description de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erreur doit être incluse dans le message de notification. *include_event_description_in* est de **type tinyint**, avec **5** comme valeur par défaut (courrier électronique et **net send**) et une ou plusieurs de ces valeurs peuvent être combinées avec un opérateur logique **or** .  
  
> [!IMPORTANT]
>  Les options du récepteur de radiomessagerie et **net send** seront supprimées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Aucun|  
|**1**|Messagerie électronique|  
|**2**|Récepteur de radiomessagerie|  
|**4**|**net send**|  
  
`[ @database_name = ] 'database'` Base de données dans laquelle l’erreur doit se produire pour déclencher l’alerte. Si la *base de données* n’est pas fournie, l’alerte se déclenche indépendamment de l’endroit où l’erreur s’est produite. *Database est de* **type sysname**. Les noms placés entre crochets ([ ]) ne sont pas autorisés. La valeur par défaut est NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword_pattern'` Séquence de caractères que la description de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erreur doit être like. Les caractères correspondant au modèle d'expression [!INCLUDE[tsql](../../includes/tsql-md.md)] LIKE sont admis. *event_description_keyword_pattern* est de type **nvarchar (100)**, avec NULL comme valeur par défaut. Ce paramètre est utile pour filtrer les noms d’objets (par exemple, **% customer_table%**).  
  
`[ @job_id = ] job_id` Numéro d’identification du travail à exécuter en réponse à cette alerte. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail à exécuter en réponse à cette alerte. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` Non implémenté dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7,0. *raise_snmp_trap* est de **type tinyint**, avec 0 comme valeur par défaut.  
  
`[ @performance_condition = ] 'performance_condition'` Est une valeur exprimée au format'*itemcomparatorvalue*'. *performance_condition* est de type **nvarchar (512)** avec NULL comme valeur par défaut et se compose de ces éléments.  
  
|Élément de format|Description|  
|--------------------|-----------------|  
|*Item*|Objet de performances, compteur de performances ou instance nommée du compteur.|  
|*Comparaison*|L’un des opérateurs suivants : >, < ou =|  
|*Valeur*|Valeur numérique du compteur.|  
  
`[ @category_name = ] 'category'` Nom de la catégorie d’alerte. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Espace de noms WMI pour interroger les événements. *wmi_namespace* est de **type sysname**, avec NULL comme valeur par défaut. Seuls les espaces de noms situés sur le serveur local sont pris en charge.  
  
`[ @wmi_query = ] 'wmi_query'` Requête qui spécifie l’événement WMI pour l’alerte. *wmi_query* est de type **nvarchar (512)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_alert** doit être exécuté à partir de la base de données **msdb** .  
  
 La liste suivante indique les circonstances pour lesquelles les erreurs ou les messages générés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont transmis au journal des applications Windows et peuvent dès lors déclencher des alertes :  
  
-   Gravité 19 ou plus élevée erreurs **sys. messages**  
  
-   toute instruction RAISERROR appelée avec la syntaxe WITH LOG ;  
  
-   Toute erreur **sys. messages** modifiée ou créée à l’aide de **sp_altermessage**  
  
-   Tout événement enregistré à l’aide de **xp_logevent**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil simple, fonctionnant en mode graphique, qui permet de gérer tout le système d'alerte. Son utilisation est recommandée pour configurer une infrastructure d'alertes.  
  
 Si une alerte ne fonctionne pas correctement, vérifiez que :  
  
-   Le service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
-   L'événement est consigné dans le journal des applications Windows.  
  
-   L'alerte est activée.  
  
-   Les événements créés à l’aide de **xp_logevent** surviennent dans la base de données master. Ainsi, **xp_logevent** ne déclenche pas d’alerte sauf si la valeur **\@database_name** pour l’alerte est **'master'** ou NULL.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter la procédure **sp_add_alert**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute une alerte (Test Alert) qui exécute le travail `Back up the AdventureWorks2012 Database` lorsqu'elle se déclenche.  
  
> [!NOTE]  
>  Dans cet exemple, on suppose que le message 55001 et le travail `Back up the AdventureWorks2012 Database` existent déjà. L’exemple est présenté à titre d’illustration uniquement.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_alert  
    @name = N'Test Alert',  
    @message_id = 55001,   
   @severity = 0,   
   @notification_message = N'Error 55001 has occurred. The database will be backed up...',   
   @job_name = N'Back up the AdventureWorks2012 Database' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_delete_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [sys.sysPerfInfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

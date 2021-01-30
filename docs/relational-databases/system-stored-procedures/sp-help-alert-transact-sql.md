---
description: sp_help_alert (Transact-SQL)
title: sp_help_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_alert
- sp_help_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_alert
ms.assetid: 850cef4e-6348-4439-8e79-fd1bca712091
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b39195e5f1dd21fead42a05850f9b5e5e5874b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208935"
---
# <a name="sp_help_alert-transact-sql"></a>sp_help_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les alertes définies pour le serveur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_alert [ [ @alert_name = ] 'alert_name' ]   
     [ , [ @order_by = ] 'order_by' ]   
     [ , [ @alert_id = ] alert_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @legacy_format = ] legacy_format ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @alert_name = ] 'alert_name'` Nom de l’alerte. *alert_name* est **de type nvarchar (128)**. Si *alert_name* n’est pas spécifié, des informations sur toutes les alertes sont retournées.  
  
`[ @order_by = ] 'order_by'` Ordre de tri à utiliser pour produire les résultats. *order_by* est de **type sysname**, avec N'*Name*'comme valeur par défaut.  
  
`[ @alert_id = ] alert_id` Numéro d’identification de l’alerte pour laquelle signaler des informations. *alert_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @category_name = ] 'category'` Catégorie de l’alerte. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @legacy_format = ] legacy_format` Indique s’il faut générer un jeu de résultats hérité. *legacy_format* est de **bit**, avec **0** comme valeur par défaut. Lorsque *legacy_format* a la valeur **1**, **sp_help_alert** retourne le jeu de résultats retourné par **sp_help_alert** dans Microsoft SQL Server 2000.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Lorsque **\@ legacy_format** a la valeur **0**, **sp_help_alert** produit le jeu de résultats suivant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur entier unique attribué par le système.|  
|**name**|**sysname**|Nom de l’alerte (par exemple demo : journal **msdb** complet).|  
|**event_source**|**nvarchar(100**|Source de l'événement. Il s’agit toujours de **MSSQLSERVER** pour la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numéro du message d'erreur définissant l'alerte (Correspond généralement à un numéro d’erreur dans la table **sysmessages** ). Si le niveau de gravité est utilisé pour définir l’alerte, **message_id** a la valeur **0** ou null.|  
|**severity**|**int**|Niveau de gravité (compris entre **9** et **25**, **110**, **120**, **130** ou **140**) qui définit l’alerte.|  
|**activé**|**tinyint**|État indiquant si l’alerte est actuellement activée (**1**) ou non (**0**). Une alerte non activée ne peut pas être envoyée.|  
|**delay_between_responses**|**int**|Délai d'attente, en secondes, entre les réponses à l'alerte.|  
|**last_occurrence_date**|**int**|Date de la dernière apparition de l'alerte.|  
|**last_occurrence_time**|**int**|Heure de la dernière apparition de l'alerte.|  
|**last_response_date**|**int**|Date à laquelle le service **SQLServerAgent** a répondu pour la dernière fois à l’alerte.|  
|**last_response_time**|**int**|Heure à laquelle le service **SQLServerAgent** a répondu pour la dernière fois à l’alerte.|  
|**notification_message**|**nvarchar(512)**|Message supplémentaire facultatif qui sera envoyé à l'opérateur avec la notification par courrier électronique ou radiomessagerie.|  
|**include_event_description**|**tinyint**|Indique si la description de l'erreur de SQL Server contenue dans le journal des applications Windows doit apparaître dans le message de notification.|  
|**database_name**|**sysname**|Base de données dans laquelle l'erreur doit apparaître pour que l'alerte soit déclenchée. Si le nom de la base de données est NULL, l'alerte se déclenche où que se soit produite l'erreur.|  
|**event_description_keyword**|**nvarchar(100**|Description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le journal des applications Windows qui doit être identique à la séquence de caractères fournie.|  
|**occurrence_count**|**int**|Nombre de déclenchements de l'alerte.|  
|**count_reset_date**|**int**|Date de la dernière réinitialisation de la **occurrence_count** .|  
|**count_reset_time**|**int**|Heure à laquelle la **occurrence_count** a été réinitialisée pour la dernière fois.|  
|**job_id**|**uniqueidentifier**|Numéro d'identification du travail à exécuter en réponse à une alerte.|  
|**job_name**|**sysname**|Nom du travail à exécuter en réponse à une alerte.|  
|**has_notification**|**int**|Différent de zéro si un ou plusieurs opérateurs sont notifiés pour cette alerte. Le paramètre peut avoir les valeurs suivantes (liées par OR) :<br /><br /> **1**= a une notification par courrier électronique<br /><br /> **2**= notification de radiomessagerie<br /><br /> **4**= notification d' **envoi réseau** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**performance_condition**|**nvarchar(512)**|Si le **type** est **2**, cette colonne indique la définition de la condition de performance ; dans le cas contraire, la colonne a la valeur NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Sera toujours '[Uncategorized]' pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.|  
|**wmi_namespace**|**sysname**|Si le **type** est **3**, cette colonne indique l’espace de noms pour l’événement WMI.|  
|**wmi_query**|**nvarchar(512)**|Si le **type** est **3**, cette colonne affiche la requête pour l’événement WMI.|  
|**type**|**int**|Type de l'événement :<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte d’événement<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte de performances<br /><br /> **3** = alerte d’événement WMI|  
  
 Lorsque **\@ legacy_format** a la valeur **1**, **sp_help_alert** produit le jeu de résultats suivant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur entier unique attribué par le système.|  
|**name**|**sysname**|Nom de l’alerte (par exemple demo : journal **msdb** complet).|  
|**event_source**|**nvarchar(100**|Source de l'événement. Il s’agit toujours de **MSSQLSERVER** pour la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numéro du message d'erreur définissant l'alerte (Correspond généralement à un numéro d’erreur dans la table **sysmessages** ). Si le niveau de gravité est utilisé pour définir l’alerte, **message_id** a la valeur **0** ou null.|  
|**severity**|**int**|Niveau de gravité (compris entre **9** et **25**, **110**, **120**, **130** ou 1 **40**) qui définit l’alerte.|  
|**activé**|**tinyint**|État indiquant si l’alerte est actuellement activée (**1**) ou non (**0**). Une alerte non activée ne peut pas être envoyée.|  
|**delay_between_responses**|**int**|Délai d'attente, en secondes, entre les réponses à l'alerte.|  
|**last_occurrence_date**|**int**|Date de la dernière apparition de l'alerte.|  
|**last_occurrence_time**|**int**|Heure de la dernière apparition de l'alerte.|  
|**last_response_date**|**int**|Date à laquelle le service **SQLServerAgent** a répondu pour la dernière fois à l’alerte.|  
|**last_response_time**|**int**|Heure à laquelle le service **SQLServerAgent** a répondu pour la dernière fois à l’alerte.|  
|**notification_message**|**nvarchar(512)**|Message supplémentaire facultatif qui sera envoyé à l'opérateur avec la notification par courrier électronique ou radiomessagerie.|  
|**include_event_description**|**tinyint**|Indique si la description de l'erreur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenue dans le journal des applications Windows doit apparaître dans le message de notification.|  
|**database_name**|**sysname**|Base de données dans laquelle l'erreur doit apparaître pour que l'alerte soit déclenchée. Si le nom de la base de données est NULL, l'alerte se déclenche où que se soit produite l'erreur.|  
|**event_description_keyword**|**nvarchar(100**|Description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le journal des applications Windows qui doit être identique à la séquence de caractères fournie.|  
|**occurrence_count**|**int**|Nombre de déclenchements de l'alerte.|  
|**count_reset_date**|**int**|Date de la dernière réinitialisation de la **occurrence_count** .|  
|**count_reset_time**|**int**|Heure à laquelle la **occurrence_count** a été réinitialisée pour la dernière fois.|  
|**job_id**|**uniqueidentifier**|Numéro d’identification du travail.|  
|**job_name**|**sysname**|Nom d'un travail à la demande exécuté en réponse à une alerte.|  
|**has_notification**|**int**|Différent de zéro si un ou plusieurs opérateurs sont notifiés pour cette alerte. Le paramètre peut avoir une ou plusieurs des valeurs suivantes (combinées avec OR) :<br /><br /> **1**= a une notification par courrier électronique<br /><br /> **2**= notification de radiomessagerie<br /><br /> **4**= notification d' **envoi réseau** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
|**performance_condition**|**nvarchar(512)**|Si le **type** est **2**, cette colonne indique la définition de la condition de performance. Si le **type** est **3**, cette colonne affiche la requête pour l’événement WMI. Dans les autres cas, cette colonne est NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Sera toujours « [n’appartenant à aucune **catégorie]**» pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0.|  
|**type**|**int**|Type d'alerte :<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte d’événement<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte de performances<br /><br /> **3** = alerte d’événement WMI|  
  
## <a name="remarks"></a>Notes  
 **sp_help_alert** doit être exécuté à partir de la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer du rôle de base de données fixe **SQLAgentOperatorRole** dans la base de données **msdb** .  
  
 Pour plus d’informations sur les **SQLAgentOperatorRole**, consultez [SQL Server Agent des rôles de base de données fixes](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur l'alerte `Demo: Sev. 25 Errors`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_alert @alert_name = 'Demo: Sev. 25 Errors';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

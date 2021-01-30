---
description: sp_MSchange_merge_agent_properties (Transact-SQL)
title: sp_MSchange_merge_agent_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_merge_agent_properties_TSQL
- sp_MSchange_merge_agent_properties
helpviewer_keywords:
- sp_MSchange_merge_agent_properties
ms.assetid: f775fa0f-28c7-4863-89ce-7bcfa1ab8b5e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89622d10a242ef2bc7fe97a4a0a75457d8dbe412
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195438"
---
# <a name="sp_mschange_merge_agent_properties-transact-sql"></a>sp_MSchange_merge_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés d’un travail de Agent de fusion qui s’exécute sur un serveur de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] distribution ou version ultérieure. Cette procédure stockée est utilisée pour modifier des propriétés lorsque le serveur de publication s'exécute sur une instance de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Cette procédure stockée est exécutée au niveau du serveur de distribution sur la base de données de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_MSchange_merge_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Propriété de publication à modifier. *Property* est de **type sysname**, sans valeur par défaut.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété. *value* est de type **nvarchar (524)**, avec NULL comme valeur par défaut.  
  
 Le tableau ci-dessous décrit les propriétés modifiables de l'Agent de fusion et les limites applicables aux valeurs de ces propriétés.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**description**||Brève description de l'abonnement.|  
|**merge_job_login**||Nom de connexion du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute.|  
|**merge_job_password**||Mot de passe du compte Windows sous lequel le travail de l'Agent s'exécute.|  
|**publisher_login**||Nom de connexion à utiliser lors de la connexion à un serveur de publication pour synchroniser l'abonnement.|  
|**publisher_password**||Mot de passe de l’éditeur.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**publisher_security_mode**|**1**|Authentification Windows.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|Authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_login**||Nom de connexion à utiliser lors de la connexion à un Abonné pour synchroniser l'abonnement.|  
|**subscriber_password**||Mot de passe de l’abonné.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_security_mode**|**1**|Authentification Windows.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|Authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
> [!NOTE]  
>  Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_MSchange_merge_agent_properties** est utilisé dans la réplication de fusion.  
  
 Quand le serveur de publication s’exécute sur une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, vous devez utiliser [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md) pour modifier les propriétés d’un travail agent de fusion qui synchronise un abonnement par émission de notification qui s’exécute sur le serveur de distribution.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** sur le serveur de distribution peuvent exécuter **sp_MSchange_merge_agent_properties**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)   
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)  
  
  

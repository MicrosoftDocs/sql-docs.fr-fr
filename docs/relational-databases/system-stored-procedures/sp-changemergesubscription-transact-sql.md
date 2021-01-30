---
description: sp_changemergesubscription (Transact-SQL)
title: sp_changemergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergesubscription_TSQL
- sp_changemergesubscription
helpviewer_keywords:
- sp_changemergesubscription
ms.assetid: fd820f35-c189-4e2d-884d-b60c1c469f58
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83211e61ad8b6e241f46b1af0ba32266c9f673dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159815"
---
# <a name="sp_changemergesubscription-transact-sql"></a>sp_changemergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés sélectionnées d'un abonnement de fusion par envoi de données (push). Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergesubscription [ [ @publication= ] 'publication' ]  
    [ , [ @subscriber= ] 'subscriber'  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication à modifier. *publication* est de **type sysname**, avec NULL comme valeur par défaut. La publication doit déjà exister et respecter les règles applicables aux identificateurs.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier pour la publication donnée. *Property* est de **type sysname** et peut prendre l’une des valeurs de la table.  
  
`[ @value = ] 'value'` Nouvelle valeur de la *propriété* spécifiée. la *valeur* est de type **nvarchar (255)** et peut prendre l’une des valeurs de la table.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**description**||Description de cet abonnement de fusion.|  
|**priority**||Priorité de l’abonnement. La priorité est utilisée par le résolveur par défaut pour déterminer un gagnant lorsque des conflits sont détectés.|  
|**merge_job_login**||Nom de connexion du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute.|  
|**merge_job_password**||Mot de passe du compte Windows sous lequel l’agent s’exécute.|  
|**publisher_security_mode**|**1**|Utiliser l'authentification Windows pour la connexion au serveur de publication.|  
||**0**|Utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion au serveur de publication.|  
|**publisher_login**||Nom de connexion du côté du serveur de publication.|  
|**publisher_password**||Mot de passe renforcé pour la connexion au serveur de publication.|  
|**subscriber_security_mode**|**1**|Utilise l'authentification Windows pour la connexion à l'Abonné.|  
||**0**|Utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion à l'Abonné.|  
|**subscriber_login**||Nom de la connexion du côté Abonné.|  
|**subscriber_password**||Mot de passe renforcé pour la connexion de l'Abonné.|  
|**sync_type**|**Automatique**|Le schéma et les données initiales des tables publiées sont transférés en premier lieu vers l'Abonné.|  
||**Aucune**|L'Abonné dispose déjà du schéma et des données initiales pour les tables publiées ; les données et les tables système sont toujours transférées.|  
|**use_interactive_resolver**|**true**|Autorise la résolution interactive des conflits pour tous les articles autorisant la résolution interactive.|  
||**false**|Les conflits sont automatiquement résolus au moyen d'un programme de résolution par défaut ou personnalisé.|  
|NULL (par défaut)|NULL (par défaut)||  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergesubscription** est utilisé dans la réplication de fusion.  
  
 Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changemergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

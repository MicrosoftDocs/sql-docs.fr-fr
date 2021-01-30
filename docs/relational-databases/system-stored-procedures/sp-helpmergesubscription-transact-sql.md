---
description: sp_helpmergesubscription (Transact-SQL)
title: sp_helpmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergesubscription
- sp_helpmergesubscription_TSQL
helpviewer_keywords:
- sp_helpmergesubscription
ms.assetid: da564112-f769-4e67-9251-5699823e8c86
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274048dcfcd76e815d3f4ec159dded0571f66e43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210891"
---
# <a name="sp_helpmergesubscription-transact-sql"></a>sp_helpmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur un abonnement à une publication de fusion, par envoi (push) et par extraction (pull) de données. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur la base de données d'abonnement d'un Abonné de republication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergesubscription [ [ @publication=] 'publication']  
    [ , [ @subscriber=] 'subscriber']  
    [ , [ @subscriber_db=] 'subscriber_db']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
    [ , [ @found=] 'found' OUTPUT]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** . La publication doit déjà exister et se conformer aux règles applicables aux identificateurs. Si la valeur **%** est null ou, des informations sur toutes les publications de fusion et les abonnements de la base de données en cours sont retournées.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec la valeur par défaut **%** . Si le paramètre a la valeur NULL ou %, des informations sur tous les abonnements à la publication donnée sont renvoyées.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur toutes les bases de données d’abonnement.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. Le serveur de publication doit être un serveur valide. *Publisher* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur tous les serveurs de publication.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur toutes les bases de données du serveur de publication.  
  
`[ @subscription_type = ] 'subscription_type'` Type d’abonnement. *subscription_type* est de type **nvarchar (15)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Push** (par défaut)|Abonnement par envoi de données (push)|  
|**collecter**|Abonnement par extraction de données (pull)|  
|**versions**|Abonnement par envoi (push) et par extraction (pull) de données|  
  
`[ @found = ] 'found'OUTPUT` Indicateur qui signale le retour de lignes. *valeur* de **type int** et paramètre de sortie, avec NULL comme valeur par défaut. **1** indique que la publication est trouvée. **0** indique que la publication est introuvable.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**sysname**|Nom de l'abonnement.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**côté**|**sysname**|Nom de l'Abonné.|  
|**subscriber_db**|**sysname**|Nom de la base de données d'abonnement.|  
|**statut**|**int**|État de l’abonnement :<br /><br /> **0** = tous les travaux sont en attente de démarrage<br /><br /> **1** = un ou plusieurs travaux commencent<br /><br /> **2** = toutes les tâches ont été exécutées avec succès<br /><br /> **3** = au moins un travail est en cours d’exécution<br /><br /> **4** = tous les travaux sont planifiés et inactifs<br /><br /> **5** = au moins un travail tente de s’exécuter après un échec précédent<br /><br /> **6** = au moins un travail n’a pas réussi à s’exécuter correctement|  
|**subscriber_type**|**int**|Type d'Abonné.|  
|**subscription_type**|**int**|Type d'abonnement :<br /><br /> **0** = Push<br /><br /> **1** = extraction<br /><br /> **2** = les deux|  
|**priority**|**float (8)**|Numéro indiquant la priorité de l'abonnement.|  
|**sync_type**|**tinyint**|Type de synchronisation d'abonnement|  
|**description**|**nvarchar(255)**|Brève description de cet abonnement de fusion.|  
|**merge_jobid**|**Binary(16**|ID de travail de l'Agent de fusion.|  
|**full_publication**|**tinyint**|Indique si l'abonnement concerne une publication complète ou filtrée.|  
|**offload_enabled**|**bit**|Indique si le déchargement d'un Agent de réplication est configuré pour être exécuté sur l'Abonné. Si la valeur est NULL, l'exécution a lieu sur le serveur de publication.|  
|**offload_server**|**sysname**|Nom du serveur sur lequel s'exécute l'Agent.|  
|**use_interactive_resolver**|**int**|Indique si le composant résolveur interactif est utilisé ou non au cours de la réconciliation. Si la **valeur est 0**, le programme de résolution interactif n’est pas utilisé.|  
|**hostname**|**sysname**|Valeur fournie lorsqu’un abonnement est filtré par la valeur de la fonction [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) .|  
|**subscriber_security_mode**|**smallint**|Est le mode de sécurité de l’abonné, où **1** correspond à l’authentification Windows et **0** à l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**subscriber_login**|**sysname**|Nom de connexion sur l'Abonné.|  
|**subscriber_password**|**sysname**|Le mot de passe réel de l'Abonné n'est jamais renvoyé. Le résultat est masqué par une chaîne « **\*\*\*\*\*\*** ».|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergesubscription** est utilisé dans la réplication de fusion pour retourner les informations d’abonnement stockées sur le serveur de publication ou sur l’abonné de republication.  
  
 Pour les abonnements anonymes, la valeur *subscription_type* est toujours **1** (extraction). Toutefois, vous devez exécuter [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md) sur l’abonné pour obtenir des informations sur les abonnements anonymes.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , le rôle de base de données fixe **db_owner** ou la liste d’accès à la publication pour la publication à laquelle appartient l’abonnement peuvent exécuter **sp_helpmergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

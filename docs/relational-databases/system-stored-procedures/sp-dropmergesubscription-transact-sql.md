---
description: sp_dropmergesubscription (Transact-SQL)
title: sp_dropmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergesubscription_TSQL
- sp_dropmergesubscription
helpviewer_keywords:
- sp_dropmergesubscription
ms.assetid: 34244ae6-bd98-4a6a-bbd3-85f50edfcdc0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29cf6541b12f49194c1d1a6562d3fb80975a5591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204797"
---
# <a name="sp_dropmergesubscription-transact-sql"></a>sp_dropmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un abonnement à une publication de fusion et à l'Agent de fusion qui lui est associé. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropmergesubscription [ [ @publication= ] 'publication' ]   
    [ , [ @subscriber= ] 'subscriber'    
    [ , [ @subscriber_db= ] 'subscriber_db' ]   
    [ , [ @subscription_type= ] 'subscription_type' ]   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec NULL comme valeur par défaut. La publication doit déjà exister et se conformer aux règles applicables aux identificateurs.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscription_database* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscription_type = ] 'subscription_type'` Type d’abonnement. *subscription_type* est de type **nvarchar (15)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**tous les**|Abonnements par envoi de données (push), par extraction de données (pull) et anonymes.|  
|**façon**|Abonnement anonyme.|  
|**push**|Abonnement par envoi de données (push).|  
|**collecter**|Abonnement par extraction de données (pull).|  
|**both** (valeur par défaut)|Abonnements par envoi et extraction de données.|  
  
`[ @ignore_distributor = ] ignore_distributor` Indique si cette procédure stockée est exécutée sans se connecter au serveur de distribution. *ignore_distributor* est de **bit**, avec **0** comme valeur par défaut. Ce paramètre peut être utilisé pour supprimer un abonnement sans effectuer de tâches de nettoyage sur le serveur de distribution. Il est également utile si vous devez réinstaller le serveur de distribution.  
  
`[ @reserved = ] reserved` Est réservé pour une utilisation ultérieure. la valeur *réservée* est de **bit**, avec **0** comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropmergesubscription** est utilisé dans la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropmergesubscription_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_dropmergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer un abonnement par envoi de notification](../../relational-databases/replication/delete-a-push-subscription.md)   
 [Supprimer un abonnement par extraction](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  

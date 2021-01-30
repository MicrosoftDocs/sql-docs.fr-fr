---
description: sp_expired_subscription_cleanup (Transact-SQL)
title: sp_expired_subscription_cleanup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_expired_subscription_cleanup
- SP_EXPIRED_SUBSCRIPTION_CLEANUP_TSQL
helpviewer_keywords:
- sp_expired_subscription_cleanup
ms.assetid: 6abc29fe-d77a-4673-9d99-ae31c688012c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c555f2aaabf3e245261a07cc0bc7257e2601926
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187972"
---
# <a name="sp_expired_subscription_cleanup-transact-sql"></a>sp_expired_subscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contrôle l'état de tous les abonnements de chaque publication et supprime ceux qui ont expiré. Cette procédure stockée est exécutée sur le serveur de publication sur n’importe quelle base de données ou sur le serveur de distribution de la base de données de distribution pour un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_expired_subscription_cleanup [ [ @publisher = ] 'publisher' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom d’un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publication* est de **type sysname**, avec NULL comme valeur par défaut. Ne spécifiez pas ce paramètre pour un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_expired_subscription_cleanup** est utilisé dans tous les types de réplications.  
  
 **sp_expired_subscription_cleanup** est exécutée par la tâche de nettoyage d’abonnement expirée pour détecter et supprimer les abonnements expirés des bases de données de publication toutes les 24 heures. Si l'un d'entre eux est périmé, c'est-à-dire s'il n'a pas été synchronisé avec le serveur de publication au cours de la période de rétention, la publication est considérée comme étant arrivée à expiration et les traces de l'abonnement sont effacées du serveur de publication. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_expired_subscription_cleanup**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md)   
 [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

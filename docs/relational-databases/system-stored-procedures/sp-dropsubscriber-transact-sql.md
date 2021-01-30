---
description: sp_dropsubscriber (Transact-SQL)
title: sp_dropsubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscriber_TSQL
- sp_dropsubscriber
helpviewer_keywords:
- sp_dropsubscriber
ms.assetid: 8c6eb282-81b5-4ec4-b691-aa061d9267dc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 96ba954cf4674c370b33e783c8f71dd4da69277b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187002"
---
# <a name="sp_dropsubscriber-transact-sql"></a>sp_dropsubscriber (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Supprime la désignation de l'Abonné dans un serveur enregistré. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Cette procédure stockée est désormais déconseillée. Vous n'avez donc plus besoin d'enregistrer un Abonné de façon explicite auprès du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropsubscriber [ @subscriber= ] 'subscriber'   
    [ , [ @reserved= ] 'reserved' ]   
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné à supprimer. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropsubscriber** est utilisé dans tous les types de réplications.  
  
 Cette procédure stockée supprime la **sous** -option Server et supprime le mappage de connexion à distance de l’administrateur système pour **repl_subscriber**.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dropsubscriber**.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer un abonnement par envoi de notification](../../relational-databases/replication/delete-a-push-subscription.md)   
 [Supprimer un abonnement par extraction](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

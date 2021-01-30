---
description: sp_reinitmergesubscription (Transact-SQL)
title: sp_reinitmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitmergesubscription_TSQL
- sp_reinitmergesubscription
helpviewer_keywords:
- sp_reinitmergesubscription
ms.assetid: 249a4048-e885-48e0-a92a-6577f59de751
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d4a16d71453d08fca6ab496cc103e43edbc160e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185678"
---
# <a name="sp_reinitmergesubscription-transact-sql"></a>sp_reinitmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Signale un abonnement de fusion en vue de sa réinitialisation lors de la prochaine exécution de l’Agent de fusion. Cette procédure stockée est exécutée dans la base de données de publication du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_reinitmergesubscription [ [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber'  
    [ , [ @subscriber_db = ] 'subscriber_db'  
    [ , [ @upload_first = ] 'upload_first'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec **All** comme valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec **All** comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données de l’abonné. *subscriber_db* est de **type sysname**, avec **All** comme valeur par défaut.  
  
`[ @upload_first = ] 'upload_first'` Indique si les modifications apportées à l’abonné sont téléchargées avant la réinitialisation de l’abonnement. *upload_first* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, les modifications sont téléchargées avant la réinitialisation de l’abonnement. Si la **valeur est false**, les modifications ne sont pas téléchargées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_reinitmergesubscription** est utilisé dans la réplication de fusion.  
  
 **sp_reinitmergesubscription** peut être appelée à partir du serveur de publication pour réinitialiser les abonnements de fusion. Il est recommandé de réexécuter également l'Agent d'instantané.  
  
 Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
## <a name="examples"></a>Exemples  

### <a name="a-reinitialize-the-push-subscription-and-lose-pending-changes"></a>R. Réinitialiser l’abonnement par émission de type push et perdre les modifications en attente

 [!code-sql[HowTo#sp_reinitmergepushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_1.sql)]  
  
### <a name="b-reinitialize-the-push-subscription-and-upload-pending-changes"></a>B. Réinitialiser l’abonnement par émission de type push et charger les modifications en attente
 [!code-sql[HowTo#sp_reinitmergepushsubwithupload](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_2.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_reinitmergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

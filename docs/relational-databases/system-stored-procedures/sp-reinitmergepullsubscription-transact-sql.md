---
description: sp_reinitmergepullsubscription (Transact-SQL)
title: sp_reinitmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitmergepullsubscription
- sp_reinitmergepullsubscription_TSQL
helpviewer_keywords:
- sp_reinitmergepullsubscription
ms.assetid: 48464bc9-60aa-4886-b526-163f010102b8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2fb9aaa1ccc6cbd8e89b42a2e82c8a663e61cc91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185687"
---
# <a name="sp_reinitmergepullsubscription-transact-sql"></a>sp_reinitmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Signale un abonnement de fusion extrait en vue de sa réinitialisation lors de la prochaine exécution de l'Agent de fusion. Cette procédure stockée est exécutée dans la base de données d'abonnement de l'abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_reinitmergepullsubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @upload_first = ] 'upload_first'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec All comme valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec All comme valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec All comme valeur par défaut.  
  
`[ @upload_first = ] 'upload_first'` Indique si les modifications apportées à l’abonné sont téléchargées avant la réinitialisation de l’abonnement. *upload_first* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, les modifications sont téléchargées avant la réinitialisation de l’abonnement. Si la **valeur est false**, les modifications ne sont pas téléchargées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_reinitmergepullsubscription** est utilisé dans la réplication de fusion.  
  
 Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
## <a name="examples"></a>Exemples  

### <a name="a-reinitialize-the-pull-subscription-and-lose-pending-changes"></a>R. Réinitialiser l’abonnement par extraction et perdre les modifications en attente

[!code-sql[HowTo#sp_reinitmergepullsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergepullsubscr_1.sql)]  
  
### <a name="b-reinitialize-the-pull-subscription-and-upload-pending-changes"></a>B. Réinitialiser l’abonnement par extraction et charger les modifications en attente

 [!code-sql[HowTo#sp_reinitmergepullsubwithupload](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergepullsubscr_2.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_reinitmergepullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

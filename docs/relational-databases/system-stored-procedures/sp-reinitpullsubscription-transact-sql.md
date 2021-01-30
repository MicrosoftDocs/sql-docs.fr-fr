---
description: sp_reinitpullsubscription (Transact-SQL)
title: sp_reinitpullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitpullsubscription_TSQL
- sp_reinitpullsubscription
helpviewer_keywords:
- sp_reinitpullsubscription
ms.assetid: 7d9abe49-ce92-47f3-82c9-aea749518c91
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c77a8e055da1e37712bda31c5d40d51201a40839
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185700"
---
# <a name="sp_reinitpullsubscription-transact-sql"></a>sp_reinitpullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Signale un abonnement par extraction de données (pull) transactionnel ou anonyme en vue de sa réinitialisation lors de la prochaine exécution de l’Agent de distribution. Cette procédure stockée est exécutée sur la base de données d'abonnement par extraction de données (pull) de l'abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_reinitpullsubscription [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec All comme valeur par défaut, ce qui marque tous les abonnements pour la réinitialisation.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_reinitpullsubscription** est utilisé dans la réplication transactionnelle.  
  
 **sp_reinitpullsubscription** n’est pas pris en charge pour la réplication transactionnelle d’égal à égal.  
  
 **sp_reinitpullsubscription** peut être appelée à partir de l’abonné pour réinitialiser l’abonnement lors de la prochaine exécution du agent de distribution.  
  
 Les abonnements aux publications créées avec la valeur **false** pour **\@ immediate_sync** ne peuvent pas être réinitialisés à partir de l’abonné.  
  
 Pour réinitialiser un abonnement par extraction, vous pouvez soit exécuter **sp_reinitpullsubscription** sur l’abonné, soit **sp_reinitsubscription** sur le serveur de publication.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_reinitpullsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitpullsubscriptio_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_reinitpullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

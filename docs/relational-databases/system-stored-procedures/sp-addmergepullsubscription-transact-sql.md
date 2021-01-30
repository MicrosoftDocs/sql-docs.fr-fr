---
description: sp_addmergepullsubscription (Transact-SQL)
title: sp_addmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_TSQL
- sp_addmergepullsubscription
helpviewer_keywords:
- sp_addmergepullsubscription
ms.assetid: d63909a0-8ea7-4734-9ce8-8204d936a3e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 54975d84e497f02dd28191c59041553994dac24d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209407"
---
# <a name="sp_addmergepullsubscription-transact-sql"></a>sp_addmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un abonnement par extraction de données (pull) à une publication de fusion. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepullsubscription [ @publication= ] 'publication'   
    [ , [ @publisher= ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @subscriber_type= ] 'subscriber_type' ]   
    [ , [ @subscription_priority= ] subscription_priority ]   
    [ , [ @sync_type= ] 'sync_type' ]   
    [ , [ @description= ] 'description' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname** et sa valeur par défaut est le nom du serveur local. Le serveur de publication doit être un serveur valide.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_type = ] 'subscriber_type'` Type de l’abonné. *subscriber_type* est de type **nvarchar (15)** et peut être **Global**, **local** ou **anonyme**. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, les abonnements locaux sont appelés abonnements client et les abonnements globaux sont appelés abonnements serveur.  
  
`[ @subscription_priority = ] subscription_priority` Priorité de l’abonnement. *subscription_priority* est **Real**, avec NULL comme valeur par défaut. Pour les abonnements locaux et anonymes, la priorité est **0,0**. La priorité est utilisée par le résolveur par défaut pour déterminer un gagnant lorsque des conflits sont détectés. Pour les abonnés globaux, la priorité de l'abonnement doit être inférieure à 100, qui correspond à la priorité du serveur de publication.  
  
`[ @sync_type = ] 'sync_type'` Type de synchronisation de l’abonnement. *sync_type* est de type **nvarchar (15)**, avec **Automatic** comme valeur par défaut. Peut être **automatique** ou **aucun**. Si la valeur est **Automatic**, le schéma et les données initiales des tables publiées sont transférés en premier vers l’abonné. Si **aucun**, il est supposé que l’abonné possède déjà le schéma et les données initiales pour les tables publiées. Les données et les tables système sont toujours transférées.  
  
> [!NOTE]  
>  Nous vous déconseillons de spécifier une valeur **None**.  
  
`[ @description = ] 'description'` Brève description de cet abonnement par extraction. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Cette valeur est affichée par le moniteur de réplication dans la colonne **nom convivial** , qui peut être utilisée pour trier les abonnements pour une publication analysée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepullsubscription** est utilisé pour la réplication de fusion.  
  
 Si vous utilisez l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent pour synchroniser l’abonnement, la procédure stockée [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) doit être exécutée sur l’abonné pour créer un agent et un travail à synchroniser avec la publication.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_1.sql)]  
  
 [!code-sql[HowTo#sp_addmergepullsub_websync_anon](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_2.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergepullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  

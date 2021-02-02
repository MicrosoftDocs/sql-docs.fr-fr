---
title: Activer les abonnements pouvant être mis à jour pour les publications transactionnelles
description: Découvrez comment activer les abonnements pouvant être mis à jour pour une publication transactionnelle dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, updatable subscriptions
- updatable subscriptions, enabling
- subscriptions [SQL Server replication], updatable
ms.assetid: 539d5bb0-b808-4d8c-baf4-cb6d32d2c595
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2d31aa07018e7c41ce4922428d7196eb66c050b8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076687"
---
# <a name="enable-updating-subscriptions-for-transactional-publications"></a>Activer les abonnements pouvant être mis à jour pour les publications transactionnelles
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment activer les abonnements de mise à jour pour les publications transactionnelles dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> **REMARQUE** [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Activez la mise à jour d'abonnements pour les publications transactionnelles dans la page **Type de publication** de l'Assistant Nouvelle publication.  
  
 Pour utiliser les abonnements mis à jour, vous devez aussi configurer des options dans l'Assistant Nouvel abonnement.  
  
#### <a name="to-enable-updating-subscriptions"></a>Pour activer les abonnements mis à jour  
  
1.  Dans la page **Type de publication** de l'Assistant Nouvelle publication, sélectionnez **Publication transactionnelle avec abonnements pouvant être mis à jour**.  
  
2.  Dans la page **Sécurité de l'Agent** , spécifiez des paramètres de sécurité pour l'Agent de lecture de file d'attente, en plus de l'Agent d'instantané et de l'Agent de lecture du journal. Pour plus d'informations sur les autorisations nécessaires pour le compte sour lequel s'exécute l'Agent de lecture de file d'attente, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  

    > **REMARQUE :** L’Agent de lecture de la file d’attente est configuré même si vous n’utilisez que des abonnements mis à jour immédiatement.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Lors de la création d'une publication transactionnelle par programme à l'aide de procédures stockées de réplication, vous pouvez activer les abonnements avec mise à jour immédiate ou avec mise à jour en file d'attente.  
  
#### <a name="to-create-a-publication-that-supports-immediate-updating-subscriptions"></a>Pour créer une publication qui prend en charge les abonnements avec mise à jour immédiate  
  
1.  Si nécessaire, créez un travail de l'Agent de lecture du journal pour la base de données de publication.  
  
    -   Si un travail de l’Agent de lecture du journal existe déjà pour la base de données de publication, passez à l’étape 2.  
  
    -   Si vous ne savez pas si un travail de l’Agent de lecture du journal existe pour une base de données publiée, exécutez [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Si le jeu de résultats est vide, un travail de l'Agent de lecture du journal doit être créé.  
  
    -   Sur le serveur de publication, exécutez [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Indiquez les informations d’identification Windows [!INCLUDE[msCoName](../../../includes/msconame-md.md)] sous lesquelles l’agent s’exécute pour **\@job_login** et **\@job_password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**.  
  
2.  Exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), en spécifiant la valeur **true** pour le paramètre **\@allow_sync_tran**.  
  
3.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 2 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@job_name** et **\@password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
4.  Ajoutez des articles à la publication. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Au niveau de l'Abonné, créez un abonnement avec mise à jour à cette publication.   
  
#### <a name="to-create-a-publication-that-supports-queued-updating-subscriptions"></a>Pour créer une publication qui prend en charge les abonnements avec mise à jour en file d'attente  
  
1.  Si nécessaire, créez un travail de l'Agent de lecture du journal pour la base de données de publication.  
  
    -   Si un travail de l’Agent de lecture du journal existe déjà pour la base de données de publication, passez à l’étape 2.  
  
    -   Si vous ne savez pas si un travail de l’Agent de lecture du journal existe pour une base de données publiée, exécutez [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Si le jeu de résultats est vide, un travail de l'Agent de lecture du journal doit être créé.  
  
    -   Sur le serveur de publication, exécutez [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Indiquez les informations d’identification Windows sous lesquelles l’agent s’exécute pour **\@job_login** et **\@job_password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**.  
  
2.  Si nécessaire, créez un travail de l'Agent de lecture de la file d'attente pour le serveur de distribution.  
  
    -   Si un travail de l'Agent de lecture de la file d'attente existe déjà pour la base de données de distribution, passez à l'étape 3.  
  
    -   Si vous ne savez pas si un travail de l’Agent de lecture de la file d’attente existe pour la base de données de distribution, exécutez [sp_helpqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md) au niveau du serveur de distribution dans la base de données de distribution. Si le jeu de résultats est vide, un travail de l'Agent de lecture de la file d'attente doit être créé.  
  
    -   Sur le serveur de distribution, exécutez [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md). Indiquez les informations d’identification Windows sous lesquelles l’agent s’exécute pour **\@job_login** et **\@job_password**. Ces informations d'identification sont utilisées lorsque l'Agent de lecture de la file d'attente se connecte au serveur de publication et à l'Abonné. Pour plus d’informations, voir [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
3.  Exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), en affectant la valeur **true** au paramètre **\@allow_queued_tran** et la valeur **pub wins**, **sub reinit** ou **sub wins** à **\@conflict_policy**.  
  
4.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 3 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@snapshot_job_name** et **\@password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
5.  Ajoutez des articles à la publication. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Au niveau de l'Abonné, créez un abonnement avec mise à jour à cette publication.  
  
#### <a name="to-change-the-conflict-policy-for-a-publication-that-allows-queued-updating-subscriptions"></a>Pour modifier la stratégie de conflit pour une publication qui autorise les abonnements avec mise à jour en file d'attente  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Affectez la valeur **conflict_policy** à **\@property** et spécifiez le mode de stratégie de conflit souhaité en choisissant parmi **pub wins**, **sub reinit** ou **sub wins** pour **\@value**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple crée une publication qui prend en charge les abonnements par extraction avec mise à jour immédiate et mise à jour en file d'attente.  
  
 [!code-sql[HowTo#sp_createtranupdatingpub](../../../relational-databases/replication/codesnippet/tsql/enable-updating-subscrip_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Définir des options de résolution des conflits de mise à jour en attente &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Réplication transactionnelle](../../../relational-databases/replication/transactional/transactional-replication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Create an Updatable Subscription to a Transactional Publication](create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Utiliser sqlcmd avec des variables de script](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  

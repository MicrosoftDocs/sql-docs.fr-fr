---
title: Suspendre une topologie de réplication (procédures stockées de réplication)
description: Découvrez comment utiliser les procédures stockées de réplication pour suspendre une topologie de réplication pour SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- administering replication, quiescing
- quiesce [SQL Server replication]
- transactional replication, backup and restore
ms.assetid: 7626d575-9994-47be-b772-5b6f1b7ef7ca
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: aa205a6281a9cd1bedd3218956a255403d875ef6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467410"
---
# <a name="quiesce-a-replication-topology-replication-transact-sql-programming"></a>Suspendre une topologie de réplication (programmation Transact-SQL de la réplication)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  *Suspendre* un système revient à interrompre toute activité sur les tables publiées de tous les nœuds et à vérifier que chaque nœud a reçu toutes les modifications des autres nœuds. Cette rubrique explique comment suspendre une topologie de réplication, requise pour plusieurs tâches d'administration et comment garantir qu'un nœud a reçu toutes les modifications d'autres nœuds.  
  
### <a name="to-quiesce-a-transactional-replication-topology-with-read-only-subscriptions"></a>Pour suspendre une topologie de réplication transactionnelle avec les abonnements en lecture seule  
  
1.  Arrêtez l'activité sur toutes les tables publiées sur le serveur de publication.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_posttracertoken &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md).  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helptracertokenhistory](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md).  
  
4.  Assurez-vous que chaque Abonné a reçu le jeton de suivi.  

### <a name="to-quiesce-a-transactional-replication-topology-with-updatable-subscriptions"></a>Pour suspendre une topologie de réplication transactionnelle avec les abonnements pouvant être mis à jour  
  
1.  Arrêtez l'activité sur toutes les tables publiées sur le serveur de publication et sur tous les Abonnés.  
  
2.  Si tous les Abonnés utilisent des abonnements de mise à jour en attente :  
  
    1.  Si l'Agent de lecture de la file d'attente ne s'exécute pas en mode continu, exécutez l'Agent. Pour plus d’informations sur l’exécution des agents, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
    2.  Pour vérifier que la file d'attente est vide, exécutez [sp_replqueuemonitor](../../../relational-databases/system-stored-procedures/sp-replqueuemonitor-transact-sql.md) sur chaque Abonné.  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_posttracertoken](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md).  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helptracertokenhistory](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md).  
  
5.  Assurez-vous que chaque Abonné a reçu le jeton de suivi.  
  
### <a name="to-quiesce-a-peer-to-peer-transactional-replication-topology"></a>Pour suspendre une topologie de réplication transactionnelle d'égal à égal  
  
1.  Arrêtez l'activité sur toutes les tables publiées sur tous les nœuds.  
  
2.  Exécutez [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) sur chaque base de données de publication dans la topologie.  
  
3.  Si l'Agent de lecture du journal ou l'Agent de distribution ne s'exécute pas en mode continu, exécutez l'Agent. L'Agent de lecture du journal doit être démarré avant l'Agent de distribution. Pour plus d’informations sur l’exécution des agents, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
4.  Exécutez [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md) sur chaque base de données de publication dans la topologie. Vérifiez que le jeu de résultats contient des réponses de chacun des autres nœuds.  
  
### <a name="to-ensure-a-peer-to-peer-node-has-received-all-prior-changes"></a>Pour garantir qu'un nœud d'égal à égal a reçu toutes les modifications antérieures  
  
1.  Exécutez [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) sur la base de données de publication au nœud que vous contrôlez.  
  
2.  Si l'Agent de lecture du journal ou l'Agent de distribution ne s'exécute pas en mode continu, exécutez l'Agent. L'Agent de lecture du journal doit être démarré avant l'Agent de distribution. Pour plus d’informations sur l’exécution des agents, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
3.  Exécutez [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md) sur la base de données de publication au nœud que vous contrôlez. Vérifiez que le jeu de résultats contient des réponses de chacun des autres nœuds.  
  
### <a name="to-quiesce-a-merge-replication-topology"></a>Pour suspendre une topologie de réplication de fusion  
  
1.  Arrêtez l'activité sur toutes les tables publiées sur le serveur de publication et sur tous les Abonnés.  
  
2.  Exécutez l'Agent de fusion pour chaque abonnement deux fois : synchronisez tous les abonnements une fois, puis synchronisez chaque abonnement une deuxième fois. Cela garantit que toutes les modifications sont répliquées sur tous les nœuds. Pour plus d’informations sur l’exécution des agents, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
    > [!NOTE]  
    >  Si les conflits se produisent pendant la synchronisation, il est possible que les modifications requises par la résolution de conflit ne soient pas propagées à tous les nœuds après que l'Agent de fusion a été exécuté deux fois.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer une topologie d’égal à égal &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)   
 [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
  

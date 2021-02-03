---
description: MSSQL_ENG020554
title: MSSQL_ENG020554 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG020554 error
ms.assetid: ef1a1b88-b2ab-43e8-99cd-163a973262d6
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1188e7a173b67bbc926e652124b62f1cff2e2222
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205608"
---
# <a name="mssql_eng020554"></a>MSSQL_ENG020554
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|20554|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|L'agent de réplication n'a enregistré aucun message d'état d'avancement en %ld minutes. Il se peut que l'agent ne réponde pas ou que l'activité du système soit élevée. Vérifiez que les enregistrements sont répliqués vers la destination et que les connexions à l'Abonné, au serveur de publication et au serveur de distribution sont toujours actives.|  
  
## <a name="explanation"></a>Explication  
 Le travail **Vérification des agents de réplication** s'exécute à des intervalles spécifiés (10 minutes par défaut) pour vérifier l'état de chaque agent de réplication. Si un agent n'a consigné aucun message d'avancement depuis la dernière fois où le travail de vérification des agents s'est exécuté, une erreur MSSQL_ENG020554 peut être signalée. L'agent est supposé consigner au moins des messages d'historique, même si aucune autre activité de réplication ne se produit. Même si l'agent de réplication ne répond pas comme attendu, il n'est pas nécessairement arrêté ou en échec (si un agent a échoué, l'erreur MSSQL_ENG020536 est normalement signalée).  
  
 Les problèmes suivants peuvent provoquer l'apparition de l'erreur MSSQL_ENG020554 :  
  
-   L’Agent est occupé.  
  
     Si l'agent est trop occupé pour répondre quand il est interrogé par le travail de vérification des agents, ce travail ne peut pas rapporter si l'agent de réplication fonctionne correctement. Il y a plusieurs raisons pour lesquelles l'agent de réplication peut être occupé : il peut y avoir un grande quantité de données en cours de réplication, ou il peut y avoir des problèmes de conception ou de configuration de l'application qui aboutissent à des processus très longs à s'exécuter.  
  
-   L'agent ne peut pas se connecter à un des ordinateurs de la topologie.  
  
     Tous les agents comportent un paramètre **-LoginTimeOut** (défini sur 15 secondes par défaut), qui détermine la durée pendant laquelle un agent tente de se connecter à un nœud de réplication, par exemple l’Agent de fusion qui se connecte au serveur de publication. Si **-LoginTimeOut** est défini sur une valeur supérieure à celle de l’intervalle auquel le travail de vérification des agents de réplication s’exécute, un problème de connexion peut être la cause principale de l'erreur : l’erreur MSSQL_ENG020554 est alors générée avant que l’agent puisse signaler une erreur plus spécifique.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 L'action requise dépend de la cause de l'erreur :  
  
-   Dans tous les cas où cette erreur est signalée :  
  
     Vérifiez les détails de l'erreur dans le moniteur de réplication, puis redémarrez l'agent s'il s'est arrêté. Les détails de l'erreur devraient fournir des informations supplémentaires sur la raison pour laquelle l'agent ne fonctionnait pas correctement. Si l'agent ne fonctionne pas, n'arrêtez pas et ne redémarrez pas l'agent car cela peut aggraver le problème. Pour obtenir des informations sur l’affichage de l’état de l’agent et des détails de l’erreur dans le moniteur de réplication, consultez [Afficher des informations et effectuer des tâches avec le moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).    
  
-   Si l'erreur est signalée fréquemment parce que l'agent est occupé :  
  
     Il peut être nécessaire de reconcevoir l'application de façon à ce que les traitements effectués par l'agent soient moins longs.  
  
     Vous pouvez augmenter l'intervalle auquel l'état de l'agent est vérifié à l'aide de la boîte de dialogue **Propriétés du travail** . Pour obtenir des informations sur l’accès à cette boîte de dialogue pour les tâches de réplication, consultez [Afficher des informations et effectuer des tâches avec le moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Si un agent ne peut pas se connecter à un des ordinateurs de la topologie :  
  
     Il est recommandé de définir **-LoginTimeOut** sur une valeur inférieure à celle de l’intervalle auquel le travail de vérification des agents de réplication s’exécute. Dans certains cas, **-LoginTimeOut** est défini sur une valeur supérieure en raison de problèmes réseau entraînant l’expiration du délai d’attente des connexions. Si **-LoginTimeOut** est défini sur une valeur inférieure, la réplication peut signaler des erreurs plus spécifiques, ce qui vous permet de résoudre des problèmes de connexion pouvant eux-mêmes découler de problèmes d’autorisations, réseau ou autres. Les paramètres des agents peuvent être spécifiés dans des profils d'agent et sur la ligne de commande. Pour plus d'informations, consultez les pages suivantes :  
  
    -   [Utiliser des profils d’agent de réplication](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Administration de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agent de lecture de la file d'attente de réplication](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  

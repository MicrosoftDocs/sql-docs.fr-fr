---
title: 'Résoudre les problèmes : Dépassement de RTO du groupe de disponibilité (SQL Server) | Microsoft Docs'
description: Découvrez comment résoudre les problèmes de basculement sur un groupe de disponibilité Always On lorsque le basculement prend plus de temps que votre objectif de délai de récupération dans SQL Server.
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: e83e4ef8-92f0-406f-bd0b-dc48dc210517
author: rothja
ms.author: jroth
ms.openlocfilehash: 5847162edb0d962826931624f4d14a3d78468066
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641875"
---
# <a name="troubleshoot-availability-group-exceeded-rto"></a>Résoudre les problèmes : Dépassement de RTO du groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Après un basculement automatique ou un basculement manuel planifié sans perte de données sur un groupe de disponibilité, vous constaterez peut-être que le temps de basculement dépasse votre RTO (objectif de temps de récupération). Vous pouvez arriver au même constat quand vous estimez le temps de basculement d’un réplica secondaire avec validation synchrone (par exemple, un partenaire de basculement automatique) à l’aide de la méthode décrite dans [Monitorer les performances des groupes de disponibilité AlwaysOn](monitor-performance-for-always-on-availability-groups.md).  
  
 Si votre basculement automatique n’est pas encore terminé, consultez [Résolution des problèmes de basculement automatique dans les environnements SQL Server 2012 Always On](https://support.microsoft.com/kb/2833707).  
  
 Les sections suivantes décrivent les principales raisons qui expliquent pourquoi le temps de basculement dépasse le RTO.  
  
1.  [Une charge de travail de création de rapports empêche l’exécution du thread de restauration par progression](#BKMK_REDOBLOCK)  
  
2.  [Le thread de restauration par progression prend du retard en raison d’une contention de ressources](#BKMK_CONTENTION)  
  
##  <a name="reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a>Une charge de travail de création de rapports empêche l’exécution du thread de restauration par progression  
 Le thread de restauration par progression sur le réplica secondaire est bloqué et ne peut pas apporter de changements au DDL (langage de définition de données) au moyen d’une requête en lecture seule de longue durée.  
  
### <a name="explanation"></a>Explication  
 Sur le réplica secondaire, les requêtes en lecture seule acquièrent les verrous de stabilité de schéma (`Sch-S`). Ces verrous `Sch-S` peuvent bloquer le thread de restauration par progression et l’empêcher d’acquérir les verrous de modification de schéma (`Sch-M`) lui permettant d’apporter des changements au DDL. Vous devez d’abord débloquer un thread de restauration par progression bloqué pour qu’il puisse appliquer les enregistrements de journal. Une fois débloqué, il peut continuer à rattraper son retard jusqu’à la fin du journal et autoriser la poursuite des processus d’annulation et de basculement consécutifs.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Quand le thread de restauration par progression est bloqué, un événement étendu appelé `sqlserver.lock_redo_blocked` est généré. Vous pouvez également interroger la DMV sys.dm_exec_request sur le réplica secondaire pour déterminer la session qui bloque le thread de restauration par progression et prendre des mesures correctives. La requête suivante retourne l’ID de session de la requête en lecture seule qui bloque le thread de restauration par progression.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Vous pouvez soit laisser la charge de travail de création de rapports se terminer, ce qui déclenche le déblocage du thread de restauration par progression, soit débloquer immédiatement le thread de restauration par progression en exécutant la commande [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) sur l’ID de session à l’origine du blocage.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_CONTENTION"></a> Le thread de restauration par progression prend du retard en raison d’une contention de ressources  
 Une importante charge de travail de création de rapports sur le réplica secondaire ralentit les performances du réplica secondaire, et le thread de restauration par progression prend du retard.  
  
### <a name="explanation"></a>Explication  
 Quand vous appliquez des enregistrements de journal sur le réplica secondaire, le thread de restauration par progression lit ces enregistrements à partir du disque de stockage des journaux. Il accède ensuite, pour chaque enregistrement de journal, aux pages de données pour appliquer l’enregistrement de journal. L’accès à la page peut être lié aux E/S (accès au disque physique) si la page ne figure pas déjà dans le pool de mémoires tampons. Si la charge de travail de création de rapports est liée aux E/S, elle est en concurrence avec le thread de restauration par progression pour les ressources d’E/S et risque de ralentir le thread de restauration par progression.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Vous pouvez utiliser la requête DMV suivante pour voir le retard subi par le thread de restauration par progression en mesurant l’écart entre `last_redone_lsn` et `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Si le thread de restauration par progression prend effectivement du retard, vous devez rechercher la cause racine de la dégradation des performances sur le réplica secondaire. En cas de contention au niveau des E/S avec la charge de travail de création de rapports, vous pouvez utiliser [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) pour contrôler les cycles d’UC utilisés par la charge de travail de création de rapports afin de contrôler indirectement, dans une certaine mesure, les cycles d’E/S entrepris. Par exemple, si votre charge de travail de création de rapports utilise 10 % de l’UC et que la charge de travail est liée aux E/S, vous pouvez utiliser Resource Governor pour limiter l’utilisation des ressources de l’UC à 5 % afin de restreindre la charge de travail de lecture et réduire l’impact sur les E/S.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Résolution des problèmes de performances dans SQL Server (s’applique à SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  

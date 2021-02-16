---
title: Les modifications ne sont pas visibles sur le réplica de groupe de disponibilité secondaire
description: Découvrez comment déterminer pourquoi les changements sur un réplica principal ne sont pas répercutés sur le réplica secondaire dans un groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: c602fd39-db93-4717-8f3a-5a98b940f9cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 48b31c951cc74f1dc848cb5705b0fd3d756291db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337890"
---
# <a name="determine-why-changes-from-primary-replica-are-not-reflected-on-secondary-replica-for-an-always-on-availability-group"></a>Déterminer pourquoi les changements apportés au réplica principal ne sont pas répercutés sur le réplica secondaire dans un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  L’application cliente mène à bien une mise à jour sur le réplica principal, mais l’exécution d’une requête sur le réplica secondaire montre que le changement n’a pas été répercuté. Ce cas part du principe que l’état de synchronisation de votre disponibilité est sain. La plupart du temps, ce problème se résout tout seul en quelques minutes.  
  
 Si les changements ne sont toujours pas répercutés sur le réplica secondaire après quelques minutes, le flux de synchronisation peut faire l’objet d’un goulot d’étranglement. L’emplacement du goulot d’étranglement varie selon le type de validation (synchrone ou asynchrone) défini sur le réplica secondaire.  
  
 **Validation synchrone**  
  
 Chaque mise à jour effectuée sur le réplica principal a déjà été synchronisée avec le réplica secondaire, ou les enregistrements de journal ont déjà été vidés à des fins de renforcement sur le réplica secondaire. Le goulot d’étranglement doit donc se situer dans la phase de restauration par progression après le vidage du journal sur le réplica secondaire.  
  
 Toutefois, une fois la restauration par progression à niveau, toutes les charges de travail de lecture sur le réplica secondaire sont des requêtes d’isolement de capture instantanée :  
  
  -   Transactions de longue durée sur le réplica principal  
  
  -   Restauration par progression sur le réplica secondaire  


**Validation asynchrone**  
 
 Dans la mesure où une validation asynchrone accuse réception d’une transaction dès son vidage sur le disque local, le goulot d’étranglement peut se situer n’importe où après ce point :  
 
  -   Transactions de longue durée sur le réplica principal  
  
  -   Latence ou débit du réseau  
  
  -   Renforcement du journal sur le réplica secondaire  
  
  -   Restauration par progression sur le réplica secondaire  


Les sections suivantes décrivent les causes courantes de la non-répercussion des changements apportés au réplica principal sur le réplica secondaire pour les requêtes en lecture seule.  


##  <a name="long-running-active-transactions"></a><a name="BKMK_OLDTRANS"></a> Transactions actives de longue durée  
 Une transaction de longue durée sur le réplica principal empêche la lecture des mises à jour sur le réplica secondaire.  
  
### <a name="explanation"></a>Explication  
 Toutes les charges de travail de lecture sur le réplica secondaire sont des requêtes d’isolement de capture instantanée. Dans l’isolement de capture instantanée, les clients en lecture seule voient la base de données de disponibilité sur le réplica secondaire au point de début de la transaction active la plus ancienne dans le journal restauré par progression. Si une transaction n’est pas validée pendant plusieurs heures, la transaction ouverte empêche toutes les requêtes en lecture seule de voir les nouvelles mises à jour.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Sur le réplica principal, utilisez [DBCC OPENTRAN &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-opentran-transact-sql.md) pour afficher les transactions actives les plus anciennes et voir si elles peuvent être restaurées. Une fois les transactions actives les plus anciennes restaurées et synchronisées avec le réplica secondaire, les charges de travail de lecture sur le réplica secondaire peuvent voir les mises à jour dans la base de données de disponibilité jusqu’au début de la transaction active la plus ancienne à ce moment-là.  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Une latence réseau élevée ou un débit réseau faible provoque l’accumulation des journaux sur le réplica principal  
 Une latence réseau élevée ou un débit faible peut empêcher l’envoi des journaux au réplica secondaire dans les temps.  
  
### <a name="explanation"></a>Explication  
 Le réplica principal active le contrôle de flux sur l’envoi de journaux en cas de dépassement du nombre maximal autorisé de messages n’ayant pas fait l’objet d’un accusé de réception envoyés au réplica secondaire. Tant qu’une partie de ces messages ne fait pas l’objet d’un accusé de réception, aucun bloc de journal supplémentaire ne peut être envoyé au réplica secondaire. Cette situation peut avoir des conséquences plus graves, notamment la perte de données, compromettant ainsi votre objectif de point de récupération (RPO).  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Une valeur DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) élevée peut indiquer que les journaux sont retenus au niveau du réplica principal. Divisez cette valeur par [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) pour obtenir une estimation approximative de l’heure à laquelle les données seront à niveau sur le réplica secondaire.  
  
 En outre, il est utile de vérifier les objets de performance [SQL Server:Availability Replica > Flow Control Time (ms/sec)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) et [SQL Server:Availability Replica > Flow control/sec](~/relational-databases/performance-monitor/sql-server-availability-replica.md). Multipliez ces deux valeurs pour savoir le temps passé à attendre l’aboutissement du contrôle de flux au cours de la dernière seconde. Plus le temps d’attente du contrôle de flux est long, plus le taux d’envoi est faible.  
  
 Voici une liste des métriques utiles pour diagnostiquer les problèmes liés à la latence et au débit du réseau. Vous pouvez utiliser d’autres outils Windows comme **ping.exe** pour évaluer l’utilisation du réseau.  
  
-   DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   DMV [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   Compteur de performances `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Compteur de performances `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Compteur de performances `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Compteur de performances `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Resent Messages/sec`  
  
 Pour remédier à ce problème, essayez d’augmenter votre bande passante réseau ou de supprimer/réduire le trafic réseau inutile.  
  
##  <a name="another-reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Une autre charge de travail de création de rapports empêche l’exécution du thread de restauration par progression  
 Le thread de restauration par progression sur le réplica secondaire est bloqué et ne peut pas apporter de changements au DDL (langage de définition de données) au moyen d’une requête en lecture seule de longue durée. Vous devez débloquer le thread de restauration par progression pour qu’il puisse mettre à disposition de la charge de travail de lecture d’autres mises à jour.  
  
### <a name="explanation"></a>Explication  
 Sur le réplica secondaire, les requêtes en lecture seule acquièrent les verrous de stabilité de schéma (`Sch-S`). Ces verrous `Sch-S` peuvent bloquer le thread de restauration par progression et l’empêcher d’acquérir les verrous de modification de schéma (`Sch-M`) lui permettant d’apporter des changements au DDL. Vous devez d’abord débloquer un thread de restauration par progression bloqué pour qu’il puisse appliquer les enregistrements de journal.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Quand le thread de restauration par progression est bloqué, un événement étendu appelé `sqlserver.lock_redo_blocked` est généré. Vous pouvez également interroger la DMV sys.dm_exec_request sur le réplica secondaire pour déterminer la session qui bloque le thread de restauration par progression et prendre des mesures correctives. La requête suivante retourne l’ID de session de la charge de travail de création de rapports qui bloque le thread de restauration par progression.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Vous pouvez soit laisser la charge de travail de création de rapports se terminer, ce qui déclenche le déblocage du thread de restauration par progression, soit débloquer immédiatement le thread de restauration par progression en exécutant la commande [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) sur l’ID de session à l’origine du blocage.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_REDOBEHIND"></a> Le thread de restauration par progression prend du retard en raison d’une contention de ressources  
 Une importante charge de travail de création de rapports sur le réplica secondaire ralentit les performances du réplica secondaire, et le thread de restauration par progression prend du retard.  
  
### <a name="explanation"></a>Explication  
 Quand vous appliquez des enregistrements de journal sur le réplica secondaire, le thread de restauration par progression lit ces enregistrements à partir du disque de stockage des journaux. Il accède ensuite, pour chaque enregistrement de journal, aux pages de données pour appliquer l’enregistrement de journal. L’accès à la page peut être lié aux E/S (accès au disque physique) si la page ne figure pas déjà dans le pool de mémoires tampons. Si la charge de travail de création de rapports est liée aux E/S, elle est en concurrence avec le thread de restauration par progression pour les ressources d’E/S et risque de ralentir le thread de restauration par progression. Cette situation empêche les autres charges de travail de création de rapports de voir des données mises à jour et affecte le RTO.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Vous pouvez utiliser la requête DMV suivante pour voir le retard subi par le thread de restauration par progression en mesurant l’écart entre `last_redone_lsn` et `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Si le thread de restauration par progression prend effectivement du retard, vous devez rechercher la cause racine de la dégradation des performances sur le réplica secondaire. En cas de contention au niveau des E/S avec la charge de travail de création de rapports, vous pouvez utiliser [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) pour contrôler les cycles d’UC utilisés par la charge de travail de création de rapports afin de contrôler indirectement, dans une certaine mesure, les cycles d’E/S entrepris. Par exemple, si votre charge de travail de création de rapports utilise 10 % de l’UC et que la charge de travail est liée aux E/S, vous pouvez utiliser Resource Governor pour limiter l’utilisation des ressources de l’UC à 5 % afin de restreindre la charge de travail de lecture et réduire l’impact sur les E/S.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Résolution des problèmes de performances dans SQL Server 2008](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))
  

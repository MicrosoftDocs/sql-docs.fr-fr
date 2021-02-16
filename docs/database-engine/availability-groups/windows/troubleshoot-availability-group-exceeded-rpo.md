---
title: Dépassement de RPO du groupe de disponibilité
description: Problèmes courants et méthodes de résolution quand votre groupe de disponibilité Always On dépasse l’objectif de point de récupération (RPO)
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 38de1841-9c99-435a-998d-df81c7ca0f1e
author: rothja
ms.author: jroth
ms.openlocfilehash: f70696592cae8f486e7ff7b52e417bb67edfa8d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337922"
---
# <a name="troubleshoot-availability-group-exceeded-rpo"></a>Résoudre les problèmes : Dépassement de RPO du groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Après avoir effectué un basculement manuel forcé sur un groupe de disponibilité vers un réplica secondaire avec validation asynchrone, vous pouvez constater que la perte de données est supérieure à votre RPO (objectif de point de récupération). Vous pouvez arriver au même constat quand vous calculez la perte de données potentielle d’un réplica secondaire avec validation asynchrone à l’aide de la méthode décrite dans [Monitorer les performances des groupes de disponibilité AlwaysOn](monitor-performance-for-always-on-availability-groups.md).  
  
 Un réplica secondaire avec validation synchrone élimine les pertes de données, mais le risque de perte de données potentielle d’un réplica secondaire avec validation asynchrone varie selon la part du journal encore en attente de renforcement sur le réplica secondaire.  
  
 Les sections suivantes décrivent les causes courantes du risque élevé de perte de données d’un réplica secondaire avec validation asynchrone, si tant est que votre instance de serveur ne fait pas l’objet d’une dégradation des performances systémiques sans rapport avec les groupes de disponibilité.  
  
1.  [Une latence réseau élevée ou un débit réseau faible provoque l’accumulation des journaux sur le réplica principal](#BKMK_LATENCY)  
  
2.  [Un goulot d’étranglement des E/S disque ralentit le renforcement du journal sur le réplica secondaire](#BKMK_IO_BOTTLENECK)  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Une latence réseau élevée ou un débit réseau faible provoque l’accumulation des journaux sur le réplica principal  
 Le plus souvent, les bases de données dépassent le RPO car elles ne peuvent pas être envoyées au réplica secondaire suffisamment vite.  
  
### <a name="explanation"></a>Explication  
 Le réplica principal active le contrôle de flux sur l’envoi de journaux en cas de dépassement du nombre maximal autorisé de messages n’ayant pas fait l’objet d’un accusé de réception envoyés au réplica secondaire. Tant qu’une partie de ces messages ne fait pas l’objet d’un accusé de réception, aucun bloc de journal supplémentaire ne peut être envoyé au réplica secondaire. Étant donné que la perte de données ne peut être évitée que si les données ont été renforcées sur le réplica secondaire, l’accumulation de messages de journal non envoyés augmente le risque de perte de données.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Un grand nombre de messages renvoyés au réplica secondaire peut indiquer un niveau élevé de latence et de bruit réseau. Vous pouvez également comparer la valeur DMV **log_send_rate** avec l’objet de performance Log Bytes Flushed/sec. Si les journaux sont vidés sur le disque plus rapidement qu’ils ne sont envoyés, le risque de perte de données peut augmenter indéfiniment.  
  
 Il est également utile de vérifier les deux objets de performance suivants : `SQL Server:Availability Replica > Flow Control Time (ms/sec)` et `SQL Server:Availability Replica > Flow Control/sec`. Multipliez ces deux valeurs pour savoir le temps passé à attendre l’aboutissement du contrôle de flux au cours de la dernière seconde. Plus le temps d’attente du contrôle de flux est long, plus le taux d’envoi est faible.  
  
 Les métriques suivantes sont utiles pour diagnostiquer les problèmes liés à la latence et au débit du réseau. Vous pouvez utiliser d’autres outils Windows comme **ping.exe** et [Moniteur réseau](https://www.microsoft.com/p/network-monitor-pro-free-edition/9n8gdvj32gp7) pour évaluer la latence et l’utilisation du réseau.  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_queue_size`  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_rate`  
  
-   Compteur de performances `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Compteur de performances `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Compteur de performances `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Compteur de performances `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Compteur de performances `SQL Server:Availability Replica > Resent Messages/sec`  

Pour remédier à ce problème, essayez d’augmenter votre bande passante réseau ou de supprimer/réduire le trafic réseau inutile.  


##  <a name="disk-io-bottleneck-slows-down-log-hardening-on-the-secondary-replica"></a><a name="BKMK_IO_BOTTLENECK"></a> Un goulot d’étranglement des E/S disque ralentit le renforcement du journal sur le réplica secondaire  
 Selon le déploiement du fichier de base de données, le renforcement du journal peut ralentir en raison d’une contention au niveau des E/S avec la charge de travail de création de rapports.  
  
### <a name="explanation"></a>Explication  
 Le risque de perte de données est éliminé dès que le bloc de journal est renforcé sur le fichier journal. Il est donc impératif d’isoler le fichier journal du fichier de données. Si le fichier journal et le fichier de données sont mappés au même disque dur, la charge de travail de création de rapports caractérisée par des lectures intensives sur le fichier de données consomme les mêmes ressources d’E/S que l’opération de renforcement du journal. Si cette dernière est lente, l’envoi des accusés de réception au réplica principal peut ralentir, ce qui peut entraîner une activation excessive du contrôle de flux et de longs délais d’attente au niveau du contrôle flux.  
  
### <a name="diagnosis-and-resolution"></a>Diagnostic et résolution  
 Si vous avez déterminé que le réseau n’est pas affecté par une latence élevée ou un débit faible, examinez le réplica secondaire à la recherche de contentions au niveau des E/S. Les requêtes de [SQL Server : Minimiser les E/S disque](/previous-versions/technet-magazine/jj643251(v=msdn.10)) sont utiles pour identifier les contentions. Des exemples dérivés de cet article sont proposés ci-dessous à toutes fins utiles.  
  
 Le script suivant vous permet de voir le nombre de lectures et d’écritures sur chaque fichier de données et de journal pour chaque base de données de disponibilité en cours d’exécution sur une instance de SQL Server. Les résultats sont triés selon le temps moyen de blocage des E/S, en millisecondes. Notez qu’il s’agit d’un cumul des valeurs générées depuis le dernier démarrage de l’instance du serveur. Vous devez donc calculer la différence entre deux mesures après un certain temps.  
  
```sql  
SELECT DB_NAME(database_id) AS   
   [Database Name] ,   
   file_id ,   
   io_stall_read_ms ,   
   num_of_reads ,   
   CAST(io_stall_read_ms / ( 1.0 + num_of_reads ) AS NUMERIC(10, 1)) AS [avg_read_stall_ms] ,   
   io_stall_write_ms ,   
   num_of_writes ,  
   CAST(io_stall_write_ms / ( 1.0 + num_of_writes ) AS NUMERIC(10, 1)) AS [avg_write_stall_ms] ,   
   io_stall_read_ms + io_stall_write_ms AS [io_stalls] ,   
   num_of_reads + num_of_writes AS [total_io] ,   
   CAST(( io_stall_read_ms + io_stall_write_ms ) / ( 1.0 + num_of_reads  
+ num_of_writes) AS NUMERIC(10,1)) AS [avg_io_stall_ms]  
FROM sys.dm_io_virtual_file_stats(NULL, NULL)  
WHERE DB_NAME(database_id) IN (SELECT DISTINCT database_name FROM sys.dm_hadr_database_replica_cluster_states)  
ORDER BY avg_io_stall_ms DESC;  
```  
  
 La requête suivante fournit un instantané à un moment donné (non cumulé) de requêtes d’E/S en attente sur votre système.  
  
```sql  
SELECT DB_NAME(mf.database_id) AS [Database] ,   
   mf.physical_name ,  
   r.io_pending ,   
   r.io_pending_ms_ticks ,   
   r.io_type ,   
   fs.num_of_reads ,   
   fs.num_of_writes  
FROM sys.dm_io_pending_io_requests AS r   
INNER JOIN sys.dm_io_virtual_file_stats(NULL, NULL) AS fs ON r.io_handle = fs.file_handle   
INNER JOIN sys.master_files AS mf ON fs.database_id = mf.database_id  
AND fs.file_id = mf.file_id  
ORDER BY r.io_pending , r.io_pending_ms_ticks DESC;  
```  
  
 Vous pouvez comparer la façon dont les E/S de lecture et les E/S d’écriture correspondent les une aux autres pour identifier toute contention au niveau des E/S.  
  
 Voici quelques compteurs de performances supplémentaires qui peuvent vous aider à diagnostiquer les goulots d’étranglement au niveau des E/S :  
  
-   **Disque physique : Tous les compteurs**  
  
-   **Disque physique : Avg. disque s/transfert**  
  
-   **SQL Server : Bases de données > Temps d’attente de vidage du journal**  
  
-   **SQL Server : Bases de données > Attentes de vidages du journal/s**  
  
-   **SQL Server : Bases de données > Journaliser les lectures du disque/s du pool**  
  
 Si vous identifiez un goulot d’étranglement au niveau des E/S et que vous avez placé le fichier journal et le fichier de données sur le même disque dur, la première chose à faire consiste à placer le fichier de données et le fichier journal sur des disques distincts. Cette pratique empêche la charge de travail de création de rapports de perturber le chemin de transfert du réplica principal au tampon journal et sa capacité à renforcer la transaction sur le réplica secondaire.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Résolution des problèmes de performances dans SQL Server (s’applique à SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  

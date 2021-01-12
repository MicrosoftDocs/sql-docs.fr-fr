---
description: MSreplication_monitordata (Transact-SQL)
title: MSreplication_monitordata (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
author: cawrites
ms.author: chadam
ms.openlocfilehash: bbe408296895f593de0af1da852b807082e8e7cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098159"
---
# <a name="msreplication_monitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSreplication_monitordata** contient les données mises en cache utilisées par le moniteur de réplication, avec une ligne pour chaque abonnement analysé. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|Date et heure auxquelles les données du moniteur ont été actualisées.|  
|**computetime**|**int**|Durée, en secondes, du calcul des données du moniteur.|  
|**publication_id**|**int**|ID de publication.|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_srvid**|**int**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication, que peut prendre l'une des valeurs suivantes :<br /><br /> **0** = publication transactionnelle<br /><br /> **1** = publication d’instantané<br /><br /> **2** = publication de fusion|  
|**agent_type**|**int**|Type d'Agent de réplication, que peut prendre l'une des valeurs suivantes :<br /><br /> **1** = agent d’instantané<br /><br /> **2** = agent de lecture du journal<br /><br /> **3** = agent de distribution<br /><br /> **4** = agent de fusion<br /><br /> **9** = agent de lecture de la file d’attente|  
|**agent_id**|**int**|ID de l'Agent de réplication.|  
|**agent_name**|**sysname**|Nom du travail d'Agent de réplication.|  
|**job_id**|**uniqueidentifier**|GUID du travail d'Agent de réplication.|  
|**statut**|**int**|État de l'Agent de réplication, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = démarré<br /><br /> **2** = réussite<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = nouvelle tentative<br /><br /> **6** = échec|  
|**isagentrunningnow**|**bit**|Indicateur qui indique si le travail de l’agent est en cours d’exécution, où la valeur **1** signifie que le travail est en cours d’exécution.|  
|**warning**|**int**|Avertissement de seuil généré par un abonnement, qui peut être le résultat OR logique d'au moins l'une des valeurs suivantes.<br /><br /> **1** = expiration : un abonnement à une publication transactionnelle a dépassé la période de rétention au-delà du seuil autorisé, en pourcentage de la période de rétention.<br /><br /> **2** = latence : le temps nécessaire à la réplication des données d’un serveur de publication transactionnel vers l’abonné dépasse le seuil, en secondes.<br /><br /> **4** = mergeexpiration : un abonnement à une publication de fusion a dépassé la période de rétention au-delà du seuil autorisé, en pourcentage de la période de rétention. 8 = durée d'exécution rapide de la fusion ; la durée de la réalisation de la synchronisation d'un abonnement de fusion dépasse le seuil, en secondes, via une connexion réseau rapide.<br /><br /> **16** = mergeslowrunduration-le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, sur une connexion réseau lente ou d’accès à distance.<br /><br /> **32** = mergefastrunspeed-la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas réussi à maintenir le taux de seuil, en lignes par seconde, sur une connexion réseau rapide.<br /><br /> **64** = mergeslowrunspeed-la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas réussi à maintenir le taux de seuil, en lignes par seconde, sur une connexion réseau lente ou d’accès à distance.|  
|**last_distsync**|**datetime**|Date et heure de la dernière exécution de la Agent de distribution.|  
|**agentstoptime**|**datetime**|Date et heure auxquelles l'Agent s'est arrêté.|  
|**distdb**|**sysname**|Nom de la base de données de distribution de l'abonnement.|  
|**fixation**|**int**|Période de rétention de la publication.|  
|**time_stamp**|**datetime**|À usage interne uniquement.|  
|**worst_latency**|**int**|Latence maximale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**best_latency**|**int**|Latence minimale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**avg_latency**|**int**|Latence moyenne, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**cur_latency**|**int**|Latence, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pendant l'exécution en cours.|  
|**worst_runspeedPerf**|**int**|L’heure de synchronisation la plus longue pour la publication de fusion|  
|**best_runspeedPerf**|**int**|Durée minimale de la synchronisation de la publication de fusion.|  
|**average_runspeedPerf**|**int**|Durée de synchronisation moyenne pour la publication de fusion|  
|**mergePerformance**|**int**|Performances de la dernière synchronisation comparées à toutes les synchronisations de l'abonnement, calculées en divisant la vitesse de transmission de la dernière synchronisation par la moyenne de toutes les vitesses de transmission antérieures.|  
|**mergelatestsessionrunduration**|**int**|Durée de l'exécution la plus récente de l'Agent de fusion.|  
|**mergelatestsessionrunspeed**|**float(53)**|Vitesse de transmission de l'exécution la plus récente de l'Agent de fusion.|  
|**mergelatestsessionconnectiontype**|**int**|Connexion utilisée pour la session la plus récente de l'Agent de fusion, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = réseau local (LAN)<br /><br /> **2** = connexion d’accès réseau à distance|  
|**retention_period_unit**|**tinyint**|Définit l'unité de rétention, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = semaine<br /><br /> **2** = mois<br /><br /> **3** = année|  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  

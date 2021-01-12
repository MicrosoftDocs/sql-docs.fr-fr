---
title: MSpeer_conflictdetectionconfigresponse (T-SQL)
description: Décrit la procédure stockée MSPeer_conflictdetectionconfigureresponse utilisée dans la réplication d’égal à égal pour stocker la réponse de chaque nœud à une réactivité de configuration à grande vue de la topologie.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_conflictdetectionconfigresponse
- MSpeer_conflictdetectionconfigresponse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_conflictdetectionconfigureresponse
ms.assetid: 2685fb66-731d-40f7-af4b-596b9222c5d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 02c82643c290e3b751ced451dd7415004272a2b0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097383"
---
# <a name="mspeer_conflictdetectionconfigresponse-transact-sql"></a>MSpeer_conflictdetectionconfigresponse (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de stocker la réponse de chaque nœud à une requête de configuration à l'échelle d'une topologie dans le cadre d'une réplication d'égal à égal. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|request_id|**int**|Identifie une entrée de demande de configuration de conflit dans la table [MSpeer_conflictdetectionconfigrequest](../../relational-databases/system-tables/mspeer-conflictdetectionconfigrequest-transact-sql.md) .|  
|peer_node|**sysname**|Nom de l'instance serveur qui a généré la réponse.|  
|peer_db|**sysname**|Base de données d’abonnement au niveau de l’homologue qui a généré la réponse.|  
|peer_version|**sysname**|Spécifie le numéro de version du serveur de publication.|  
|peer_db_version|**sysname**|Identifie le numéro de version de la base de données d'homologue.|  
|is_peer|**bit**|Indique si un nœud est un Abonné en lecture seule. La valeur **0** indique un abonné en lecture seule.|  
|conflict_detection_enabled|**bit**|Indique si la détection de conflit est activée pour la topologie.|  
|originator_id|**varbinary(16)**|Identifie chaque nœud dans la topologie pour les besoins de la détection de conflit. Pour plus d’informations, voir [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|peer_conflict_retention|**int**|Durée, en jours, du stockage des métadonnées dans les tables en conflit.|  
|peer_subscriptions|**XML**|Informations relatives au nœud ayant répondu à la demande.|  
|progress_phase|**nvarchar(32)**|Identifie la phase actuelle du traitement en utilisant l'une des valeurs suivantes :<br /><br /> Démarré<br /><br /> Version homologue collectée<br /><br /> État collecté|  
|modified_date|**datetime**|Date et heure d'achèvement d'une phase.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

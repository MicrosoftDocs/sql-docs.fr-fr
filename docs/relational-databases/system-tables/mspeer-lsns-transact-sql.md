---
description: MSpeer_lsns (Transact-SQL)
title: MSpeer_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpeer_lsns
- MSpeer_lsns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_lsns system table
ms.assetid: 0ba33907-601b-4c3d-8099-2663f680a161
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9a48b7681fb40f6c6e8758b54f61123326e397f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097358"
---
# <a name="mspeer_lsns-transact-sql"></a>MSpeer_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **Mspeer_lsns** permet de mapper chaque transaction à un abonnement dans une topologie de réplication d’égal à égal. Cette table est stockée dans chaque base de données de publication, dans une topologie de réplication d'égal à égal dans la base de données d'abonnement de tous les Abonnés à une publication d'égal à égal. Pour plus d’informations sur ce type de topologie de réplication transactionnelle, consultez [réplication transactionnelle d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md). Cette table est stockée dans la base de données de publication.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifie un numéro de séquence d'enregistrement d'égal à égal.|  
|**last_updated**|**datetime**|**Date et heure** de la dernière mise à jour de la ligne.|  
|**Emetteur**|**sysname**|Nom du serveur de publication à l'origine de la transaction|  
|**originator_db**|**sysname**|Nom de la base de données d'où provient la transaction|  
|**originator_publication**|**sysname**|Nom de la publication d'où provient la transaction|  
|**originator_publication_id**|**int**|Identificateur de la publication d'où provient la transaction|  
|**originator_db_version**|**int**|Identifie le numéro de version de la base de données d'origine.|  
|**originator_lsn**|**int**|Identifie le numéro de séquence d'enregistrement dans la publication d'origine.|  
|**originator_version**|**int**|Spécifie le numéro de version du serveur de publication.|  
|**originator_id**|**smallint**|Identifie chaque nœud dans la topologie pour les besoins de la détection de conflit. Pour plus d’informations, voir [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

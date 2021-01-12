---
description: MScached_peer_lsns (Transact-SQL)
title: MScached_peer_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MScached_peer_lsns_TSQL
- MScached_peer_lsns
dev_langs:
- TSQL
helpviewer_keywords:
- MScached_peer_lsns system table
ms.assetid: f8b6089a-0230-45f9-8c34-9fe0d2a3a74e
author: cawrites
ms.author: chadam
ms.openlocfilehash: f1b9c527f1411f84afeef72d60c266ef1f3a91ea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092419"
---
# <a name="mscached_peer_lsns-transact-sql"></a>MScached_peer_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MScached_peer_lsns** permet de suivre les valeurs LSN dans le journal des transactions utilisées pour déterminer les commandes à retourner à un abonné donné dans une réplication d’égal à égal. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID de l'Agent de distribution.|  
|**Emetteur**|**sysname**|Nom du serveur de publication d'origine.|  
|**originator_db**|**sysname**|Nom de la base de données de publication d'origine.|  
|**originator_publication_id**|**int**|Identifie la publication d'origine.|  
|**originator_db_version**|**int**|Identifie le numéro de version de la base de données d'origine.|  
|**originator_lsn**|**varbinary(16)**|Numéro de séquence d'enregistrement (LSN)  de la transaction d'origine.|  
  
## <a name="remarks"></a>Notes  
 Les valeurs LSN (numéros de séquence d'enregistrement) sont utilisées uniquement immédiatement après l'insertion. Elles n'ont pas de signification durable dans le système.  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

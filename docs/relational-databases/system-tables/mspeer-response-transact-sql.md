---
description: MSpeer_response (Transact-SQL)
title: MSpeer_response (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_response
- MSpeer_response_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_response system table
ms.assetid: 510e24cf-0292-47a9-b1d9-71a30fef030f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8008d0cd813666f009846defe2715159545e2893
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200263"
---
# <a name="mspeer_response-transact-sql"></a>MSpeer_response (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSpeer_response** est utilisée dans la réplication d’égal à égal pour stocker la réponse de chaque nœud à une demande d’état de publication. Cette table est stockée dans la base de données de publication.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**request_id**|**int**|Identifie une entrée de demande d’État dans la table [MSpeer_request](../../relational-databases/system-tables/mspeer-request-transact-sql.md) .|  
|**paires**|**sysname**|Homologue qui a généré la réponse.|  
|**peer_db**|**sysname**|Base de données d'abonnement sur l'homologue qui a généré la réponse.|  
|**received_date**|**datetime**|Date et heure de réception de la demande de l'homologue.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  

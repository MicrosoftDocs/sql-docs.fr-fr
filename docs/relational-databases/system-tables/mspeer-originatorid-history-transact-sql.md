---
description: MSpeer_originatorid_history (Transact-SQL)
title: MSpeer_originatorid_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_originatorid_history_TSQL
- MSpeer_originatorid_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_originatorid_history
ms.assetid: c1f53d0f-4080-43ff-be38-2b10395c68c9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0080e3af9f2072e4a86da350a80c633117550301
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200304"
---
# <a name="mspeer_originatorid_history-transact-sql"></a>MSpeer_originatorid_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque ID d'appelant défini dans la topologie. Ces informations incluent les ID des nœuds qui ne sont plus actifs. La table est utilisée lorsque vous configurez un nouveau nœud pour la détection de conflit afin de garantir que l'ID d'appelant spécifié n'a pas déjà été utilisé. Cette table est stockée dans la base de données de publication. Pour plus d’informations sur la détection des conflits, consultez [détection de conflit dans la réplication d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|originator_publication|**sysname**|Publication pour laquelle l'ID d'appelant a été spécifié.|  
|originator_id|**int**|Nombre qui identifie chaque nœud dans la topologie à des fins de détection de conflit|  
|originator_node|**sysname**|Instance de serveur pour laquelle l'ID d'appelant a été spécifié.|  
|originator_db|**sysname**|Base de données de publication pour laquelle l'ID d'appelant a été spécifié.|  
|originator_db_version|**int**|Identifie le numéro de version de la base de données d'origine.|  
|originator_version|**int**|Spécifie le numéro de version du serveur de publication.|  
|inserted_date|**datetime**|Date et heure d'insertion de la ligne de l'ID d'appelant dans cette table.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

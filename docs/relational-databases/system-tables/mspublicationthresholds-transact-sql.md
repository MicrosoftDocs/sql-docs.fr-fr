---
description: MSpublicationthresholds (Transact-SQL)
title: MSpublicationthresholds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- mspublicationthresholds
- mspublicationthresholds_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublicationthresholds system table
ms.assetid: 9da3879f-b1f4-4ab4-abd4-a9a8ac395eba
author: cawrites
ms.author: chadam
ms.openlocfilehash: ff905e4e8849c460ddd947978b89cd63cc2b7a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191171"
---
# <a name="mspublicationthresholds-transact-sql"></a>MSpublicationthresholds (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSpublicationthresholds** est utilisée pour suivre les métriques de performances de réplication d’une publication, avec une ligne pour chaque valeur de seuil surveillée. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publication_id**|**int**|Identifie la publication pour laquelle un seuil a été défini.|  
|**metric_id**|**int**|Identifie une mesure de performance de réplication surveillée telle qu’elle est définie dans la table système [MSreplmonthresholdmetrics](../../relational-databases/system-tables/msreplmonthresholdmetrics-transact-sql.md) .|  
|**value**|**sql_variant**|Valeur de seuil de la mesure surveillée.|  
|**shouldalert**|**bit**|La valeur **1** indique qu’une alerte doit être générée lorsque la mesure dépasse le seuil défini.|  
|**IsEnabled**|**bit**|La valeur **1** indique que l’analyse est activée pour cette mesure de performance de réplication.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

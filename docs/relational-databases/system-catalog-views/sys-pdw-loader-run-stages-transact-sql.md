---
description: sys.pdw_loader_run_stages (Transact-SQL)
title: sys.pdw_loader_run_stages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 255681e9-323c-42c0-a63c-1f05536efdd5
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: b0c0c6507a512e6eb02440735f4a7a9c41217ef8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206393"
---
# <a name="syspdw_loader_run_stages-transact-sql"></a>sys.pdw_loader_run_stages (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contient des informations sur les opérations de chargement en cours et terminées dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Ces informations sont conservées après le redémarrage du système.  
  
| Nom de la colonne | Type de données | Description | Plage |
| ----------- | --------- | ----------- | ----- |
|run_id|**int**|Identificateur unique d’une exécution de chargeur.||  
|étape|**nvarchar(30)**|Étape actuelle pour l’exécution.|'CREATE_STAGING', 'DMS_LOAD', 'LOAD_INSERT', 'LOAD_CLEANUP'|  
|request_id|**nvarchar(32)**|ID de la demande exécutant cette étape.||  
|status|**nvarchar (16)**|État de cette phase.||  
|start_time|**datetime**|Heure à laquelle l’étape a été démarrée.||  
|end_time|**datetime**|Heure de fin de l’étape, le cas échéant.|NULL s’il n’est pas démarré ou en cours.|  
|total_elapsed_time|**int**|Durée totale d’exécution de cette étape (ou passée jusqu’à présent).|Si total_elapsed_time dépasse la valeur maximale d’un entier (24,8 jours en millisecondes), cela entraînera un échec de matérialisation en raison d’un dépassement de capacité.<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

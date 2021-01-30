---
description: sys.workload_management_workload_classifiers (Transact-SQL)
title: sys.workload_management_workload_classifiers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 0b354af4a220adcaed93663fc899f80306e0a85a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190848"
---
# <a name="sysworkload_management_workload_classifiers-transact-sql"></a>sys.workload_management_workload_classifiers (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 Retourne les détails des classifieurs de charge de travail.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|ID unique du classifieur. N'accepte pas la valeur NULL||
group_name|**sysname**|Nom du groupe de charge de travail auquel le classifieur est affecté. N'accepte pas la valeur NULL. Joignable à sys.workload_management_workload_groups ||
name|**sysname**|Nom du classifieur. Doit être unique pour l’instance. N'accepte pas la valeur NULL.||
|importance|**sysname**|Est l’importance relative d’une demande dans ce groupe de charges de travail et entre les groupes de charges de travail pour les ressources partagées.  L’importance spécifiée dans le classifieur remplace le paramètre d’importance du groupe de charge de travail. Autorise la valeur NULL.  Si la valeur est null, le paramètre d’importance du groupe de charge de travail est utilisé.|Low, below_normal, normal (par défaut), above_normal, High |
|create_time|**datetime**|Heure de création du classifieur. N'accepte pas la valeur NULL.||
modify_time|**datetime**|Heure de la dernière modification du classifieur. N'accepte pas la valeur NULL.||
is_enabled|**bit**|INTERNAL||
|&nbsp;||||
  
## <a name="permissions"></a>Autorisations

Requiert l'autorisation VIEW SERVER STATE.

## <a name="next-steps"></a>Étapes suivantes

 Pour obtenir la liste de tous les affichages catalogue pour l’analyse Azure Synapse et les Data Warehouse parallèles, consultez [Azure Synapse Analytics et les affichages catalogue parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Pour créer un classifieur de charge de travail, consultez [créer un classifieur de charge de travail](../../t-sql/statements/create-workload-classifier-transact-sql.md). Pour plus d’informations sur la classification des charges de travail, consultez [classification des charges de travail](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)

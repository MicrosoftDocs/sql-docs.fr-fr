---
description: sys.workload_management_workload_groups (Transact-SQL)
title: sys.workload_management_workload_groups (Transact-SQL) | Microsoft Docs
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
ms.openlocfilehash: 6b081619b6aaa98efe4b3312acb3a44034a71c20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190148"
---
# <a name="sysworkload_management_workload_groups-transact-sql"></a>sys.workload_management_workload_groups (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 Retourne les détails des groupes de charge de travail.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|
|group_id|**int**|ID unique du groupe de charges de travail. N'accepte pas la valeur NULL.||
|name|**sysname**|Nom du groupe de charges de travail. Doit être unique pour l’instance.  N'accepte pas la valeur NULL.||
|importance|**nvarchar(128)**|Est l’importance relative d’une demande dans ce groupe de charges de travail et entre les groupes de charges de travail pour les ressources partagées. N'accepte pas la valeur NULL.|Low, below_normal, normal (par défaut), above_normal, High||
|min_percentage_resource|**tinyint**|Quantité de ressources garantie pour les demandes dans le groupe de charge de travail. Les ressources ne sont pas partagées avec d’autres groupes de charge de travail. N'accepte pas la valeur NULL.||
|cap_percentage_resource|**tinyint**|Limite inconditionnelle de l’allocation de pourcentage de ressources pour les demandes dans le groupe de charge de travail. Limite le nombre maximal de ressources allouées au niveau spécifié. La plage autorisée pour la valeur est comprise entre 1 et 100.||
|request_min_resource_grant_percent|**décimal (5, 2)**|Spécifie la quantité minimale de ressources allouées à une demande. La plage autorisée pour value est comprise entre 0,75 et 100.||
|request_max_resource_grant_percent |**décimal (5, 2)**|Spécifie la quantité maximale de ressources allouées à une demande.||
|query_execution_timeout_sec|**int**|Durée d’exécution, en secondes, autorisée avant l’annulation de la requête.  Les requêtes ne peuvent pas être annulées une fois qu’elles ont atteint la phase de retour de l’exécution.  query_execution_timeout_sec n’inclut pas le temps passé en file d’attente.|
|query_wait_timeout_sec|**int**|INTERNAL||
|create_time|**datetime**|Heure de création du groupe de charge de travail. N'accepte pas la valeur NULL.||
modify_time|**datetime**|Heure de la dernière modification du groupe de charge de travail. N'accepte pas la valeur NULL.||
|&nbsp;||||
  
## <a name="permissions"></a>Autorisations

Requiert l'autorisation VIEW SERVER STATE.

## <a name="next-steps"></a>Étapes suivantes

 Pour obtenir la liste de tous les affichages catalogue pour l’analyse Azure Synapse et les Data Warehouse parallèles, consultez [Azure Synapse Analytics et les affichages catalogue parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Pour créer un groupe de charge de travail, consultez [créer un groupe de charge de travail](../../t-sql/statements/create-workload-group-transact-sql.md). Pour plus d’informations sur la classification de la charge de travail, consultez isolation de la [charge de travail](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation)

---
description: sys.dm_cluster_endpoints (Transact-SQL)
title: sys.dm_cluster_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_cluster_endpoints
- dm_cluster_endpoints_TSQL
- dm_cluster_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cluster_endpoints dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 9f9d78015c5413c7e0ec4e827240e7465969d189
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095249"
---
# <a name="sysdm_cluster_endpoints-transact-sql"></a>sys.dm_cluster_endpoints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|name|`sysname`|Nom du service exposé en externe dans un cluster SQL Big Data. Identificateur unique du point de terminaison. Clé pour cette vue. N'accepte pas la valeur NULL. |  
|description|`nvarchar(4000)`|Description du service. N'accepte pas la valeur NULL. |
|endpoint|`sysname`|URL de point de terminaison ou attribut de connexion. N'accepte pas la valeur NULL. |
|protocol_desc|`sysname`|Description du protocole de point de terminaison |

## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.

## <a name="see-also"></a>Voir aussi

[Que sont [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] ](../../big-data-cluster/big-data-cluster-overview.md)?

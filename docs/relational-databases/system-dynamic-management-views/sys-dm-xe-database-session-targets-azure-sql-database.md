---
description: sys.dm_xe_database_session_targets (Azure SQL Database)
title: sys.dm_xe_database_session_targets
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
ms.assetid: 7f353e2a-f8fc-4366-97e4-aa1c49eadaf4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: e3a11ca92581e879c1fc0fbc51fc02b630251fad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344296"
---
# <a name="sysdm_xe_database_session_targets-azure-sql-database"></a>sys.dm_xe_database_session_targets (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Renvoie des informations sur les cibles d’une session d’événements.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 et à toutes les futures versions.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Adresse mémoire de la session d'événements. A une relation plusieurs-à-un avec sys.dm_xe_database_sessions. Address. N'accepte pas la valeur NULL.|  
|target_name|**nvarchar(60)**|Nom de la cible au sein d'une session. N'accepte pas la valeur NULL.|  
|target_package_guid|**uniqueidentifier**|GUID du package qui contient la cible. N'accepte pas la valeur NULL.|  
|execution_count|**bigint**|Nombre d'exécutions de la cible pour la session. N'accepte pas la valeur NULL.|  
|execution_duration_ms|**bigint**|Temps d'exécution total de la cible, en millisecondes. N'accepte pas la valeur NULL.|  
|target_data|**nvarchar(max)**|Données gérées par la cible, par exemple les informations sur l'agrégation d'événements. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_database_session_targets sys.dm_xe_database_session_targets.event_session_address|sys.dm_xe_database_sessions. adresse|Plusieurs-à-un|  
  
  

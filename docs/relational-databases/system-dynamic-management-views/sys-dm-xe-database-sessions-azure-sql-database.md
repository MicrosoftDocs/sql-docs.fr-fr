---
description: sys.dm_xe_database_sessions (Azure SQL Database)
title: sys.dm_xe_database_sessions (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 33ea5179-16bb-4abd-96cc-9bc696e80987
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: d800d67e59bb72c6120ec786adaf5863a80bdf5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093942"
---
# <a name="sysdm_xe_database_sessions-azure-sql-database"></a>sys.dm_xe_database_sessions (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Retourne des informations sur les événements de la session. Les événements sont des points d'exécution discrets. Des prédicats peuvent être appliqués aux événements pour les empêcher de se déclencher si ces événements ne contiennent pas les informations requises.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 et à toutes les versions ultérieures.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Adresse mémoire de la session d'événements. N'accepte pas la valeur NULL.|  
|event_name|**nvarchar(60)**|Nom de l'événement auquel est liée une action. N'accepte pas la valeur NULL.|  
|event_package_guid|**uniqueidentifier**|GUID pour le package contenant l'événement. N'accepte pas la valeur NULL.|  
|event_predicate|**nvarchar(2048)**|Représentation XML de l'arborescence prédicat qui est appliquée à l'événement. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
À partir de 2015-07-13, 'sys.dm_xe_objects’est l’une de ces DMV XEvents qui ne contiennent pas' _database’dans leur nom. Il ne s’agit pas d’une faute de frappe ou d’erreur dans la colonne de droite du tableau suivant. Le nom est le même dans Microsoft SQL Server et Azure SQL Database.  
  
|Du|À|Relationship|  
|--------|------|----------------|  
|sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_session_address|sys.dm_xe_database_sessions. adresse|Plusieurs-à-un|  
|sys. DM _xe_database_session_events. event_package_guid, sys. DM _xe_database_session_events. event_name|sys.dm_xe_objects.name, sys.dm_xe_objects.package_guid|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
[Événements étendus dans une base de données SQL Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
[Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  

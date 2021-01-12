---
description: sys.dm_geo_replication_link_status (Azure SQL Database)
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092799"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contient une ligne pour chaque lien de réplication entre les bases de données primaires et secondaires dans un partenariat de géo-réplication. Cela inclut les bases de données primaires et secondaires. S’il existe plusieurs liens de réplication continus pour une base de données primaire donnée, cette table contient une ligne pour chacune des relations. La vue est créée dans toutes les bases de données, y compris la logique principale. Cependant, l'interrogation de cette vue dans la base de données master logique retourne un ensemble vide.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|ID unique du lien de réplication.|  
|partner_server|**sysname**|Nom du serveur de SQL Database qui contient la base de données liée.|  
|partner_database|**sysname**|Nom de la base de données liée sur le serveur SQL Database lié.|  
|last_replication|**datetimeoffset**|Horodateur de l’accusé de réception de la dernière transaction par la base de données secondaire en fonction de l’horloge de la base de données primaire. Cette valeur est disponible uniquement sur la base de données primaire.|  
|replication_lag_sec|**int**|Différence de temps en secondes entre la valeur de last_replication et l’horodateur de la validation de cette transaction sur la base de données primaire en fonction de l’horloge de la base de données primaire.  Cette valeur est disponible uniquement sur la base de données primaire.|  
|replication_state|**tinyint**|État de la géo-réplication pour cette base de données, parmi les suivants :<br /><br /> 1 = amorçage. La cible de géo-réplication est amorcée, mais les deux bases de données ne sont pas encore synchronisées. Tant que l’amorçage n’est pas terminé, vous ne pouvez pas vous connecter à la base de données secondaire. La suppression de la base de données secondaire du réplica principal annule l’opération d’amorçage.<br /><br /> 2 = rattrapage. La base de données secondaire est dans un état cohérent au niveau transactionnel et est constamment synchronisée avec la base de données primaire.<br /><br /> 4 = suspendu. Il ne s'agit pas d'une relation de copie continue active. Cet état indique généralement que la bande passante disponible pour l'interlien est insuffisante pour le niveau d'activité de transaction dans la base de données primaire. Toutefois, la relation de copie continue est toujours intacte.|  
|replication_state_desc|**nvarchar (256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|rôle|**tinyint**|Rôle de géo-réplication, parmi les suivants :<br /><br /> 0 = principal. Le database_id fait référence à la base de données primaire dans le partenariat de géo-réplication.<br /><br /> 1 = secondaire.  Le database_id fait référence à la base de données primaire dans le partenariat de géo-réplication.|  
|role_desc|**nvarchar (256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Le type secondaire, parmi les suivants :<br /><br /> 0 = aucune connexion directe n’est autorisée dans la base de données secondaire et la base de données n’est pas disponible pour l’accès en lecture.<br /><br /> 2 = toutes les connexions sont autorisées à la base de données dans le réplica secondaire REPL ; ication pour l’accès en lecture seule.|  
|secondary_allow_connections_desc|**nvarchar (256)**|Non<br /><br /> Tous|  
|last_commit|**datetimeoffset**|Heure de la dernière transaction validée dans la base de données. S’il est récupéré sur la base de données primaire, il indique l’heure de la dernière validation sur la base de données primaire. S’il est récupéré sur la base de données secondaire, il indique l’heure de la dernière validation sur la base de données secondaire. En cas de récupération sur la base de données secondaire lorsque le réplica principal du lien de réplication est arrêté, il indique jusqu’à quel point la base de données secondaire a été capturée.|
  
> [!NOTE]  
>  Si la relation de réplication se termine en supprimant la base de données secondaire (section 4,2), la ligne de cette base de données dans la vue **sys.dm_geo_replication_link_status** disparaît.  
  
## <a name="permissions"></a>Autorisations  
 Tout compte avec view_database_state autorisation peut interroger **sys.dm_geo_replication_link_status**.  
  
## <a name="example"></a>Exemple  
 Affichez les retards de réplication et l’heure de la dernière réplication de mes bases de données secondaires.  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Azure SQL Database&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.geo_replication_links &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [sys.dm_operation_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)

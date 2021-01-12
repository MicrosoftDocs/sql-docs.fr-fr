---
description: sys.geo_replication_links (Azure SQL Database)
title: sys.geo_replication_links (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_links_TSQL
- dm_geo_replication_links
- sys.dm_geo_replication_links
- sys.dm_geo_replication_links_TSQL
helpviewer_keywords:
- sys.dm_geo_replication_links dynamic management view
- dm_geo_replication_links dynamic management view
ms.assetid: 58911798-1d60-4f28-87ab-2def2bfc3de7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: f10f318ebba5627957f1555b5979e8993f6f5e02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093898"
---
# <a name="sysgeo_replication_links-azure-sql-database"></a>sys.geo_replication_links (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contient une ligne pour chaque lien de réplication entre les bases de données primaires et secondaires dans un partenariat de géo-réplication. Cette vue se trouve dans la base de données master logique.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données active dans la vue sys. databases.|  
|start_date|**datetimeoffset**|Heure UTC dans un centre de données de SQL Database régional lorsque la réplication de base de données a été lancée.|  
|modify_date|**datetimeoffset**|Heure UTC à laquelle la géo-réplication de la base de données est effectuée au niveau régional SQL Database Datacenter. La nouvelle base de données est synchronisée avec la base de données primaire à ce moment-là.|  
|link_guid|**uniqueidentifier**|ID unique du lien de géo-réplication.|  
|partner_server|**sysname**|Nom du serveur de SQL Database qui contient la base de données géo-répliquée.|  
|partner_database|**sysname**|Nom de la base de données géo-répliquée sur le serveur de SQL Database lié.|  
|replication_state|**tinyint**|État de la géo-réplication pour cette base de données, parmi les suivants :<br /><br /> 0 = en attente. La création de la base de données secondaire active est planifiée, mais les étapes de préparation nécessaires ne sont pas encore terminées.<br /><br /> 1 = amorçage. La cible de géo-réplication est amorcée, mais les deux bases de données ne sont pas encore synchronisées. Tant que l’amorçage n’est pas terminé, vous ne pouvez pas vous connecter à la base de données secondaire. La suppression de la base de données secondaire du réplica principal annule l’opération d’amorçage.<br /><br /> 2 = rattrapage. La base de données secondaire est dans un état cohérent au niveau transactionnel et est constamment synchronisée avec la base de données primaire.|  
|replication_state_desc|**nvarchar (256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|rôle|**tinyint**|Rôle de géo-réplication, parmi les suivants :<br /><br /> 0 = principal. Le database_id fait référence à la base de données primaire dans le partenariat de géo-réplication.<br /><br /> 1 = secondaire.  Le database_id fait référence à la base de données primaire dans le partenariat de géo-réplication.|  
|role_desc|**nvarchar (256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Le type secondaire, parmi les suivants :<br /><br /> 0 = Non. La base de données secondaire n’est pas accessible avant le basculement.<br /><br /> 1 = ReadOnly. La base de données secondaire est accessible uniquement aux connexions clientes avec ApplicationIntent = ReadOnly.<br /><br /> 2 = Toutes. La base de données secondaire est accessible à toutes les connexions clientes.|  
|secondary_allow_connections _desc|**nvarchar (256)**|Non<br /><br /> Tous<br /><br /> Lecture seule|  
  
## <a name="permissions"></a>Autorisations

Cette vue est disponible uniquement dans la base de données **Master** à la connexion du principal au niveau du serveur.  
  
## <a name="example"></a>Exemple

Affiche toutes les bases de données avec des liens de géo-réplication.  

```sql
SELECT
     database_id  
   , start_date  
   , partner_server  
   , partner_database  
   , replication_state  
   , role_desc  
   , secondary_allow_connections_desc
FROM sys.geo_replication_links;  
```

## <a name="see-also"></a>Voir aussi

 [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.dm_geo_replication_link_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.dm_operation_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)

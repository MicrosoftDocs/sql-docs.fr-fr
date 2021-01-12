---
description: sys.dm_xe_database_session_object_columns (Azure SQL Database)
title: sys.dm_xe_database_session_object_columns
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 0e6adc54-4d97-4ef0-bf4f-b4538d69f136
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 0831d3c4f6634e1b7c5ae2568d853567eeaf32b0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093943"
---
# <a name="sysdm_xe_database_session_object_columns-azure-sql-database"></a>sys.dm_xe_database_session_object_columns (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Indique les valeurs de configuration d'objets liés à une session.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 et à toutes les versions ultérieures.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Adresse mémoire de la session d'événements. A une relation plusieurs-à-un avec sys.dm_xe_database_sessions. Address. N'accepte pas la valeur NULL.|  
|column_name|**nvarchar(60)**|Nom de la valeur de configuration. N'accepte pas la valeur NULL.|  
|column_id|**int**|ID de la colonne. Unique dans l'objet. N'accepte pas la valeur NULL.|  
|column_value|**nvarchar(2048)**|Valeur configurée de la colonne. Autorise la valeur NULL.|  
|object_type|**nvarchar(60)**|Type de l'objet.  N’est pas nullable.object_type est l’un des suivants :<br /><br /> événement<br /><br /> target|  
|object_name|**nvarchar(60)**|Nom de l'objet auquel appartient cette colonne. N'accepte pas la valeur NULL.|  
|object_package_guid|**uniqueidentifier**|GUID du package qui contient l'objet. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_package_guid|sys.dm_xe_objects.package_guid<br /><br /> sys.dm_xe_objects.name|Plusieurs-à-un|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_id|sys.dm_xe_object_columns.name<br /><br /> sys.dm_xe_object_columns.column_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  

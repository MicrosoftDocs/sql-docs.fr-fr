---
description: sys.dm_xe_session_object_columns (Transact-SQL)
title: sys.dm_xe_session_object_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xe_session_object_columns_TSQL
- sys.dm_xe_session_object_columns_TSQL
- dm_xe_session_object_columns
- sys.dm_xe_session_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- xe
- sys.dm_xe_session_object_columns dynamic management view
ms.assetid: e97f3307-2da6-4c54-b818-a474faec752e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8a7cf766e172d0cb62a3051894be390b176361c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99128618"
---
# <a name="sysdm_xe_session_object_columns-transact-sql"></a>sys.dm_xe_session_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indique les valeurs de configuration d'objets liés à une session.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Adresse mémoire de la session d'événements. A une relation plusieurs-à-un avec sys.dm_xe_sessions.address. N'accepte pas la valeur NULL.|  
|column_name|**nvarchar (256)**|Nom de la valeur de configuration. N'accepte pas la valeur NULL.|  
|column_id|**int**|ID de la colonne. Unique dans l'objet. N'accepte pas la valeur NULL.|  
|column_value|**nvarchar (3072)**|Valeur configurée de la colonne. Autorise la valeur NULL.|  
|object_type|**nvarchar(60)**|Type de l'objet. N'accepte pas la valeur NULL. object_type est l’un des éléments suivants :<br /><br /> événement<br /><br /> target|  
|object_name|**nvarchar (256)**|Nom de l'objet auquel appartient cette colonne. N'accepte pas la valeur NULL.|  
|object_package_guid|**uniqueidentifier**|GUID du package qui contient l'objet. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relation|  
|----------|--------|------------------|  
|dm_xe_session_object_columns dm_xe_session_object_columns.object_name,<br /><br /> dm_xe_session_object_columns.object_package_guid|sys.dm_xe_objects sys.dm_xe_objects.package_guid,<br /><br /> sys.dm_xe_objects.name|Plusieurs-à-un|  
|dm_xe_session_object_columns dm_xe_session_object_columns.column_name,<br /><br /> dm_xe_session_object_columns.column_id|sys.dm_xe_object_columns. Name,<br /><br /> sys.dm_xe_object_columns.column_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  


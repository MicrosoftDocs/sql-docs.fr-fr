---
description: sys.dm_xe_map_values (Transact-SQL)
title: sys. dm_xe_map_values (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_map_values
- dm_xe_map_values
- dm_xe_map_values_TSQL
- sys.dm_xe_map_values_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_map_values dynamic management view
- xe
ms.assetid: c0c5dd7e-9cee-47e2-b65a-88194c00aa1f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 235f6d9bf0d84985d9749f9cae5ee2b93460feae
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89546355"
---
# <a name="sysdm_xe_map_values-transact-sql"></a>sys.dm_xe_map_values (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne un mappage des clés numériques internes à du texte explicite.  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|name|**nvarchar (256)**|Nom du mappage. le nom est unique dans le système local. N'accepte pas la valeur NULL.|  
|object_package_guid|**uniqueidentifier**|GUID du package qui contient le mappage. N'accepte pas la valeur NULL.|  
|map_key|**int**|Valeur de clé interne. N'accepte pas la valeur NULL.|  
|map_value|**nvarchar (3072)**|Description de la valeur de clé. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relation|  
|----------|--------|------------------|  
|dm_xe_map_values.object_package_guid<br /><br /> dm_xe_map_values.name|sys.dm_xe_objects.package_guid<br /><br /> sys.dm_xe_objects.name|Plusieurs-à-un| 
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  


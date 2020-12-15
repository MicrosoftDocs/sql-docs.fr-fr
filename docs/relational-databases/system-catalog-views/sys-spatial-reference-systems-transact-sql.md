---
description: sys.spatial_reference_systems (Transact-SQL)
title: sys.spatial_reference_systems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_reference_systems_TSQL
- sys.spatial_reference_systems_TSQL
- sys.spatial_reference_systems
- spatial_reference_systems
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_reference_systems catalog view
- spatial_reference_systems
ms.assetid: 3c9bc120-67c3-463f-9e24-29fd623f25a0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9df5c147ae41d915cede5d3c89628c0b5baebb91
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97428884"
---
# <a name="sysspatial_reference_systems-transact-sql"></a>sys.spatial_reference_systems (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Répertorie les systèmes de référence spatiale (SRID) pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|spatial_reference_id|**int**|SRID pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|authority_name|**nvarchar(128)**|Autorité du SRID.|  
|authorized_spatial_reference_id|**int**|SRID donné par l’autorité nommée dans **authority_name**.|  
|well_known_text|**nvarchar(4000)**|Représentation WKT du SRID.|  
|unit_of_measure|**nvarchar(128)**|Nom de l'unité de mesure.|  
|unit_conversion_factor|**float**|Longueur de l'unité de mesure en mètres.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
  

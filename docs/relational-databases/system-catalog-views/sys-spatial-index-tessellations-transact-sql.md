---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddf7b9cd75999da6cc48f6e7a83bab7a733f822b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095415"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Représente les informations sur le schéma de pavage et les paramètres de chaque index spatial.  
  
> [!NOTE]  
>  Pour plus d’informations sur le pavage, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de l'objet sur lequel l'index est défini. Chaque paire (object_id, index_id) a une entrée correspondante dans [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|ID de l'index spatial où la colonne indexée est définie.|  
|tessellation_scheme|**sysname**|Nom du schéma de pavage, l’un des éléments suivants : GEOMETRY_GRID, GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|Coordonnée x de l’angle inférieur gauche du cadre englobant, l’une des valeurs suivantes : NULL = non applicable pour un schéma de pavage donné (par exemple GEOGRAPHY_GRID) *n* = Si tessellation_scheme est GEOMETRY_GRID, la valeur de la coordonnée x-min.                     **Remarque :** Les coordonnées définies par les paramètres du cadre englobant sont interprétées pour chaque objet en fonction de son [identificateur de référence spatiale (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).|  
|bounding_box_ymin|**float(53)**|Coordonnée Y de l’angle inférieur gauche du cadre englobant, l’une des valeurs suivantes : NULL = non applicable pour un schéma de pavage donné (par exemple GEOGRAPHY_GRID) *n* = Si tessellation_scheme est GEOMETRY_GRID, la valeur de la coordonnée y-min|  
|bounding_box_xmax|**float(53)**|Coordonnée x de l’angle supérieur droit du cadre englobant, l’une des valeurs suivantes : NULL = non applicable pour un schéma de pavage donné (par exemple GEOGRAPHY_GRID) *n* = Si tessellation_scheme est GEOMETRY_GRID, la valeur de coordonnée x-max|  
|bounding_box_ymax|**float(53)**|Coordonnée Y de l’angle supérieur droit du cadre englobant, l’une des valeurs suivantes : NULL = non applicable pour un schéma de pavage donné (par exemple GEOGRAPHY_GRID) *n* = Si tessellation_scheme est GEOMETRY_GRID, la valeur de coordonnée y-max|  
|level_1_grid|**smallint**|Densité de la grille pour la grille de niveau supérieur. Si tessellation_scheme est GEOMETRY_GRID ou GEOGRAPHY_GRID, l’un des éléments suivants est : 16 = 4 sur 4 Grid (faible) 64 = 8 sur 8 grille (moyenne) 256 = 16 par 16 grille (haute) NULL = non applicable au type d’index spatial ou au schéma de pavage donné.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_1_grid_desc|**nvarchar(60)**|Densité de la grille pour la grille de niveau supérieur, une des valeurs suivantes : faible moyenne élevée NULL = non applicable pour le type d’index spatial donné ou le schéma de pavage.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_2_grid|**smallint**|Densité de la grille pour la grille de 2e niveau. Si tessellation_scheme est GEOMETRY_GRID ou GEOGRAPHY_GRID, l’un des éléments suivants est : 16 = 4 sur 4 Grid (faible) 64 = 8 sur 8 grille (moyenne) 256 = 16 par 16 grille (haute) NULL = non applicable au type d’index spatial ou au schéma de pavage donné.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_2_grid_desc|**nvarchar(60)**|Densité de la grille pour la grille de 2e niveau, une des valeurs suivantes : faible moyenne élevée NULL = non applicable pour le type d’index spatial donné ou le schéma de pavage.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_3_grid|**smallint**|Densité de la grille pour la grille de 3e niveau.   Si tessellation_scheme est GEOMETRY_GRID ou GEOGRAPHY_GRID, l’un des éléments suivants est : 16 = 4 sur 4 Grid (faible) 64 = 8 sur 8 grille (moyenne) 256 = 16 par 16 grille (haute) NULL = non applicable au type d’index spatial ou au schéma de pavage donné.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_3_grid_desc|**nvarchar(60)**|Densité de la grille pour la grille de troisième niveau, une des valeurs suivantes : faible moyenne élevée NULL = non applicable pour le type d’index spatial donné ou le schéma de pavage.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_4_grid|**smallint**|Densité de la grille pour la grille de 4e niveau. Si tessellation_scheme est GEOMETRY_GRID ou GEOGRAPHY_GRID, l’un des éléments suivants est : 16 = 4 sur 4 Grid (faible) 64 = 8 sur 8 grille (moyenne) 256 = 16 par 16 grille (haute) NULL = non applicable au type d’index spatial ou au schéma de pavage donné.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_4_grid_desc|**nvarchar(60)**|Densité de la grille pour la grille de 4e niveau, une des suivantes : < faible moyenne élevée NULL = non applicable pour le type d’index spatial donné ou le schéma de pavage.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|cells_per_object|**int**|Nombre de cellules par objet spatial, l’une des valeurs suivantes : Si tessellation_scheme est GEOMETRY_GRID ou GEOGRAPHY_GRID, *n* = nombre de cellules par objet null = non applicable pour le type d’index spatial donné ou le schéma de pavage|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  

---
description: sys. pdw_table_distribution_properties (Transact-SQL)
title: sys. pdw_table_distribution_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 639a7475-7c92-41e0-a8ab-ad630eb5aea3
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 580ce1c29f8c788e23cac3d2c78edf164118cb6d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88475327"
---
# <a name="syspdw_table_distribution_properties-transact-sql"></a>sys. pdw_table_distribution_properties (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient les informations de distribution pour les tables.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|**object_id**|**int**|ID de la table pour laquelle les propriétés sont spécifiées.||  
|**distribution_policy**|**tinyint**|0 = NON DÉFINI<br /><br /> 1 = AUCUN<br /><br /> 2 = HACHAGE<br /><br /> 3 = RÉPLIQUER<br /><br /> 4 = ROUND_ROBIN||  
|**distribution_policy_desc**|**nvarchar(60)**|NON DÉFINI, AUCUN, HACHAGE, RÉPLICATION, ROUND_ROBIN|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] retourne HASH, ROUND_ROBIN ou replicate.|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue SQL Data Warehouse et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

---
description: sys.pdw_column_distribution_properties (Transact-SQL)
title: sys.pdw_column_distribution_properties (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 46b74f99-2e22-4dbd-872a-533fce0e239c
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d812be285cb2a89aaa6f83db2d08312dfed939af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191218"
---
# <a name="syspdw_column_distribution_properties-transact-sql"></a>sys.pdw_column_distribution_properties (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient les informations de distribution pour les colonnes.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|**object_id**|**int**|ID de l’objet auquel la colonne appartient.||  
|**column_id**|**int**|Identificateur de la colonne.||  
|**distribution_ordinal**|**tinyint**|Ordinal (de base 1) au sein d’un ensemble de distribution.|0 = n’est pas une colonne de distribution. 1 = [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] utilise cette colonne pour distribuer la table parente.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

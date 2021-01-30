---
description: sys.partition_functions (Transact-SQL)
title: sys.partition_functions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.partition_functions_TSQL
- partition_functions
- sys.partition_functions
- partition_functions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_functions catalog view
ms.assetid: 96515727-728b-4bea-804a-36ce915b8b75
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ea9b54bbdcffc6e43f313119365da189aec49c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191246"
---
# <a name="syspartition_functions-transact-sql"></a>sys.partition_functions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par fonction de partition dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la fonction de partition Unique dans la base de données.|  
|**function_id**|**int**|ID de la fonction de partition. Unique dans la base de données.|  
|**type**|**char(2)**|Type de fonction.<br /><br /> R = plage|  
|**type_desc**|**nvarchar(60)**|Type de fonction.<br /><br /> RANGE|  
|**sort**|**int**|Nombre de partitions créées par la fonction.|  
|**boundary_value_on_right**|**bit**|Pour le partitionnement par spécification de plages de valeurs.<br /><br /> 1 = la valeur limite est incluse dans la plage RIGHT de la limite.<br /><br /> 0 = LEFT.|  
|**is_system**||**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> 1 = l'objet est utilisé pour les fragments d'index de recherche en texte intégral.<br /><br /> 0 = l'objet n'est pas utilisé pour les fragments d'index de recherche en texte intégral.|  
|**create_date**|**datetime**|Date de création de la fonction.|  
|**modify_date**|**datetime**|Date de la dernière modification de la fonction à l'aide d'une instruction ALTER.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue des fonctions de partition &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)  
  
  

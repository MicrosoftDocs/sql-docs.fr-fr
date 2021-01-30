---
description: sys.partition_parameters (Transact-SQL)
title: sys.partition_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- partition_parameters_TSQL
- partition_parameters
- sys.partition_parameters_TSQL
- sys.partition_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_parameters catalog view
ms.assetid: 2012ed9d-3ea3-4c29-9b78-dfa54a392dce
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d750939c6d2a99e695b894164c6af20953297bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203807"
---
# <a name="syspartition_parameters-transact-sql"></a>sys.partition_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque paramètre d'une fonction de partition.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**function_id**|**int**|Identificateur de la fonction de partition à laquelle appartient ce paramètre.|  
|**parameter_id**|**int**|ID du paramètre. Unique au sein de la fonction de partition, en commençant par 1.|  
|**system_type_id**|**tinyint**|Identificateur du type système du paramètre. Correspond à la colonne **system_type_id** de l’affichage catalogue **sys. types** .|  
|**max_length**|**smallint**|Longueur maximale du paramètre, en octets.|  
|**precision**|**tinyint**|Précision du paramètre s'il est de type numérique ; sinon, 0.|  
|**scale**|**tinyint**|Échelle du paramètre s'il est de type numérique ; sinon, 0.|  
|**collation_name**|**sysname**|Nom du classement du paramètre s'il est constitué de caractères alphanumériques, sinon NULL.|  
|**user_type_id**|**int**|ID du type. Unique dans la base de données. Pour les types de données système, **user_type_id**  =  **system_type_id**.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue des fonctions de partition &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)  
  
  

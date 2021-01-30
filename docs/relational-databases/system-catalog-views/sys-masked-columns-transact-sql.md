---
description: sys.masked_columns (Transact-SQL)
title: sys.masked_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.masked_columns
- masked_columns_tsql
- sys.masked_columns_tsql
- masked_columns
helpviewer_keywords:
- sys.masked_columns catalog view
ms.assetid: 671577e4-d757-4b8d-9aa9-0fc8d51ea9ca
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9246c30c8f71d73d1def1bc54179b2b094a2c5bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191334"
---
# <a name="sysmasked_columns-transact-sql"></a>sys.masked_columns (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Utilisez la vue **sys.masked_columns** pour rechercher des colonnes de table auxquelles une fonction de masquage dynamique des données est appliquée. Celle-ci hérite de la vue **sys.columns** . Elle retourne toutes les colonnes de la vue **sys.columns** , ainsi que les colonnes **is_masked** et **masking_function** , en indiquant si les colonnes sont masquées et, dans ce cas, la fonction de masquage est définie. Cette vue présente uniquement les colonnes auxquelles une fonction de masquage est appliquée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|name|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|column_id|**int**|Identificateur de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|**sys.masked_columns** retourne beaucoup plus de colonnes héritées de **sys. Columns**.|divers|Pour plus d’informations sur les définitions de colonnes, consultez [sys. columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) .|  
|is_masked|**bit**|Indique si la colonne est masquée. 1 indique masqué.|  
|masking_function|**nvarchar(4000)**|Fonction de masquage de la colonne.|  
  
## <a name="remarks"></a>Notes  
  
## <a name="permissions"></a>Autorisations  
 Cette vue retourne des informations sur les tables pour lesquelles l’utilisateur dispose d’autorisations sur la table ou si l’utilisateur dispose de l’autorisation VIEW ANY DEFINITION.  
  
## <a name="example"></a>Exemple  
 La requête suivante joint **sys.masked_columns** à **sys. tables** pour renvoyer des informations sur toutes les colonnes masquées.  
  
```  
SELECT tbl.name as table_name, c.name AS column_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.object_id = tbl.object_id  
WHERE is_masked = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Dynamic Data Masking](../../relational-databases/security/dynamic-data-masking.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  

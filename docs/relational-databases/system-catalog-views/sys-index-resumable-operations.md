---
description: sys.index_resumable_operations (Transact-SQL)
title: sys.index_resumable_operations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.index_resumable_operations_TSQL
- sys.indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes
- sys.index_resumable_operations
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97b02afea0dc4756783ad5197a24b5ca5d952282
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094524"
---
# <a name="sysindex_resumable_operations-transact-sql"></a>sys.index_resumable_operations (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]
**sys.index_resumable_operations** est une vue système qui surveille et vérifie l’état d’exécution actuel de la reconstruction ou de la création d’index pouvant être reprise.  
**S’applique à**: SQL Server (2017 et versions ultérieures) et Azure SQL Database
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l’objet auquel cet index appartient (non Nullable).|  
|**index_id**|**int**|ID de l’index (non Nullable). **index_id** n’est unique que dans l’objet.|
|**name**|**sysname**|Nom de l'index. le **nom** est unique dans l’objet.|  
|**sql_text**|**nvarchar(max)**|Texte de l’instruction T-SQL DDL|
|**last_max_dop**|**smallint**|Dernier MAX_DOP utilisé (valeur par défaut = 0)|
|**partition_number**|**int**|Numéro de partition dans l’index ou le segment de mémoire propriétaire. Pour les tables et les index non partitionnés, ou si toutes les partitions sont en cours de reconstruction, la valeur de cette colonne est NULL.|
|**state**|**tinyint**|État opérationnel de l’index pouvant être repris :<br /><br />0 = en cours d’exécution<br /><br />1 = pause|
|**state_desc**|**nvarchar(60)**|Description de l’état opérationnel de l’index pouvant être repris (en cours d’exécution ou en pause)|  
|**heure-début**|**datetime**|Heure de début de l’opération d’index (non Nullable)|
|**last_pause_time**|**datatime**| Heure de la dernière suspension de l’opération d’index (Nullable). NULL si l’opération est en cours d’exécution et n’est jamais suspendue.|
|**total_execution_time**|**int**|Durée d’exécution totale à partir de l’heure de début en minutes (non Nullable)|
|**percent_complete**|**real**|Progression de l’opération d’index en% (non Nullable).|
|**page_count**|**bigint**|Nombre total de pages d’index allouées par l’opération de génération d’index pour les index nouveaux et de mappage (non Nullable).

## <a name="permissions"></a>Autorisations

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="example"></a>Exemple

 Répertorie toutes les opérations de création ou de régénération d’index pouvant être reprise qui sont dans l’État PAUSE.

```sql
SELECT * FROM  sys.index_resumable_operations WHERE STATE = 1;  
```

## <a name="see-also"></a>Voir aussi

- [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [Affichages catalogue](catalog-views-transact-sql.md)
- [Affichages catalogue d’objets](object-catalog-views-transact-sql.md)
- [sys.indexes](sys-xml-indexes-transact-sql.md)
- [sys.index_columns](sys-index-columns-transact-sql.md)
- [sys.xml_indexes](sys-xml-indexes-transact-sql.md)
- [sys.objects](sys-index-columns-transact-sql.md)
- [sys.key_constraints](sys-key-constraints-transact-sql.md)
- [sys.filegroups](sys-filegroups-transact-sql.md)
- [sys.partition_schemes](sys-partition-schemes-transact-sql.md)
- [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](querying-the-sql-server-system-catalog-faq.md)

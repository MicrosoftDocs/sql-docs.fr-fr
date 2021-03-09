---
description: sys.views (Transact-SQL)
title: sys. views (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- views_TSQL
- views
- sys.views_TSQL
- sys.views
dev_langs:
- TSQL
helpviewer_keywords:
- sys.views catalog view
ms.assetid: f8a8ea39-5a09-4662-801e-b43519467def
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 33b0001ef88110215a82a35a1a49f1000392d618
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464270"
---
# <a name="sysviews-transact-sql"></a>sys.views (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque objet de vue, avec **sys. Objects. type** = V.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|  
|**is_replicated**|**bit**|1 = La vue est répliquée.|  
|**has_replication_filter**|**bit**|1 = La vue comporte un filtre de réplication.|  
|**has_opaque_metadata**|**bit**|1 = L'option VIEW_METADATA est spécifiée pour la vue. Pour plus d’informations, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).|  
|**has_unchecked_assembly_data**|**bit**|1 = La vue contient des données persistantes qui dépendent d'un assembly dont la définition a changé au cours de la dernière instruction ALTER ASSEMBLY. Est réinitialisé à 0 après la dernière instruction DBCC CHECKDB ou DBCC CHECKTABLE réussie.|  
|**with_check_option**|**bit**|1 = WITH CHECK OPTION est spécifié dans la définition de la vue.|  
|**is_date_correlation_view**|**bit**|1 = Le système a créé automatiquement la vue pour stocker les informations de corrélation entre les colonnes de date/heure (datetime). L'attribution de la valeur ON à l'option DATE_CORRELATION_OPTIMIZATION a permis la création de cette vue.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

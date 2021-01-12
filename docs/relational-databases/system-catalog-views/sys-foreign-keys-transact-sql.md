---
description: sys.foreign_keys (Transact-SQL)
title: sys.foreign_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba0494d8ca1532f02e5f0cde92eef18d9a7d0639
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098029"
---
# <a name="sysforeign_keys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par objet qui est une contrainte de clé étrangère, avec **sys. Object. type** = F.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**referenced_object_id**|**int**|ID de l'objet référencé.|  
|**key_index_id**|**int**|ID de l'index de clé dans l'objet référencé.|  
|**is_disabled**|**bit**|La contrainte FOREIGN KEY est désactivée.|  
|**is_not_for_replication**|**bit**|La contrainte FOREIGN KEY a été créée à l'aide de l'option NOT FOR REPLICATION.|  
|**is_not_trusted**|**bit**|Le système n'a pas vérifié la contrainte FOREIGN KEY.|  
|**delete_referential_action**|**tinyint**|Action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une suppression a lieu.<br /><br /> 0 = Pas d'action<br /><br /> 1 = Cascade<br /><br /> 2 = Définir avec une valeur NULL<br /><br /> 3 = Définir avec une valeur par défaut|  
|**delete_referential_action_desc**|**nvarchar(60)**|Description de l'action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une suppression a lieu :<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|Action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une mise à jour a lieu.<br /><br /> 0 = Pas d'action<br /><br /> 1 = Cascade<br /><br /> 2 = Définir avec une valeur NULL<br /><br /> 3 = Définir avec une valeur par défaut|  
|**update_referential_action_desc**|**nvarchar(60)**|Description de l'action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une mise à jour a lieu.<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = Le nom a été créé par le système.<br /><br /> 0 = Le nom a été fourni par l'utilisateur.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  

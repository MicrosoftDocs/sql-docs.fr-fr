---
description: sys. periods (Transact-SQL)
title: sys. periods (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 25e66ed3-2270-4c5c-9f5a-2c0f165a57ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0e74808f16b7b287544d11d209d4da94d8485b3b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463847"
---
# <a name="sysperiods-transact-sql"></a>sys. periods (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne une ligne pour chaque table pour laquelle des périodes ont été définies.  
  
|En-tête de colonne|Type de données|Description|  
|-------------------|---------------|-----------------|  
|name|**sysname**|Nom de la période|  
|period_type|**tinyint**|Valeur numérique représentant le type de période :<br /><br /> 1 = période système|  
|period_type_desc|**nvarchar(60)**|Description textuelle du type de colonne :<br /><br /> SYSTEM_TIME_PERIOD|  
|object_id|**int**|ID de la table contenant la colonne period_type|  
|start_column_id|**int**|ID de la colonne qui définit la limite inférieure de la période|  
|end_column_id|**int**|ID de la colonne qui définit la limite supérieure de la période|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)  
  

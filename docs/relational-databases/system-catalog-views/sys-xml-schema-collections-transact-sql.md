---
description: sys.xml_schema_collections (Transact-SQL)
title: sys.xml_schema_collections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_collections_TSQL
- sys.xml_schema_collections
- xml_schema_collections
- xml_schema_collections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_collections catalog view
ms.assetid: f3f7f3dc-029f-4942-ab3c-75fa9814e40f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9fb972784e2e05d60bc5df687b80aa561a9536e1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464051"
---
# <a name="sysxml_schema_collections-transact-sql"></a>sys.xml_schema_collections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne par collection de schémas XML. Une collection de schémas XML est un ensemble nommé de définitions XSD. La collection de schémas XML elle-même est contenue dans un schéma relationnel et identifiée par un nom [!INCLUDE[tsql](../../includes/tsql-md.md)] d'étendue sur le schéma. Les tuples suivants sont uniques : xml_collection_id et schema_id et  name.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|xml_collection_id|**int**|ID de la collection de schémas XML. Unique dans la base de données.|  
|schema_id|**int**|ID du schéma relationnel qui contient cette collection de schémas XML.|  
|principal_id|**int**|ID du propriétaire individuel, s’il est différent du propriétaire du schéma. Par défaut, le propriétaire du schéma détient les objets contenus dans le schéma. Toutefois, un autre propriétaire peut être spécifié à l’aide de l’instruction ALTER AUTHORIZation pour modifier la propriété.<br /><br /> NULL = Pas d'autre propriétaire individuel.|  
|name|**sysname**|Nom de la collection de schémas XML.|  
|create_date|**datetime**|Date de création de la collection de schémas XML.|  
|modify_date|**datetime**|Date de dernière modification de la collection de schémas XML.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;les affichages catalogue du système de type XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

---
description: VIEW_COLUMN_USAGE (Transact-SQL)
title: VIEW_COLUMN_USAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- VIEW_COLUMN_USAGE
- VIEW_COLUMN_USAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.VIEW_COLUMN_USAGE view
- VIEW_COLUMN_USAGE view
ms.assetid: fc0b3608-a7e8-4532-8215-32235d6670f1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f72b7a0555615a3bb4852ef61e5bdb62f98eeca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190075"
---
# <a name="view_column_usage-transact-sql"></a>VIEW_COLUMN_USAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque colonne de la base de données active qui est utilisée dans la définition d'une vue. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**VIEW_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la vue.|  
|**VIEW_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la vue.<br /><br /> **&#42;&#42; Important &#42;&#42;**  seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**VIEW_NAME**|**sysname**|Nom de la vue.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la table.<br /><br /> **&#42;&#42; Important &#42;&#42;**  seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**TABLE_NAME**|**sysname**|Table de base.|  
|**COLUMN_NAME**|**sysname**|Nom de la colonne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sql_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  

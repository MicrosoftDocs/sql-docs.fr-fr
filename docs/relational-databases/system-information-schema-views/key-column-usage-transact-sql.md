---
description: KEY_COLUMN_USAGE (Transact-SQL)
title: KEY_COLUMN_USAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- KEY_COLUMN_USAGE_TSQL
- KEY_COLUMN_USAGE
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.KEY_COLUMN_USAGE view
- KEY_COLUMN_USAGE view
ms.assetid: ec1e18c2-63a1-4d2b-ba9a-c13857403782
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 922b9819d722f76d8d962fe85688e26f263b7d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189663"
---
# <a name="key_column_usage-transact-sql"></a>KEY_COLUMN_USAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque colonne pour laquelle une contrainte de clé est définie dans la base de données active. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la contrainte.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la contrainte.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. <strong> \* \* \* \* </strong> Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**CONSTRAINT_NAME**|**nvarchar (** 128 **)**|Nom de la contrainte.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la table.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. <strong> \* \* \* \* </strong> Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nom de la table.|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Nom de la colonne.|  
|**ORDINAL_POSITION**|**int**|Position ordinale de la colonne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.foreign_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)   
 [sys.key_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)  
  
